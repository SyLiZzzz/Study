# 1.SVN管理员命令

1. **svnadmin help** ：svn管理员命令帮助。
2. **svnadmin --version** :查看版本信息。
3. **svnadmin create** ：创建仓库。 如：svnadmin create d:\svn\repository\sms。**注**：svn仓库分为顶层仓库与根仓库。该命令创建的是根仓库，在创建根仓库的时候顶层仓库必须是村早的，其不会自动创建。根仓库目录是否存在，均是可以的。若根目录不存在，命令会自动创建该根仓库

# 2.SVN服务端命令

1. **svnserve -h** ：svn服务命令帮助
2. **svnserve -d**  ：开启DOS系统下的SVN服务。即开放SVN服务端的顶层仓库，允许客户端访问SVN服务器里的各个根仓库。SVN服务端默认端口为：3690  。netstat -a查看当前网络的连接状态。
3. **svnserve -d** -listener-port=xxxx ：指定svn服务占用的端口为xxxx。
4. **svnserve -d -r** ：指定默认的SVN顶层仓库的路径。指定后，客户端在使用SVN时直接给出根仓库名即可。如svn:/localhost/sms
5. sc create SVNServer binpath="D:/SVN/svn/bin/svnserve.exe --service -r D:/SVN/svn/repository" start=auto depend=Tcpip  : 将SVN服务注册为开机自启动的win服务。该命令需在管理员权限的命令提示符下运行。且在win7.8环境下binpath= "D:/SVN/svn/bin/svnserve.exe需加入空格。

![1563697840836](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1563697840836.png)

![1563701694002](C:\Users\LZP\AppData\Roaming\Typora\typora-user-images\1563701694002.png)

修改客户端权限

# 3.SVN客户端命令

**svn checkout** ：检出，即创建客户端指定目录与服务端根仓库之间的连接关系，一点连接成功只要无明确的断开连接则一直存在。所以一个客户端一般情况下只要检出一次。

​		**如**：svn checkout svn://localhost/sms d:/SVN/svn/group/aacof

**svn add** ：当一个文件/目录，被存放到working copy中时，并不会感知到他们的存在，即svn不会对其进行管理。若要使SVN对其管理，必须将其通知add命令，添加到SVN管理中。

​		**如**：svn add d:/SVN/svn/group/aacof/Students.txt

​		**注** ：1)被add的文件/目录，必须存在于Working Copy中。2) add命令的作用就是将制定文件/目录交给SVN进行管理，所以一个文件/目录一般情况下，就一次add即可。3）被add的目录，会将当前目录包含其所有文件/目录，一次性交给SVN管理

**svn commit** ：用于将客户端中working copy中所有对文件/目录的操作提交到服务端。

- commit命令必须携带参数-m,用于完成日志记录。
- 提交文件后在服务端是无法直接看到的。
- 对于已经提交过的文件再没有被修改的情况下，再次提交是没有意义的。
- -m参数与目标文件在commit命令中的顺序是可颠倒的

**svn update** ：用于将当前客户端Working copy中的文件/目录更新到SVN服务端相同版本。

**svn delete** ：用于删除指定的文件/目录，仅仅删除本地的，不会影响服务端的文件/目录。只有当前客户端执行了commit命令，才会将删除操作同步到SVN服务端。

**svn revert** ：用于恢复客户端被删除的文件/目录 。如被删除的文件已commit，则无法恢复。

**svn list** ：用于获取当前服务端中已commit的文件/目录

**svn info** ：用于获取当前svn客户端与服务端的相关信息

**svn help** ：svn命令帮助

# 4.TortoiseSVN客户端

- 客户端检出checkout
- 客户端导入import       注：在要导入所在目录上右击/在要导入的内容所在目录中右击，选择TortoiseSVN-->import
- 客户端更新Update
- 客户端导出Export       注：在要导出所在目录上右击/在要导出的内容所在目录中右击，选择TortoiseSVN-->export
- 客户端添加Add
- 客户端提交Commit
- 客户端删除Delete       注：使用SVN下的删除操作，与使用windows中的删除操作，效果相同
- 客户端恢复Revers
- 客户端指定版本回退TortoiseSVN-->update to revision