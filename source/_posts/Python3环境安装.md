title: Python3环境安装
author: peace
tags:
  - Python
categories:
  - 编程
date: 2020-07-13 05:36:00
---
本来想写一个Python3环境安装的笔记，以备自己以后使用，但是我发现网上有些文章写的已经很好了，我就不再赘述了。链接如下：
[https://liqiang.io/post/install-python3-8-in-centos-973bdb81](https://liqiang.io/post/install-python3-8-in-centos-973bdb81)
### 下载python3
下载的是 Python3.8 版本的：Python-3.8.0.tgz
[root@test]# wget [https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz](https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz) -O /tmp/Python-3.8.0.tgz
[root@test]# cd /tmp && tar zxf Python-3.8.0.tgz
[root@test]# cd Python-3.8.0
### 编译安装 Python3.8
```shell
[root@test]# rm -rf /usr/local/python3 #如果之前安装过Python3，最好将其卸载掉。
[root@test]# yum install sqlite-devel #安装sqlite，非常多的框架都依赖这个数据库，如果不安装，很有可能之后还得重新安装Python
[root@test]# ./configure prefix=/usr/local/python3  
[root@test]# make && make install
[root@test]# export PATH=$PATH:/usr/local/python3/bin/
```
### 问题1
python 3.8.0 编译报错 SystemError: returned NULL without setting an error。
##### 解决方式
configure的时候将`--enable-optimizations`去掉
参考 [https://stackoverflow.com/questions/58048079/how-to-succesfully-compile-python-3-7](https://stackoverflow.com/questions/58048079/how-to-succesfully-compile-python-3-7)