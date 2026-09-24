title: 在Linux上安装ClickHouse可视化工具tabix
author: peace
tags:
  - Clickhouse
categories:
  - 编程
date: 2021-06-02 01:42:00
---
### 1 先安装好nginx, 可以参考 
[http://www.hohode.com/2018/02/13/Nginx%E7%9A%84%E5%AE%89%E8%A3%85/](http://www.hohode.com/2018/02/13/Nginx%E7%9A%84%E5%AE%89%E8%A3%85/)
### 2 从GitHub上下载tabix

![从GitHub上下载tabix](/images/pasted-41.png)

### 3 解压并查看tabix目录结构
```shell
tar -zxvf tabix-18.07.1.tar.gz
ll tabix-18.07.1
```

### 4 配置nginx
```shell
vim conf/nginx.conf
```
替换成以下内容,注意要更改root地址

```nginx
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    server {
        listen 80;
        server_name ui.tabix.io;
        charset        utf-8;
        #替换成自己tabix的build目录地址
        root /opt/tabix-18.07.1/build;
        location / {
            if (!-f $request_filename) {
                rewrite ^(.*)$ /index.html last;
            }
            index  index.html index.htm;
        }
    }
}
```


### 5 启动Nginx
```shell
sbin/nginx
```
### 6 访问http://126.21.18.61 
注意将126.21.18.61替换为自己服务器的ip地址

![tabix home page](/images/pasted-42.png)

![tabix index page](/images/pasted-43.png)

下载地址 [https://github.com/tabixio/tabix/releases](https://github.com/tabixio/tabix/releases)
参考文档 [https://blog.csdn.net/qq_28603127/article/details/109281086](https://blog.csdn.net/qq_28603127/article/details/109281086)
