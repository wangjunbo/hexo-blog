title: Nginx日志输出cookie的值
author: peace
tags:
  - Nginx
categories:
  - 编程
date: 2021-10-26 08:40:00
---
### 一 输出全部cookie的信息
```
    log_format data_log '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" $cookie_C_I $http_x_forwarded_for'
                    '"$http_cookie"'
                    '"$upstream_addr" "$upstream_status" "$upstream_response_time" "$request_time" ';
```
通过`$http_cookie`就可以将请求的全部cookie获得。

### 二 输出单个cookie
输出单个cookie也很简单，只需要为cookie key加上`$cookie_`前缀就可以了，例如有一个cookie的key为`_tracker_user_id_`,那么在nginx中可以通过`$cookie__tracker_user_id_`就可以获取到了。
```
    log_format data_log '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" $cookie_C_I $http_x_forwarded_for'
                    '"$cookie__tracker_user_id_"'
                    '"$upstream_addr" "$upstream_status" "$upstream_response_time" "$request_time"';
```