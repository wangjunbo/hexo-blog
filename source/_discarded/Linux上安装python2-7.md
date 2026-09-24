title: Linux上安装python2.7
author: peace
date: 2018-03-04 21:56:45
tags:
---
1. 打开python[下载页面](https://www.python.org/downloads/release/python-2714/)  
2. 下载 Gzipped source tarball版本  
```
wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz
```
3. 解压
```
tar -zxf Python-2.7.14.tgz
```
4. 进入目录
```
cd Python-2.7.14
```
5. 编译安装
```
./configure --prefix=/usr/local/python2.7 --with-threads --enable-shared
make && make altinstall
```
6. 备份旧python相关命令
```
[root@server1 bin]# mv /usr/bin/pip /usr/bin/pip_old
[root@server1 bin]# mv /usr/bin/easy_install /usr/bin/easy_install_old
[root@server1 bin]# mv /usr/bin/python /usr/bin/python_old
```
7. 新版本python命令做软连接，快捷使用
```
```
8. 测试python是否可以正常使用
