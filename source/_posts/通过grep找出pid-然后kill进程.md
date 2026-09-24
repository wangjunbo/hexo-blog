title: '通过grep找出pid,然后kill进程'
author: peace
tags:
  - Linux
  - ''
categories:
  - 编程
date: 2018-03-13 09:50:00
---
通过grep找出pid,然后kill进程   
```shell
pid=`ps aux | grep java | grep -v grep | awk '{print $2}' `
echo pid is $pid
if [ -z "$pid" ]
then
  echo "pid is empty"
else
  echo "pid exists, will kill it"
  kill -9 $pid
fi
```