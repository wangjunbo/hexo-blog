title: ClickHouse JDBC bridge的使用
author: peace
tags:
  - Clickhouse
categories:
  - 编程
date: 2019-09-10 15:06:00
---
参考文档 https://github.com/alex-krash/clickhouse-jdbc-bridge

### 1.先把项目下载到本地，解压到目录比如/data/clickhouse-jdbc-bridge-master

### 2.使用如下命令打包
```
mvn clean package
```

生成的最终jar包如下
![upload successful](/images/pasted-3.png)

### 3.上传到目标服务器上
比如我要把 10.0.0.1 上Clickhouse上的数据复制到10.0.0.2的Clickhouse服务器上,那么将clickhouse-jdbc-bridge-1.0.jar上传到10.0.0.2上，

### 4.启动clickhouse-jdbc-bridge
之后使用如下命令启动 
```
java -jar clickhouse-jdbc-bridge-1.0.jar
```

### 5.在目标服务器10.0.0.2上执行命令


![upload successful](/images/pasted-4.png)
