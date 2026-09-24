title: Linux上安装mysql
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2020-02-22 13:19:00
---
查看CentOS自带mysql是否已安装。
```
输入：yum list installed | grep mysql
```
卸载已经安装的mysql
```
yum -y remove mysql-libs.x86_64
```

查看yum库上的mysql版本信息(CentOS系统需要正常连接网络)。
```
输入：yum list | grep mysql 或 yum -y list mysql*
```


安装
```
yum install mariadb-server mariadb
启动：systemctl start mariadb
```

修改root密码
```
在一个窗口执行 sudo mysqld_safe --skip-grant-tables --skip-networking &
在另一个窗口通过  mysql -u root 登录mysql
```

通过如下方式修改密码
```
update mysql.user set PASSWORD=PASSWORD('123456') where user='root' and host='localhost';
或 update mysql.user set authentication_string=PASSWORD('123456') where user='root' and host='localhost';
flush privileges;
exit;

ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```
查看mysql的版本
```
select version();
```
```
create database test character set utf8;
```
创建用户
```
create user test@localhost identified by '123456';
grant all privileges on *.* to test@localhost identified by '123456';
grant all privileges on *.* to test@'%' identified by '123456';
flush privileges;
```

检查
```
[root@snails ~]# netstat -ano |grep 3306
tcp        0      0 0.0.0.0:3306                0.0.0.0:*                   LISTEN      off (0.00/0/0)
```