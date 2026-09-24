title: Nginx中include其他配置文件
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2019-02-24 11:48:00
---
/usr/local/nginx/conf/nginx.conf
```
#user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    log_format mylog_format '$remote_addr|$remote_user|$time_local'
                       '$request|$status|$bytes_sent'
                       '$http_referer|$http_user_agent|$gzip_ratio'
                       '#$bytes_sent|$connection|$connection_requests|$msec|$pipe|$request_length|$request_time|$status|$time_iso8601|$time_local';

    access_log logs/access.log mylog_format;

    server {
	listen 80;
	location / {
		proxy_pass http://127.0.0.1:8181;
	}
   }

server {
    listen 443;
    client_max_body_size 5M;
    server_name api.dict123.cn;
    ssl on;
    ssl_certificate   cert/1638702783821.pem;
    ssl_certificate_key  cert/1638702783821.key;
    ssl_session_timeout 5m;
    ssl_ciphers EBDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:DIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        proxy_pass http://132.232.74.114:80;
    }
}

include /usr/local/nginx/conf/test1.conf;

}
```
如果test1.conf文件中不指定server_name，在使用nginx -s reload命令的时候，提示 
```
nginx: [warn] conflicting server name "" on 0.0.0.0:80, ignored
```
这个时候需要在test1.conf的配置文件中加上server_name的配置，配置如下：

/usr/local/nginx/conf/test1.conf
```
server {
    listen 80;
    server_name hohode.com;
    location /test1 {
        alias /data/pserver/;
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
        charset utf-8,gbk;
    }
}
```
