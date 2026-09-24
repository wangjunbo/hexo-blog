title: zeppelin  spark java.lang.NullPointerException
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2019-09-11 17:59:00
---
root用户通过ps aux | grep zeppelin 可以看到相关的进程信息和启动目录

先确定启动进行所用的用户，然后把所有进程kill

然后使用./zeppelin-daemon.sh start启动
