title: Centos6.5安装jdk8
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-04-25 13:45:00
---
1 下载  
[download](http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz
)

2 解压  
```
cd /data/software/
tar -zxvf jdk-8u171-linux-x64.tar.gz
```

3 配置  
```
vi /etc/profile

export JAVA_HOME=/data/software/jdk1.8.0_171
export PATH=$PATH:$JAVA_HOME/bin:
```

4 使其生效  
```
source /etc/profile
```

5 检查
```
[root@server1 jdk8]# java -version
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```
