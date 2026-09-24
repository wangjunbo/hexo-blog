title: axios CORS跨域问题解决
author: peace
tags:
  - 前端
  - Nginx
categories:
  - 编程
date: 2021-11-02 03:22:00
---
前端通过使用axios请求后台服务，默认是不带cookie上报的，要想带上，需要进行如下设置：
```
axios.defaults.withCredentials = true
```

然后后端服务也要做些调整，比如Tornado需要设置
```
self.set_header("Access-Control-Allow-Credentials", "true")
self.set_header("Access-Control-Allow-Origin",origin)
```

如果不想在程序中设置的话，可以调整一下nginx的配置，比如某个location下增加如下配置：
```
 add_header 'Access-Control-Allow-Credentials' "true";
	add_header 'Access-Control-Allow-Origin' "$http_origin";
	add_header 'Access-Control-Allow-Headers' '*';
	add_header 'Access-Control-Max-Age' 1000;
	add_header 'Access-Control-Allow-Methods' 'POST, GET, OPTIONS';
	add_header 'Access-Control-Allow-Headers' 'authorization, Authorization, Content-Type, Access-Control-Allow-Origin, Access-Control-Allow-Headers, X-Requested-By, Access-Control-Allow-Methods';

```