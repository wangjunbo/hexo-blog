title: Docker的一些使用
author: peace
date: 2020-03-14 10:03:59
tags:
---
docker的入门教程参考 https://www.runoob.com/docker/docker-tutorial.html

### Docker和宿主机之间传输文件
https://blog.csdn.net/xiangxianghehe/article/details/77131962
https://blog.csdn.net/zhuchunyan_aijia/article/details/80089443

docker cp /root/test2.txt  4404748690f1:/data/

### 安装mysql
```
docker pull mysql:8.0.19
docker images
```

![upload successful](/images/pasted-23.png)
```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d 9b51d9275906
```
```
mysql> grant all privileges on test.* to root@'%' identified '123456';

提示如下错误：ERROR 1064(4200): you have an error in you SQL syntax; **near 'identified '123456'' at line 1
```

查询MySQL8 相关授权资料的得知，分配权限不能带密码。
```
create user admin@localhost identified by 'xxxx';
```
好像得use mysql;再使用授权语句才会生效；
```
update user set host='%' where user='admin';

grant all privileges on *.* to admin@'%';

flush privileges;
```
远程连接的时候报plugin caching_sha2_password could not be loaded这个错误，可以尝试修改密码加密插件：
```
alter user 'admin'@'%' identified with mysql_native_password by 'xxxx';
```
查看某个用户的权限
```
show grants for admin;
select * from mysql.user where user='admin'\G;
```
如果遇到sequel pro操作数据库有异常，请下载最新版的sequel pro
https://sequelpro.com/test-builds

### 安装Elasticsearch
```
docker pull elasticsearch:7.6.1
```
