title: Nginx根据参数转发
author: peace
tags:
  - Nginx
categories:
  - 编程
date: 2021-08-11 08:27:00
---
首先明确一点：Nginx的location不能否匹配到问号后的参数。参考https://www.zhihu.com/question/50190510

所以通过在location中的if条件来进行逻辑判断

```
        location /p.gif {
                if ($args ~ "getip") {
                    add_header Content-Type "text/plain;charset=utf-8";
                    return 200 '$proxy_add_x_forwarded_for';
                }
                proxy_pass         http://big-da/log-server/push;
                proxy_set_header   Host             $host;
                proxy_set_header   X-Real-IP        $remote_addr;
                proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
```


url匹配规则 参考https://www.cnblogs.com/woshimrf/p/nginx-config-location.html
```
location [=|~|~*|^~|@] /uri/ {
  ...
} 
= : 表示精确匹配后面的url
~ : 表示正则匹配，但是区分大小写
~* : 正则匹配，不区分大小写
^~ : 表示普通字符匹配，如果该选项匹配，只匹配该选项，不匹配别的选项，一般用来匹配目录
@ : "@" 定义一个命名的 location，使用在内部定向时，例如 error_page
```