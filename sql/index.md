## SQL
> 以下是我的实际操作, 有些问题也不能解答
> aliyun ECS Ubuntu16.04 mysql5.7.24 putty navicat 工具 
> MySQL5.7 mysql.user表没有password字段 改 authentication_string

### Ubuntu 安装mysql
```sh
	sudo apt-get install mysql-server
	sudo apt-get isntall mysql-client
	sudo apt-get install libmysqlclient-dev
```
> 期间需要输入root密码

```sh 
	mysql -V
```
mysql  Ver 14.14 Distrib 5.7.24, for Linux (x86_64) using  EditLine wrapper
```sh 
	sudo netstat -tap | grep mysql	
```
如果看到有mysql 的socket处于 listen 状态则表示安装成功

---
### 登录|退出
```sh 
	mysql -uroot -p 
	show databases;  # 显示库
	use mysql;				# 切换到表
	show tables; 			# 显示表
	exit; # 退出mysql
```
---
### 允许远程访问
```sh
	vi /etc/mysql/mysql.conf.d/mysqld.cnf
	# bind-address = 127.0.0.1 注释掉 bind-address = 127.0.0.1
	mysql -uroot -p  # 登录MySQL
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'youpassword' WITH GRANT OPTION; # youpassword 改为自己的, 引号要有
	FLUSH PRIVILEGES; # 重载授权表
	exit; # 退出
	sudo service mysql restart # 重启
	登录自己的工具 连接测试
```
### 创建新用户, 用于日常操作
> 添加用户

```sh
	mysql -u root -p
	create user 'newuser'@'host' identified by 'password'; # 添加用户 newuser用户名 host 选择(localhost|%) %远程连接  localhost本地  password 密码
	eg: create user 'xiaoming'@'%' identified by '123456';
	eg: create user 'xiaoli'@'localhost' identified by '123456';
```	
> 分配数据库权限

```sh
	grant all privileges on `test_privileges`.* to 'xiaoming'@'%'; # 给xiaoming 分配test_privileges库下面表的全部权限 all privileges 可换为具体操作权限(select,delete,update,create,drop); 
	eg: grant select,delete,update,create on `test_privileges`.* to 'xiaoming'@'%';
	# 给xiaoming 分配test_privileges库下面表的select,delete,update,create权限 
	eg: grant select,delete,update,create on `test_privileges`.'user' to 'xiaoming'@'%';
	# 给xiaoming 分配test_privileges库下面user表的select,delete,update,create权限 
	eg: grant all privileges on *.* to 'xiaoming'@'%';  # xiaoming用户有所有数据库权限
	flush privileges; 
```
	