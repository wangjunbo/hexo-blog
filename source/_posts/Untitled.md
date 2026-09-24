title: Linux文件删除，但是磁盘空间没有释放
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2019-03-07 21:38:00
---
Linux 磁盘空间总是报警，查到到大文件，删除之后，df看到磁盘空间并没有释放。

查找了下发现系统对rm进行了alias ，因为Linux对删除操作没有回收站机制，对rm操作进行了自定义，对删除文件进行移动到/tmp 目录里面。

又对/temp删除 但是还是没有发现磁盘冲击释放 

执行  
```
lsof | grep deleted
```
发现有大量刚刚删除文件的进程存在，通过kill -9 关闭进程（或者重启进程）   OK
参考 https://www.cnblogs.com/xd502djj/p/6668632.html