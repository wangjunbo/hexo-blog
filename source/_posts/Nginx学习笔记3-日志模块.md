title: Nginx学习笔记3 日志模块
author: peace
tags:
  - Nginx
categories:
  - 编程
date: 2018-11-26 22:45:00
---
From: <http://nginx.org/en/docs/http/ngx_http_log_module.html>
前两篇介绍Nginx的文章，全是选自Nginx官方文档，因为通俗易懂，比很多中文资料讲的都要透彻，并且很容易上手操作，所以就放了因为原文。读日志模块英文文档的时候，发现有些地方比较吃力，所以就得加入一些中文和图示了。
<!-- more -->
##### 首先看一下真实的日志配置
```
http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;


    log_format mylog_format '$remote_addr|$remote_user|$time_local'
                           '$request|$status|$bytes_sent'
                           '$http_referer|$http_user_agent|$gzip_ratio'
                           '#$bytes_sent|$connection|$connection_requests|$msec|$pipe|$request_length|$request_time|$status|$time_iso8601|$time_local';

    access_log /data/nginx/logs/access.log mylog_format;

    server {
        listen 8091;
        location / {
            proxy_pass http://localhost:8080;
        }

        location ~ \.(gif|jpg|png)$ {
            root /data/images/;
        }

    	location /software {
    		root /data;
    	}
    }


    server {
        listen 8080;
        access_log /data/nginx/logs/access0.log mylog_format;
        root /data/www;
        location / {
        }
    }

    server {
        listen 8081;
        access_log /data/nginx/logs/access1.log mylog_format;
        root /data/www1;
        location / {
        }
    }

    server {
        listen 8082;
        access_log /data/nginx/logs/access2.log mylog_format;
        root /data/www2;
        location / {
        }
    }

    upstream myapp1 {
        server localhost:8080;
        server localhost:8081;
        server localhost:8082;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://myapp1;
        }
    }
    ...
```
默认的日志格式已经通过#注释掉了，下边是我定义的一个log_format
```
    log_format mylog_format '$remote_addr|$remote_user|$time_local'
                           '$request|$status|$bytes_sent'
                           '$http_referer|$http_user_agent|$gzip_ratio'
                           '#$bytes_sent|$connection|$connection_requests|$msec|$pipe|$request_length|$request_time|$status|$time_iso8601|$time_local';
```
上边的log_format将很多参数以|的形式放在一行，最后日志的输出格式如下：
```
127.0.0.1|-|28/Nov/2018:21:24:11 +0800GET / HTTP/1.0|200|242-|Mozilla/5.0 (YunSecurityBot/1.0) Gecko/20100101 Firefox/36.0|-#242|5524|1|1543411451.623|.|187|0.000|200|2018-11-28T21:24:11+08:00|28/Nov/2018:21:24:11 +0800
```
其中下边的语句设置了access_log(也就是是请求访问的日志)日志使用上边定义的mylog_format格式，并且日志的位置为/data/nginx/logs/access.log
```
 access_log /data/nginx/logs/access.log mylog_format;
```
看下access.log日志文件的位置：
```
[root@server1 conf]# ll /data/nginx/logs/access0.log
-rw-r--r-- 1 root root 205870 11月 28 21:24 /data/nginx/logs/access0.log
```
上边access_log定义放在http模块里，也可以放到server里，比如上边的定义，访问8080端口的日志记录在/data/nginx/logs/access0.log文件里，访问8081端口的记录在/data/nginx/logs/access1.log，访问8082端口的记录在/data/nginx/logs/access2.log文件里。

上边是一个实际的例子。具体的日志设置细节还是建议看英文原文。

## Module ngx_http_log_module 


The `ngx_http_log_module` module writes request logs in the specified format. 

Requests are logged in the context of a location where processing ends. It may be different from the original location, if an [internal redirect][1] happens during request processing. 

   [1]: ngx_http_core_module.html#internal

#### Example Configuration 

> log_format compression '$remote_addr - $remote_user [$time_local] '
                       '"$request" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent" "$gzip_ratio"';    
                       
> access_log /spool/logs/nginx-access.log compression buffer=32k;

#### Directives 

> `Syntax`:	**access_log** path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];
	**access_log** off;
`Default`:	access_log logs/access.log combined;
`Context`:	http, server, location, if in location, limit_except


Sets the path, format, and configuration for a buffered log write. Several logs can be specified on the same level. Logging to [syslog][2] can be configured by specifying the “`syslog:`” prefix in the first parameter. The special value `off` cancels all `access_log` directives on the current level. If the format is not specified then the predefined “`combined`” format is used. 

   [2]: ../syslog.html

If either the `buffer` or `gzip` (1.3.10, 1.2.7) parameter is used, writes to log will be buffered. 

<p style="border:  1px dotted;"> The buffer size must not exceed the size of an atomic write to a disk file. For FreeBSD this size is unlimited. </p>

When buffering is enabled, the data will be written to the file: 

  * if the next log line does not fit into the buffer;
  * if the buffered data is older than specified by the `flush` parameter (1.3.10, 1.2.7);
  * when a worker process is [re-opening][3] log files or is shutting down.

   [3]: ../control.html

If the `gzip` parameter is used, then the buffered data will be compressed before writing to the file. The compression level can be set between 1 (fastest, less compression) and 9 (slowest, best compression). By default, the buffer size is equal to 64K bytes, and the compression level is set to 1. Since the data is compressed in atomic blocks, the log file can be decompressed or read by “`zcat`” at any time. 

Example: 

> access_log /path/to/log.gz combined gzip flush=5m;
>     
>     

<p style="border:  1px dotted;"> For gzip compression to work, nginx must be built with the zlib library. </p>

The file path can contain variables (0.7.6+), but such logs have some constraints: 

  * the [user][4] whose credentials are used by worker processes should have permissions to create files in a directory with such logs;
  * buffered writes do not work;
  * the file is opened and closed for each log write. However, since the descriptors of frequently used files can be stored in a cache, writing to the old file can continue during the time specified by the open_log_file_cache directive’s `valid` parameter
  * during each log write the existence of the request’s [root directory][5] is checked, and if it does not exist the log is not created. It is thus a good idea to specify both [root][5] and `access_log` on the same level:

   [4]: ../ngx_core_module.html#user
   [5]: ngx_http_core_module.html#root
   
```
server {
         root       /spool/vhost/data/$host;
         access_log /spool/vhost/logs/$host;
         ... 
       }
```

The `if` parameter (1.7.0) enables conditional logging. A request will not be logged if the `_condition_` evaluates to “0” or an empty string. In the following example, the requests with response codes 2xx and 3xx will not be logged: 

```
 map $status $loggable {
         ~^[23]  0;
         default 1;
     }
     
access_log /path/to/access.log combined if=$loggable;
```

the log_format follows the next syntax :

> Syntax:**log_format** _name_ [`escape`=`default`|`json`|`none`] `string` ...;  
Default: log_format combined "...";
Context: `http` 


Specifies log format. 

The `escape` parameter (1.11.8) allows setting `json` or `default` characters escaping in variables, by default, `default` escaping is used. The `none` value (1.13.10) disables escaping. 

The log format can contain common variables, and variables that exist only at the time of a log write: 

`$bytes_sent`
    the number of bytes sent to a client 
`$connection`
    connection serial number 
`$connection_requests`
    the current number of requests made through a connection (1.1.18) 
`$msec`
    time in seconds with a milliseconds resolution at the time of the log write 
`$pipe`
    “`p`” if request was pipelined, “`.`” otherwise 
`$request_length`
    request length (including request line, header, and request body) 
    request processing time in seconds with a milliseconds resolution; time elapsed between the first bytes were read from the client and the log write after the last bytes were sent to the client 
`$status`
    response status 
    local time in the ISO 8601 standard format 
    local time in the Common Log Format 

> In the modern nginx versions variables [$status][6] (1.3.2, 1.2.2), [$bytes_sent][7] (1.3.8, 1.2.5), [$connection][8] (1.3.8, 1.2.5), [$connection_requests][9] (1.3.8, 1.2.5), [$msec][10] (1.3.9, 1.2.6), [$request_time][11] (1.3.9, 1.2.6), [$pipe][12] (1.3.12, 1.2.7), [$request_length][13] (1.3.12, 1.2.7), [$time_iso8601][14] (1.3.12, 1.2.7), and [$time_local][15] (1.3.12, 1.2.7) are also available as common variables. 

   [6]: ngx_http_core_module.html#var_status
   [7]: ngx_http_core_module.html#var_bytes_sent
   [8]: ngx_http_core_module.html#var_connection
   [9]: ngx_http_core_module.html#var_connection_requests
   [10]: ngx_http_core_module.html#var_msec
   [11]: ngx_http_core_module.html#var_request_time
   [12]: ngx_http_core_module.html#var_pipe
   [13]: ngx_http_core_module.html#var_request_length
   [14]: ngx_http_core_module.html#var_time_iso8601
   [15]: ngx_http_core_module.html#var_time_local

Header lines sent to a client have the prefix “`sent_http_`”, for example, `$sent_http_content_range`. 

The configuration always includes the predefined “`combined`” format: 

> log_format combined '$remote_addr - $remote_user [$time_local] '
>                         '"$request" $status $body_bytes_sent '
>                         '"$http_referer" "$http_user_agent"';
>     
>     

Syntax:
`**open_log_file_cache**` max=`_N_` [`inactive`=`_time_`] [`min_uses`=`_N_`] [`valid`=`_time_`];  
`**open_log_file_cache**` off;  


Default:
    
    open_log_file_cache off;
    

Context:
`http`, `server`, `location`  


Defines a cache that stores the file descriptors of frequently used logs whose names contain variables. The directive has the following parameters: 

`max`
    sets the maximum number of descriptors in a cache; if the cache becomes full the least recently used (LRU) descriptors are closed 
`inactive`
    sets the time after which the cached descriptor is closed if there were no access during this time; by default, 10 seconds 
`min_uses`
    sets the minimum number of file uses during the time defined by the `inactive` parameter to let the descriptor stay open in a cache; by default, 1 
`valid`
    sets the time after which it should be checked that the file still exists with the same name; by default, 60 seconds 
`off`
    disables caching 

Usage example: 

> open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
>     
>     
