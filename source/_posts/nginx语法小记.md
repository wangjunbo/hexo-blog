title: nginx语法小记
author: peace
tags:
  - Nginx
categories:
  - 编程
date: 2018-10-18 19:46:00
---

### least_conn [负载均衡的算法](https://blog.csdn.net/zhangskd/article/details/50242241)


我们知道轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同。 

这有个前提，就是每个请求所占用的后端时间要差不多，如果有些请求占用的时间很长，会导致其所在的后端负载较高。在这种场景下，把请求转发给连接数较少的后端，能够达到更好的负载均衡效果，这就是least_conn算法。  

least_conn算法很简单，首选遍历后端集群，比较每个后端conns/weight，选取该值最小的后端。

如果有多个后端的conns/weight值同为最小的，那么对它们采用加权轮询算法。

如果有least_conn指令，表示使用least connected负载均衡算法。

### log_format [日志格式设定](https://www.cnblogs.com/kevingrace/p/5893499.html) 

log_format指令用来设置日志的记录格式，它的语法如下：
```

log_format name format {format ...}
其中name表示定义的格式名称，format表示定义的格式样式。
 
log_format有一个默认的、无须设置的combined日志格式设置，相当于Apache的combined日志格式，其具体参数如下：
log_format combined '$remote_addr-$remote_user [$time_local]'
‘"$request"$status $body_bytes_sent’
 ‘"$http_referer" "$http_user_agent"’
 
```

### access_log 用来指定日志文件的存放路径、格式（把定义的log_format 跟在后面）和缓存大小；如果不想启用日志则access_log off ;  
location 用来匹配来访的url https://segmentfault.com/a/1190000013267839
proxy_redirect https://blog.csdn.net/u010391029/article/details/50395680
X-Real-IP 这个X-real-ip是一个自定义的变量名，名字可以随意取，这样做完之后，用户的真实ip就被放在X-real-ip这个变量里了，然后，在web端可以这样获取：
request.getAttribute("X-real-ip")
    http://gong1208.iteye.com/blog/1559835

root 需要本地文件 https://www.jianshu.com/p/4be0d5882ec5
