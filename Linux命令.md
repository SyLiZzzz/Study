# 1.关机、重启、用户登录注销

1. **shutdown** :

   ​					shutdown -h now :立即关机							shutdwon -h 1:一分钟后关机

   ​					shutdown -r now :立即重启						其中：halt--直接使用     reboot--重启系统 

2. **logout**：注销用户

# 2.用户管理

- ##### 用户

1. 添加用户 ：**useradd** []  用户名		当创建用户成功后，会自动创建和用户名相同的home/目录，也可通过useradd -d 指定目录 更换新的home/目录
2. 修改密码 ：**passwd** 用户名
3. 删除用户 ：**userdel** 用户名       删除用户xx以及主目录   userdel -r xx    (在删除用户时，一般不会删除家目录) 
4. 查找用户 ：**id**   '用户名'   
5. 切换用户： **su**-  '用户名'    **注**：从高权限切换至低权限时不用输入密码，反之需要。 需返回原来用户时，使用exit.

- ##### 用户组

1. 增加组：**groupadd**  '组名'
2. 删除组：**groupdel**   '组名'
3. 创建用户时指定组：**useradd -g**  '组名'  '用户名'
4. 修改用户组 ：**usermod -g**  '组名'  '用户名'

# 3.实用指定

- ##### 运行级别：

  0：关机      1：单用户[找回丢失密码]     2：多用户状态没有网络服务    3：多用户状态有网路服务

  4：系统未使用保留给用户    5：图形界面     6：系统重启

- ##### **切换指定运行级别**

  **init**[0123456]

  **题目** ：找回root密码

​			开机-->在引导时 按下enter-->进入界面输入 e -->新界面，选中第二行 再输入e -->在这行最后输入1，再按下enter-->再输入b就会进入单用户模式        通过单用户模式使用passwd修改root密码

- ##### 帮助指令

1. **man ls** ：查看帮助信息
2. **help** [命令] ：查看[命令]的帮助信息

- ##### 文件目录类

1. **pwd** : 显示当前工作目录的绝对路径

2. **ls** :查看文件与目录

3. **ls -a** ：显示当前目录所有的文件和目录，包括隐藏的

4. **ls -l** :	以列表的方式显示信息

5. **cd** : 改变目录      cd ~  :切换回根目录    cd .. 切换回上一级目录

6. **mkdir** ：创建目录     mkdir -p ：创建多级目录

7. **rmdir** ：删除空目录   rm -rf ：删除非空目录

8. **touch** ：创建空文件   **如**：touch hello.txt

9. **cp** ：拷贝文件到指定目录    cp -r ：递归拷贝整个文件夹   **如**：cp -r   aaa/    bbb/   即拷贝aaa文件夹下所有文件到bbb文件夹下

10. **rm** ：移除文件或目录    rm -r ：递归删除整个文件夹      rm  -f ：强制删除不提示

11. **mv** ：移动文件夹与目录或重命名

    ​				mv  oldNameFile   newNameFile  (重命名)

    ​				mv   /temp/moveFile   /targetFolder   (移动文件)

12. **cat** ：查看文件内容(只读)    			cat -n  显示行号       **如** ： cat -n  /etc/profile | more

13. **more** ：以全屏幕的方式按页显示文本文件内容。     **如**：more /etc/profile

14. **less** ：分屏查看文件内容，比more强大 。    **如** ：less /etc/profile

15. 输出重定向：**>**  ， 追加 ：**>>**  。 输出重定向会将原文件的内容覆盖，而追加不会，而是追加至文件尾部         **如** ：ls -l  >  a.txt       ls -l  >>a.txt

16. **echo** ：输出内容至控制台    **如** ：echo $PATH

17. **head** ：显示文件开头部分内容，默认前10行。    head -n  5   :查看文件头5行内容   。 **如** ：head -n 5 /etc/profile

18. **tail**  ：显示文件尾部内容，默认后10行。    tail  -n  5 ：查看文件后5行内容，  tail  -f  监控文档变化。    **如** ：tail -f  mydata.txt

19. **ln** ：软连接   类似于Windows中的快捷方式， ln -s  [原文件或目录]   [软连接名]   给原文件创建一个软连接。    **如**  ：ln -s /root    linkToRoot

20. **history** ：查看已执行过的历史命令  。 history  10  ：显示最近使用过的10个指令。

21. **date**  ：显示当前时间  

    ​			date +%Y ：显示当前年份          date +%m  ：显示当前月份

    ​			date +%d ：显示当前天       	   date "+%Y-%m-%d %H:%M:%S" ：显示年月日时分秒

22. **cal**  ：查看日历 

    ​		find ：从指定目录向下递归遍历各个子目录，将满足条件的显示在终端。 

    ​		find [搜索范围] [选项]     ：-name     -user    -size      即按指定文件名查找文件，按指定用户名查找文件，按指定文件大小查找文件

    ​		**如** ：find /home -name hello.txt

23. **locate** ：快速指定文件路径。由于locate指令基于数据库进行查询，所以第一次运行前先要使用update指令创建locate数据库。

24. **grep** 与 **|** ：过滤查找  ，管道符'|'。  -n  显示行号，-i  忽略字母大小写。   **如**  ：cat hello.txt |grep -ni yes。

25. **gzip** /**gunzip** :压缩与解压 。  gzip  文件：只能压缩为*.gz文件   。gunzip  文件.gz  ：解压

26. **zip**/**unzip**  ：压缩与解压  。  zip [选项]  文件.zip   ，unzip  [选项]  文件.zip    。选项 :   -r递归压缩，-d<目录>  :指定解压后文件存放的目录。    **如**  ：zip -r  test.zip  /home/     将home目录下所有文件压缩成test.zip          |      unzip  -d  /opt/tmp/   test.zip

27. **tar**  ：打包。   打包后文件夹为.tar.gz 文件   。  选项  ： -c  -v  -f  -z  -x    即产生.tar打包文件，显示详细信息，指定压缩后的文件名，打包同时压缩，解压.tar文件![1563974622577](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1563974622577.png)

# 4.组管理

- **查看文件所有者**

  ​			ls -ahl   

- **修改文件所有者**

  ​			chown  用户名   文件名  			如  ：chown  tom   1.txt

- **组的创建**

  ​			groupadd 组名          如创建用户，并放入指定组中        useradd -g  teacher    tom

- **修改文件所在组**

  ​			chgrp  组名   文件名       **如** ：chgrp  A     1.txt

- **改变用户所在组**

  ​			usermod  -g 组名  用户名  			usermod  -d  目录名   用户名    改变该用户登陆的初始目录

# 5.权限管理

- **如  ：-rwxrw-r--  1  root  root  1213  Feb  2 09:39 abc**

- **0-9位说明**

1. 第0位     确定文件类型（d,-,l,c,b）
2. 第1-3位 确定所有者（该文件所有者） 拥有该文件的权限 ---User
3. 第4-6位 确定所属组（同用户组的）拥有该文件的权限  ---Group
4. 第7-9位  确定其他用户拥有该文件的权限 ---Other
5. ![1563975757976](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1563975757976.png)

- #### **rwx 详解** 

- **rwx作用至文件**

  ​	r ：可读(read) ：可读取，查看

  ​	w：可写(write)：可修改，但不可删除，删除的 条件是该文件所在的目录有写权限，才能删

  ​	x :   可执行(execute) ：可被执行

- **rwx作用至目录**

  ​	r ：可读(read) ：可读取，ls查看目录内容

  ​	w：可写(write)：可修改，目录内创建+删除+重命名目录

  ​	x :   可执行(execute) ：可进入该文件

- #### 文件及目录权限

- **如**  **：-rwxrw-r--  1  root  root  1213  Feb  2 09:39 abc**

1. 1字符代表文件类型 ：文件(-),目录(d),链接(l)
2. 其余字符每3个一组(rwx)   读(r)  写(w)  执行(x)
3. 第一组rwx ：文件拥有者的权限是读，写，和执行
4. 第二组rw- :与文件拥有者同一组的用户权限是读，写但不能执行
5. 第三组r-- ：不与文件拥有者同组的其他用户的权限是读，但不能写与执行
6. 可用数字代表 ：r=4,w=2,x=1 因此rwx =4+2+1=7
7. 1					文件：硬连接数  或  目录 ：子目录数
8. root   			用户
9. root   			组
10. 1213  			文件字节，如果是文件夹 则为4096字节
11. Feb 2 09:39  最后修改日期
12. abc				文件名

- #### 修改权限

  chmod ：修改文件或目录的权限

  **第一种方式 ：+、-、=更变权限**

  ​	chmod u=rwx,g=rx,o=x  文件目录名

  ​	chmod o+w    文件目录名

  ​	chmod a-x      文件目录名

![1563977110828](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1563977110828.png)

- **第二种方式  ：通过数字变更权限**

​				r=4 w=2  x=1    ,rwx=4+2+1=7

​				chmod u=rwx,g=rx,o=x  文件目录名

​				等价于 chmod 751 文件目录名

​				如：将abc.txt文件权限修改成  rwxr-xr-x    即  rwx =4+2+1=7   r-x =4+1=5  r-x=4+1=5

​				chmod 755 abc.txt					

- ##### **修改文件所有者**

  ​		chown  newowner file  ：改变文件的所有者

  ​		chown  newowner:newgroup file  修改用户的所有者与所有组

  ​		-R 如果是目录，则使其下所有子目录文件都递归

  ​		如  ：chown -R tom  kkk/      将kkk目录下所有文件的所有者替换成tom

- **修改文件所在组**

  ​		chgrp  newgroup file   ：修改文件的所有组

  ​		

# 6.crond任务调度

​		crontab [选项]     选项 : -e   -l  -f     编辑crontab定时任务    查询crontab任务    删除当前用户所有的crontab任务。![1564226593888](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564226593888.png)

**参数说明**

​		![1564226650230](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564226650230.png)

![1564226683959](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564226683959.png)

![1564226700624](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564226700624.png)

# 7.磁盘分区与挂载

​	**分区方式**

​			1）mbr分区

​						1.最多支持4个主分区

​						2.系统只能安装在主分区

​						3.扩展分区要占一个主分区

​						4.MBR最大只支持2TB，但拥有最好的兼容性

​			2）gtp分区

​						1.支持无限多个主分区

​						2.最大支持18EB的容量(1EB=1024PB,1PB=1024TB)

​						3.windows7 64位以后支持gtp

​	**原理介绍**

​		1.Linux来说无论有几个分区，分给那一目录使用，都只有一个根目录，一个独立且唯一的文件架构，Linux中每个分区都是用来组成整个系统文件的一部分

​		2.Linux采用了一种叫"载入"的处理方式，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来，这时要载入的一个分区将使它的存储空间在一个目录下获得

![1564227279845](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564227279845.png)

​	硬盘说明

​		1.Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘

​		2.对于IDE硬盘，驱动器标识符为"hdx~",其中'hd'代表分区所在设备的类型。'x'为盘号（a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘），'~'代表分区，前4个分区用数字1~4表示，是主分区或拓展分区，从5开始就是逻辑分区。**例** ：hda3,表示为 第一个IDE硬盘上的第三个主分区或拓展分区。

​	**指令**

​		lsblk  -f

​		![1564227676833](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564227676833.png)

**磁盘情况查询**

​	查询系统整体磁盘使用情况

​			df -h

​	查询指定目录的磁盘占用情况

​			du -h /目录    -s 指定目录占用大小汇总    -h 带计量单位   -a 含文件   --max-depth=1 子目录深度   -c 列出明细的同时 ，增加汇总值

​	**查询/opt目录的磁盘占用情况 深度为1**

​			du -ach --max-depth=1 /opt

​	**统计/home文件夹下文件的个数**

​			ls -l /home | grep "^-" |wc -l

​	**统计/home文件夹下的目录个数**

​			ls -l /home | grep "^d" |wc -l

​	**统计/home文件夹下的文件个数，包括子文件夹**

​			ls -lR /home | grep "^-" |wc -l

​	**统计文件夹下目录的个数，包括子文件夹**

​			ls -lR /home | grep "^d" |wc -l

​	树状图显示目录结构

​			tree

# 8.网络配置

####    固定ip

​	vim/etc/sysconfig/network-scripts/ifcfg-eth0

![1564228772781](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564228772781.png)

**service network restart**  重启服务

**reboot** 重启系统

​	ifcfg-eth0文件说明

​	![1564228901011](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564228901011.png)

# 9.进程管理

##### 	进程说明

​		PID：进程识别号   TTY 终端机号	TIME ：此进程所消耗CPU时间    CMD：正在执行的命令或进程名

##### 	指令

​	**ps -aux**  :查看进行使用。      -a：显示当前终端的所有进程信息    -u：以用户的格式显示进程信息    -x：显示后台进程运行的参数

![1564229295630](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1564229295630.png)

​	**ps -ef |more**  ：以全格式显示当前所有的进程

​		-e显示所有进程  -f全格式

​	**kill** [选项]  进程号： 通过进程号杀死进程			选项 -9 ：强迫进程立即停止

​	**killall** 进程名 ：通过进程名称杀死进程

​	**pstree** [选项] ：查看进程详细信息。    -p:显示进程的PID   -u:显示进程的所属用户

#### 	**service 管理**

​		service  服务名 	[start | stop | restart | reload | status]

​		在CentOS7.0后 不再舒勇service ，而是systemctl

​		**service iptables status**  :查看当前防火墙的状态

​		**service iptables stop** :关闭防火墙

​		**service iptables start** :重启防火墙

#### 	**chkconfig**

​		**chkconfig --list |grep xxx** ：查看指定服务

​		**chkconfig  iptables --list** ：查看指定服务名

#### 动态监控进程

​		top与ps相似，都是用来显示正在执行的进程，Top与ps不同在于top在执行一段时间后可以更新正在运行的进程

​		top [选项] 

​		**选项**			

​			-d 秒数  ：指定top命令每隔几秒更新，默认是3秒，在top命令的交互模式当中可以执行的命令

​			-i ：使top不显示任何显示或僵死进程

​			-p ：通过指定监控进程ID来仅仅监控某个进程的状态

​			交互操作命令

​			P ：以CPU使用率排序        M ：以内存使用率排序       N：以PID排序    q 退出top

​		**例 ：监视特定用户**

​		top :输出此命令，按回车，查看执行的进程

​		u：然后输入 'u' 回车，再输入用户名

​		**例 ：终止指定的进程**

​		top :输出此命令，按回车，查看执行的进程

​		k：然后输入‘k’ 回车，再输入要结束的进程ID号

#### 查看系统网络情况

​	**netstat** [选项]				netstat -anp

​	选项  ： -an ：按一定顺序排列输出    -p ：显示那个进程在调用

# 10.RPM与YUM

### rpm包管理

#### 		rpm包的查询指令

​				**rpm -qa |grep xx**

​				**rpm -qa** ：查询所安装的所有的rpm软件包

​				**rpm -q** 软件包名  ：查看软件包是否安装

​				**rpm -qi** 软件包名  ：查看软件包信息

​				**rpm -ql** 软件包名  ：查看软件包中的文件

​				**rpm -qf** 文件全路径名  ：查看文件所属的软件包

​				**rpm -e**  软件包名 ：删除指定软件包

​				**rpm -ivh** RPM包全路径名 ：安装rpm包  i=install 安装   v=verbose 提示   h=hash 进度条

​	**注** ：1.如果其他软件包依赖于要卸载的软件包，卸载使则会产生错误信息。   2.如要删除有依赖包的软件包，则可增加--nodeps 强制删除（不推荐）

### yum

​	**yum list |grep xx** ：查询yum服务器是否有需要安装的软件

​	**yum install xxx** ：安装指定的yum包



# Vim常用命令

1)拷贝当前行  yy 拷贝当下几行 nyy  粘贴p
2)删除当前行 dd 删除当下几行 ndd
3)在文件中查找某个单词    即命令行模式下 /关键字  回车  输入n就是查找下一个
4)设置文件的行号 ，取消文件的行号 即命令行模式下  set nu  和 set nonu
5)编辑文件，使用快捷键到文档最末行[G]和最首行[gg]
6）在一个文件中输入"hello"然后撤销此动作
7)在文件中输入“hello”然后撤销此动作，在正常模式下输入u
8)编辑文件  并将光标移动到第二十行 shift+g
1.显示行号 :set nu
2.输入20
3.输入shift+g