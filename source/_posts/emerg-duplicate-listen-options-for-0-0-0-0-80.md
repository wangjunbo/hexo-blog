title: '[emerg] duplicate listen options for 0.0.0.0:80'
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2020-02-25 07:56:00
---
Just hit this same issue, but the duplicate default_server directive was not the only cause of this message.

You can only use the backlog parameter on one of the server_name directives.

Example
site 1:
```
server {
    listen 80 default_server backlog=2048;
    server_name www.example.com;
    location / {
        proxy_pass http://www_server;
    }
```
site 2:
```
server {
    listen 80;    ## NOT NOT DUPLICATE THESE SETTINGS 'default_server backlog=2048;'
    server_name blogs.example.com;
    location / {
        proxy_pass http://blog_server;
    }
```
backlog 只能在其中一个server上设置。
参考 https://stackoverflow.com/questions/13676809/serving-two-sites-from-one-server-with-nginx