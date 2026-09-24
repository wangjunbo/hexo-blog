title: Canal的基本使用
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2022-10-20 02:17:00
---
参考 [https://www.51cto.com/article/626708.html](https://www.51cto.com/article/626708.html)
# 安装Mysql 8 
```shell
docker pull mysql/mysql-server:latest
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=qxiu2022 -d mysql/mysql-server

进入mysql docker容器
docker exec -it 123456 /bin/sh
GRANT ALL PRIVILEGES ON *.* TO 'abc'@'%';
#设置可以远程连接
ALTER USER 'abc'@'%' IDENTIFIED WITH mysql_native_password BY 'abc123';
flush priviles;

#修改容器的系统时间
docker cp /usr/share/zoneinfo/Asia/Shanghai docker_container_id:/etc/localtime
docker restart docker_container_id
```

# Canal-Server

通过docker镜像的方式启动Cannal-Server还有点问题，下边通过下载 canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz ，然后通过命令行的方式启动

<!-- more -->
## 下载 canal.deployer
canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz 

> wget [https://d7.serctl.com/downloads8/2022-10-13-16-00-30-canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz](https://d7.serctl.com/downloads8/2022-10-13-16-00-30-canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz)


## 解压 canal.deployer
```shell
mv 2022-10-13-16-00-30-canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz
tar -zxvf canal-canal.deployer-1.1.7-SNAPSHOT.tar.gz
```
## 修改配置文件
创建 conf/prod目录，并将 conf/example/instance.properties 复制到 conf/prod 目录下， 并修复 conf/prod/instance.properties 
```shell
canal.instance.master.address=bigdata.example.com:3306
canal.instance.dbUsername=abc
canal.instance.dbPassword=abc123
canal.instance.filter.regex=db1\\..*

```
## 启动canal.deployer

> bin/startup.sh


# Canal Adapte
## 下载canal.adapter
[canal-canal.adapter-1.1.7-SNAPSHOT.tar.gz](https://d.serctl.com/?uuid=c7d4e816-72dc-4e78-ae3f-47d75af70eb5)

## 修改配置文件
### 配置文件application.yml
```shell
  srcDataSources:
    defaultDS:
      url: jdbc:mysql://bigdata.example.com:3306/mydata?useUnicode=true&characterEncoding=utf8&autoReconnect=true&useSSL=false
      username: abc
      password: abc123
  canalAdapters:
  - instance: prod  # canal.deployer中 instance.properties 的文件夹名
    groups:
    - groupId: g1
      outerAdapters:
#      - name: logger
      - name: rdb
        key: mysql1
        properties:
          jdbc.driverClassName: com.mysql.jdbc.Driver
          jdbc.url: jdbc:mysql://10.8.20.145:3306/business?useUnicode=true&characterEncoding=utf8&autoReconnect=true&useSSL=false
          jdbc.username: abc
          jdbc.password: abc123
```

### 修改配置文件 rdb/dashboard_api_log.yml
在rdb目录下创建一个dashboard_api_log.yml文件
配置如下：
```shell
dataSourceKey: defaultDS
destination: prod  # canal.deployer中 instance.properties 的文件夹名
groupId: g1
outerAdapterKey: mysql1
concurrent: true
dbMapping:
  database: mydata
  table: dashboard_api_log
  targetTable: dashboard_api_log
  targetPk:
    id: id
  mapAll: true
  etlCondition: "where  1 = 1 "
  commitBatch: 5 # 批量提交的大小
```
## 启动canal adapter

>  bin/startup.sh


这样就可以将mysql中的数据，同步到另外一个mysql表中了。



## 客户端报错
 1 Reason: Unable toset value forproperty src-data-sources 
可能是mysql connector jar的版本太低了
由于Mysql 是8.0 这里需要下载 mysql-connector-java-8.0.20.jar，并将其放入lib中 
```shell
cp mysql-connector-java-8.0.20.jar canal-adapter/lib/ 
#将老版本的mysql-connector移出lib目录
mv canal-adapter/lib/mysql-connector-java-5.1.48.jar ./
```

2 com.alibaba.otter.canal.protocol.exception.CanalClientException: failed to subscribe with reason: something goes wrong with channel:[id: 0x46f25d90, /127.0.0.1:26056 => /127.0.0..
1:11111], exception=com.alibaba.otter.canal.server.exception.CanalServerException: destination:example should start first

因为需要在 canal.adapter 中的application.yml 和 conf/rdbdashboard_api_log.yml 配置文件中指定 destination 指定为 canal.deployer 中对应的conf 目录下的文件夹名，比如这里是prod。