title: 轻量级自动化运维工具pssh
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-12-07 14:41:00
---
官网 http://www.theether.org/pssh/
文档 http://www.theether.org/pssh/docs/0.2.3/pssh-HOWTO.pdf

### 安装
```
mkdir -p /data/software/
cd /data/software/
wget http://www.theether.org/pssh/pssh-1.4.3.tar.gz
tar -zxvf pssh-1.4.3.tar.gz
cd pssh-1.4.3/
python setup.py install
```
### 使用
```
pssh -P -h hosts.txt  -o foo hostname
pscp -v -h hosts.txt  client.xml /data/apps/cat/
```
-P,-v可以把执行的结果直接打印到控制台。
