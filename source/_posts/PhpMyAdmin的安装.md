title: PhpMyAdmin的安装
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2019-11-06 20:16:00
---
一 安装Apache
1.安装apache
```
yum -y install httpd
```
2.安装apache扩展
```
yum -y install httpd-manual mod_ssl mod_perl mod_auth_mysql
```
3.启动apache
```
service httpd start
(centos 7 请使用下面命令)

systemctl start httpd.service #启动apache
systemctl stop httpd.service #停止
systemctl restart httpd.service #重启
systemctl enable httpd.service #设置开机自启动
```
<!-- more -->
4.检查安装
如果80端口被占用，可能会出现如下的报错
```
Job for httpd.service failed because the control process exited with error code. See "systemctl status httpd.service" and "journalctl -xe" for details.
```
这个时候，将/etc/httpd/conf/httpd.conf中的端口改成8123。

浏览器访问ip,安装成功,结果如下 http://localhost:8123
![](/images/pasted-10.png)

二 安装PHP
1、更换RPM源
```
#Centos 5.X：
rpm -Uvh http://mirror.webtatic.com/yum/el5/latest.rpm

#CentOs 6.x：
rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm

#CentOs 7.X：
rpm -Uvh https://mirror.webtatic.com/yum/el7/epel-release.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```
2、安装新版本 PHP
```
yum install php71w php71w-cli php71w-common php71w-devel php71w-embedded php71w-fpm php71w-gd php71w-mbstring php71w-mysqlnd php71w-opcache php71w-pdo php71w-xml php71w-ldap php71w-mcrypt
```
3、 重新启动相关服务
```
service php-fpm restart
service httpd restart
```
4、检查版本
```
php -v
```
![](/images/pasted-15.png)

4、检查php是否和apache集成成功
```
cd /var/www/html/
touch v.php
vim v.php
```
写入一下内容
```
<?php
    echo '<title>hello world</title>';
    phpinfo();
?>
```
保存，退出

重启相关服务
```
service php-fpm restart
service httpd restart
```
在浏览器中输入 http://localhost/v.php ，回车后显示如下页面，说明配置没有问题

![](/images/pasted-11.png)

三 安装phpMyAdmin

1、下载phpMyAdmin并解压至/var/www/html/目录下

![upload successful](/images/pasted-12.png


进入phpmyadmin，复制phpmyadmin的简单配置文件，作为默认配置文件
```
[root@hadoop001 html]# cd phpmyadmin
[root@hadoop001 html]# cp config.sample.inc.php config.inc.php
[root@hadoop001 html]# vi config.inc.php
```
配置phpmyadmin的主机。修改成你要连的主机

![](/images/pasted-14.png)

2、重启相关服务
```
service php-fpm restart
service httpd restart
```

3、在浏览器中输入http://localhost/phpmyadmin/ ， 回车之后应该就会显示登录页面

![](/images/pasted-13.png)