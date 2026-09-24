title: LogServer服务效率的优化
author: peace
tags:
  - 服务器
categories:
  - 编程
date: 2020-02-11 23:11:00
---
### 1 TCP: out of memory -- consider tuning tcp_mem
```
2018/05/08 08:56:29 [error] 27#27: *225448 readv() failed (104: Connection reset by peer) while reading upstream, client: 47.xx.xx.43, server: cdn.xxx.com, request: "GET /home/css/img/area-214f1779.png HTTP/1.1", upstream: "http://xx.xx.xx.xx:80/home/css/img/area-214f1779.png", host: "cdn.xxx.com", referrer: "http://cdn.xxx.com/home/css/index-1.0.9.css"
```
[可能是TCP连接的内存不够引起的。](https://yeadoc.cn/2018/05/15/TCP-MEM%E5%B0%8F%E5%AF%BC%E8%87%B4%E4%B8%8B%E8%BD%BD%E6%85%A2%E6%88%96%E6%97%A0%E6%B3%95%E4%B8%8B%E8%BD%BD/)  
[比较详细的查看方式](https://blog.csdn.net/yiyeguzhou100/article/details/52049797)

2 time_wait
https://blog.51cto.com/hld1992/2285410
