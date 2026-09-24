title: Python 2.6 升级至 Python 2.7
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-03-04 22:41:00
---
```
# 查看当前系统中的 Python 版本，返回 Python 2.6.6 为正常
python --version

Python 2.6.6

# 检查 CentOS 版本，返回 CentOS release 6.8 (Final) 为正常
cat /etc/redhat-release

CentOS release 6.8 (Final)

# 安装所有的开发工具包
yum groupinstall -y "Development tools"
# 安装其它的必需包
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel

# 下载、编译和安装 Python 2.7.13
#wget https://www.python.org/ftp/python/2.7.13/Python-2.7.13.tgz
tar zxf Python-2.7.13.tgz
cd Python-2.7.13
./configure
make && make install

# 查看新的 Python 版本，返回 Python 2.7.13 为正常
python --version

Python 2.7.13
```
升级pip
```
vim /etc/profile
#在环境变量中加入pip所在的路径/usr/local/bin
export PATH=$PATH:$JAVA_HOME/bin:/usr/local/bin
```
参考https://zhuanlan.zhihu.com/p/26596585