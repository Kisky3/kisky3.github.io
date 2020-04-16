---
layout: post
title: Connect Mysql8.0.14 Container To Sequel
date: 2019-12-08 00:57:08
tags:
- Mysql
- Sequel Pro
clearReading: true
thumbnailImage: 20191208.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
将Mysql 8.0.14容器连接到Sequel Pro
<!--more-->
### 启动一个Mysql容器
首先本地建造一个测试用的mysql容器,为了避免使用重复端口号，这里设置为4306。
```
docker run --name test_mysql -v ~/docker/mysql/conf:/etc/mysql/conf.d -v mysql-datavolume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=root -e MYSQL_DATABASE=test -p 4306:3306 mysql:8.0.14 --default-authentication-plugin=mysql_native_password
```

出现[Server] /usr/sbin/mysqld: ready for connections的log就说明应该启动了Mysql8.0.14的容器了。
***

### 连接启动好的docker容器
分别试试以下途径连接，看看能否成功连接上容器。
- docker
- ローカル
- Sequel Pro

##### 1.使用docker进入容器
1.使用exec命令行进入容器，容器名为test_mysql
```
docker exec -it test_mysql bash
```

显示如下则表示连接成功：
```
root@cf28f18b8a08:/#
```
测试一下我们的容器参数是否设置成功了。
```
root@cf28f18b8a08:/# printenv
```
显示如下，设置成功。
```
HOSTNAME=cf28f18b8a08
MYSQL_DATABASE=test
MYSQL_ROOT_PASSWORD=root
PWD=/
HOME=/root
MYSQL_MAJOR=8.0
GOSU_VERSION=1.7
MYSQL_USER=root
MYSQL_VERSION=8.0.14-1debian9
TERM=xterm
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/usr/bin/printenv
```
2.在容器内连接Mysql
```
root@cf28f18b8a08:/# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.14 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
连接成功，可以在容器内进行Mysql的操作了。
***
##### 2.本地连接
使用本地的Mysql连接容器内的Mysql（首先确保本地Mysql安装完毕）
```
mysql -uroot -p -h 127.0.0.1 --port 4306
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.14 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```
本地连接成功！
***
##### 3.从Sequel Pro进行连接
输入信息后发现Sequel Pro出现了下面的错误导致无法连接：
<img src="./1.png" style="width:500px">

这是因为Mysql8.0系对登陆密码的认证方法更新了。
为了解除错误，要将root用户的插件修改成mysql_native_password。

首先要在本地连接docker容器内的Mysql，并确认目前user的情况
```
mysql> SELECT user, host, plugin FROM mysql.user;
+------------------+-----------+-----------------------+
| user             | host      | plugin                |
+------------------+-----------+-----------------------+
| root             | %         | caching_sha2_password |
| mysql.infoschema | localhost | caching_sha2_password |
| mysql.session    | localhost | caching_sha2_password |
| mysql.sys        | localhost | caching_sha2_password |
| root             | localhost | caching_sha2_password |
+------------------+-----------+-----------------------+
5 rows in set (0.01 sec)
```
可以看到所有的插件（plugin）都被设定为caching_sha2_password，总之为了连接Sequel Pro，我们要将root用户的插件修改成mysql_native_password。
```
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
Query OK, 0 rows affected (0.01 sec)
```
修改成功后再次确认状态
```
mysql> SELECT user, host, plugin FROM mysql.user;
+------------------+-----------+-----------------------+
| user             | host      | plugin                |
+------------------+-----------+-----------------------+
| root             | %         | mysql_native_password |
| mysql.infoschema | localhost | caching_sha2_password |
| mysql.session    | localhost | caching_sha2_password |
| mysql.sys        | localhost | caching_sha2_password |
| root             | localhost | caching_sha2_password |
+------------------+-----------+-----------------------+
5 rows in set (0.01 sec)
```
可以看到plugin已经被修改成功了。
再试着连接Sequel Pro看看
<img src="./1.png" style="width:500px">

以上 成功！

***
### 踩坑
在本地安装完Mysql的时候，输入mysql -u root -p会出现：zsh: command not found: mysql的提示。
此时需要配置环境变量。

解决方法：

a.打开终端,输入： cd ~

b.输入：sudo vim .bash_profile

回车执行，需要输入root用户密码。sudo是使用root用户修改环境变量文件。

c.输入i进入编辑模式，然后输入：export PATH=${PATH}:/usr/local/mysql/bin

然后esc退出insert状态，并在最下方输入:wq保存退出。

d.输入：source .bash_profile

回车执行，运行环境变量。

e.vi ~/.zshrc，在这里面添加了：

export PATH=${PATH}:/usr/local/mysql/bin

保存后 source ~/.zshrc

f.执行命令：mysql -u root -p即可
