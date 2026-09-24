title: Mac上的一些常用命令
author: peace
tags:
  - Mac
categories:
  - 编程
date: 2018-04-28 15:03:00
---
1 文件上传  
创建/usr/local/bin/upl文件,内容如下
```
scp $1 root@transfer:/data/transfer/
```
切换到root用户，执行
```
chmod 777 /usr/local/bin/upl
```
