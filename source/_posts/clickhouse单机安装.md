title: clickhouse单机安装
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-18 15:53:00
---
参考
https://blog.csdn.net/JIANG123456T/article/details/77674857
http://www.clickhouse.com.cn/topic/5a366e97828d76d75ab5d5a0
### 一. 环境
1、Centos 7.3   
2、Rpm包下载   
http://repo.red-soft.biz/repos/clickhouse/stable/el7/

<!-- more -->
### 二. 安装步骤
1.首先安装依赖
```
 yum install libicu unixODBC
```
2.下载安装包   
http://repo.red-soft.biz/repos/clickhouse/stable/el7/

3.安装
```
rpm -ivh clickhouse-server-common-1.1.54236-4.el7.x86_64.rpm
rpm -ivh clickhouse-server-1.1.54236-4.el7.x86_64.rpm
rpm -ivh clickhouse-debuginfo-1.1.54236-4.el7.x86_64.rpm
rpm -ivh clickhouse-client-1.1.54236-4.el7.x86_64.rpm
rpm -ivh clickhouse-compressor-1.1.54236-4.el7.x86_64.rpm
```
3.1 配置文件路径   
/etc/clickhouse-server/   

4.启动   
4.1 启动服务端   
```
clickhouse-server --config-file=/etc/clickhouse-server/config.xml
```
可看到clickhouse服务已经成功启动

4.2 启动客户端
```
clickhouse-client --host=xx.xx.xx.xx  --port=9000
```
![客户端简单操作](https://img-blog.csdn.net/20170829110804761?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSklBTkcxMjM0NTZU/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
4.3 查看启动的进程
```
ps -aux|grep clickhouse-server
```

4.4 关闭clickhouse服务  
使用kill -9 杀掉相关进程即可。

5 后台托管启动服务
```
nohup clickhouse-server --config-file=/etc/clickhouse-server/config.xml >null 2>&1 &
```