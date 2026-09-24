title: Linux top命令之VIRT
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-02-27 17:51:00
---
Top命令监控某个进程的资源占有情况 下面是各种内存： VIRT：virtual memory usage  
1、进程“需要的”虚拟内存大小，包括进程使用的库、代码、数据等  
2、假如进程申请100m的内存，但实际只使用了10m，那么它会增长  100m，而不是实际的使用量  
RES：resident memory usage 常驻内存  
1、进程当前使用的内存大小，但不包括swap out  
2、包含其他进程的共享  
3、如果申请100m的内存，实际使用10m，它只增长10m，与VIRT相反  
4、关于库占用内存的情况，它只统计加载的库文件所占内存大小  