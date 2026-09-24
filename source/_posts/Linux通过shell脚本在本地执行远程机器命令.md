title: Linux通过shell脚本在本地执行远程机器命令
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-08-03 10:03:23
---
Linux通过shell脚本在本地执行远程机器命令

```
#!/bin/bash

ssh root@hohode.com << remotessh         

####从这里开始都是在远程机器上执行命令啦

cd /data/hexo/blog ; 
pwd;  
hexo g

#####执行完毕

exit  ###不要忘记退出远程机器
remotessh  ###还有这里的结尾哦，不要忘记
```
https://blog.csdn.net/sn3009/article/details/52779642
