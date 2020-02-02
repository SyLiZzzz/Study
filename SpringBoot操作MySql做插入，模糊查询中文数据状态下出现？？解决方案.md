# SpringBoot操作MySql做插入，模糊查询中文数据状态下出现？？解决方案

**笔者是在阿里云上的docker镜像下的mysql。在排除参数传入时就错误的情况下，最终找到是数据库的编码问题。**

首先，排除编辑器编码问题，因为参数传入是中文，SpringBoot日志记录SQL语句，参数为中文。没出现乱码

```java
2020-01-24 18:28:37.757 DEBUG 22364 --- [nio-8080-exec-2] com.demo.bms.dao.UserDao.selectPageInfo  : ==>  Preparing: select user_id,account,sex,password,phone,create_time,is_deleted,user_role_id from t_user where account like concat('%',?,'%') 
2020-01-24 18:28:37.883 DEBUG 22364 --- [nio-8080-exec-2] com.demo.bms.dao.UserDao.selectPageInfo  : ==> Parameters: 尹(String)
2020-01-24 18:28:37.979 DEBUG 22364 --- [nio-8080-exec-2] com.demo.bms.dao.UserDao.selectPageInfo  : <==      Total: 0
```

但是返回的total为0条数据，但是数据库存在。

使用Navicat 检查数据库编码

```sql
show VARIABLES like 'char%'
```

显示如下

```
character_set_client	utf8mb4
character_set_connection	utf8mb4
character_set_database	utf8
character_set_filesystem	binary
character_set_results	utf8mb4
character_set_server	不是UTF-8
character_set_system	utf8
character_sets_dir	/usr/share/mysql/charsets/
```

修改docker中的mysql配置文件

命令如下

```shell
# 查看所有镜像文件
docker images 
# 查看所有容器
docker ps 
```

进入msql容器

```shell
docker exec -it 机器码 /bin/bash
```

进入配置文件目录

```shell
cd /etc/mysql/
```

下载vim

apt-get install vim  出现报错

```shell
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package vim
```

因为要同步 /etc/apt/sources.list 和 /etc/apt/sources.list.d 中列出的源的索引，这样才能获取到最新的软件包。输入命令apt-get update，运行完成后在重新install

打开配置文件

vim my.cnf 有部分配置文件名字不同请斟酌

在[mysqld]下插入如下代码

```shell
character-set-server=utf8
```

exit退出当前模式

停止并重启运行的mysql

```shell
docker stop 机器码
docker start 机器码
```

在Navicat中重新查看编码格式。显示如下

```
character_set_client	utf8mb4
character_set_connection	utf8mb4
character_set_database	utf8
character_set_filesystem	binary
character_set_results	utf8mb4
character_set_server	utf8
character_set_system	utf8
character_sets_dir	/usr/share/mysql/charsets/
```

重新请求接口，查看sql代码与返回total

```java
2020-01-24 18:50:17.051 DEBUG 20836 --- [nio-8080-exec-1] com.demo.bms.dao.UserDao.selectPageInfo  : ==>  Preparing: select user_id,account,sex,password,phone,create_time,is_deleted,user_role_id from t_user where account like concat('%',?,'%') 
2020-01-24 18:50:17.223 DEBUG 20836 --- [nio-8080-exec-1] com.demo.bms.dao.UserDao.selectPageInfo  : ==> Parameters: 尹(String)
2020-01-24 18:50:17.311 DEBUG 20836 --- [nio-8080-exec-1] com.demo.bms.dao.UserDao.selectPageInfo  : <==      Total: 1
```

**解决成功！！！！**

