# Mysql 安装 #
## 删除suse自带mysql ##
1. 查看已安装的mysql安装包
	\# rpm -qa | grep -i mysql
	MysqlySQL-devel-5.5.37-1.sles10
	MySQL-server-5.5.37-1.sles10
	MySQL-shared-compat-5.5.37-1.sles10
	MySQL-client-5.5.37-1.sles10
	MySQL-shared-5.5.37-1.sles10
	MySQL-test-5.5.37-1.sles10
	MySQL-embedded-5.5.37-1.sles10
2. 停止mysql服务
	\# /etc/init.d/mysql stop
3. 使用rpm卸载mysql
	\# rpm -e --nodeps MysqlySQL-devel-5.5.37-1.sles10
	使用此命令依次卸载所有mysql rpm包

## 上传并解压安装介质 ##
1. 通过root用户上传mysql安装包mongodb-linux-x86_64-rhel62-3.2.7.tgz到服务器目录
   /usr/local/
2. 解压安装包
	\# tar -xzvf mongodb-linux-x86_64-rhel62-3.2.7.tgz
3. 启动mysql
	\#
