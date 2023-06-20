### mysql

#### 下载安装使用
下载地址: [mysql download](https://dev.mysql.com/downloads/mysql/

##### windows下载安装使用
1. 下载windows版本，将bin目录设置为环境变量
2. 使用管理员身份cmd生成data文件 `mysqld --initialize-insecure --user=mysql`
3. 输入mysqld命令安装服务mysqld -install， cmd输入services.msc查看mysql服务并启动.
4. 启动服务 net start mysql 关闭服务 net stop mysql

**登录**
mysql [-h 主机名 -p 端口号] -u 用户名 -p密码
本地数据库可以不用输入主机名和端口号，mysql -u root -proot(-p与密码没有空格)
mysql -u root -p回车输入密码.
