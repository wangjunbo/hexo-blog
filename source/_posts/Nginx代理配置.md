title: Nginx代理配置
author: peace
tags:
  - 服务器
categories:
  - 编程
date: 2018-02-13 20:21:00
---
上文介绍了Nginx的安装，下边介绍使用Nginx代理网站。
我是申请的阿里云免费证书，同时域名也在阿里云买的，所以中间省去了很多麻烦。   

按照阿里云上的说明，一步一步的安装配置好久可以了。通过页面访问，比如 https://www.youdomain.com ，然后看到 Welcome to nginx! ,就说明配置成功了。  

**中间有一个坑，就是现在阿里云的服务器默认很多端口都是关闭的，必须去控制台的安全组里面打开相应的端口，这次是https，才可以访问。**

转发配置如下,注意下边配置参数做相应的修改：
```
server {
    listen 443;
    server_name www.hohode.com;
    ssl on;
#    root html;
#    index index.html index.htm;
    ssl_certificate   cert/5614507086590401.pem;
    ssl_certificate_key  cert/561407086590401.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
	proxy_pass http://127.0.0.1:8083;
#        root html;
#        index index.html index.htm;
    }
}
```
### 配置上传文件的大小
```
client_max_body_size 1g;
```
参考 [Nginx配置上传文件的大小](http://blog.51cto.com/5148737/1977553)

### nginx将多个不同域名转发到不同端口
只在nginx的配置文件里面加了2个upstream,和2个server，其它的一行没动。重启nginx。

这里的ip用127.0.0.1代理，2个域名分别用 百度 和 谷歌 代替。

这里例如www.baidu.cn   baidu.cn ,空格分开表示两个访问地址都能控制。

百度谷歌两个项目页面分别在8081和8082两个tomcat下。
```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


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

    #gzip  on;

    upstream baidu{
        server 127.0.0.1:8081;
    }
	
    upstream google {
        server 127.0.0.1:8082;
    }

    server {
        listen       80;
        server_name  www.baidu.cn baidu.cn;
        location / {
          proxy_pass http://baidu;
          proxy_set_header   Host    $host;
          proxy_set_header   X-Real-IP   $remote_addr;
          proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }

    server {
        listen       80;
        server_name  www.google.cn google.cn;
        location / {
          proxy_pass http://google;
          proxy_set_header   Host    $host;
          proxy_set_header   X-Real-IP   $remote_addr;
          proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
 
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}
```