title: kafka cmake(原kafka manager)的使用
author: peace
tags:
  - kafka
categories:
  - 编程
date: 2021-09-06 03:55:00
---

![lastest_version](/images/pasted-54.png)

### 下载压缩包
wget https://github.com/yahoo/CMAK/releases/download/3.0.0.5/cmak-3.0.0.5.zip

### 解压cmak
unzip  cmak-3.0.0.5.zip

### 修改conf/application.conf
cmak.zkhosts="hadoop101.eqxiu.com:2181"  改为真实的zk地址

### 下载open jdk11
 wget https://download.java.net/openjdk/jdk11/ri/openjdk-11+28_linux-x64_bin.tar.gz
 
### 解压 open jdk11 
tar -zxvf openjdk-11+28_linux-x64_bin.tar.gz

### 修改bin/cmak启动脚本
在文件的最上边加上JAVA_HOME的路径,比如:
```
JAVA_HOME=/data/software/jdk-11
```

### 启动cmak 
```
./bin/cmak -Dhttp.port=10010
```

### 在浏览器上访问
```
http://your-ip-address:10010
```

参考 
https://github.com/yahoo/CMAK  
https://cloud.tencent.com/developer/article/1651137  