title: linux中shell 特殊变量$0 $n $* $@ $! $?的详解
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-04-26 17:34:00
---
From http://www.111cn.net/sys/linux/79750.htm

### $0:获取当前执行脚本的文件名，包括路径。

```
[root@test script]# cat 0.sh 
#!/bin/bash
echo $0
```
```
[root@test script]# sh 0.sh 
0.sh
```
```
[root@test script]# cat 0.sh 
#!/bin/bash
dirname "$0"
basename "$0"
```
```
[root@test script]# sh /byrd/script/0.sh 
/byrd/script
0.sh
```
<!-- more -->

### $n:获取当前执行的shell脚本的第N个参数，n=1..9，当n为0时表示脚本的文件名，如果n大于9，用大括号括起来like${10}.

```
[root@test script]# cat n.sh 
#!/bin/bash
echo $1 $2 ${10}
```
```
[root@test script]# sh n.sh a b c d e f g h i j k l m n
a b j
[root@LAMP script]# sh n.sh {a..z}
a b j
[root@test script]# sh n.sh `seq 11` 
1 2 10
```

### $*:获取当前shell的所有参数，将所有的命令行参数视为单个字符串。
$@:这个程序的所有参数"$1" "$2" "$3" "...",这是将参数传递给其他程序的最佳方式，因此TA会保留所有内嵌在每个参数里的任何空白。
#### $#:获取当前shell命令行中参数的总个数。

```
[root@test script]# cat hashtag.sh 
#!/bin/bash
echo "$#"
```
```
[root@test script]# sh hashtag.sh 
0
[root@test script]# sh hashtag.sh 1 2 3
3
[root@test script]# sh hashtag.sh `seq 300`
300
```
```
[root@test script]# cat example.sh 
#!/bin/bash
#Example
if [ $# -ne 2 ];then
    echo "Error, please enter two parameters."
    exit 1
else
    echo "You did a good job."
fi
```
```
[root@test script]# sh example.sh a 
Error, please enter two parameters.
[root@test script]# sh example.sh a b
You did a good job.
[root@test script]# sh example.sh a b c
Error, please enter two parameters.
```
### $_:代表上一个命令的最后一个参数
### $$:代表所在命令的PID

```
[root@LAMP script]# cat dollar.sh 
#!/bin/bash
echo "$$" >/tmp/dollar.pid
while true
do
    sleep 1
done
```
```
[root@LAMP script]# sh dollar.sh 
```
################################################
```
[root@LAMP ~]# cat /tmp/dollar.pid 
1483
[root@LAMP ~]# ps -ef |grep 1483
root      1483  1453  0 14:58 pts/1    00:00:00 sh dollar.sh
root      1532  1483  0 14:58 pts/1    00:00:00 sleep 1
root      1534  1496  0 14:58 pts/0    00:00:00 grep 1483
[root@LAMP ~]# ps -ef |grep dollar
root      1483  1453  0 14:58 pts/1    00:00:00 sh dollar.sh
root      1555  1496  0 14:58 pts/0    00:00:00 grep dollar
```

### $!:代表最后执行的后台命令的PID
### $?:代表上一个命令执行是否成功的标志，如果执行成功则$? 为0，否则不为0

```
[byrd@LAMP script]$ pwd
/byrd/script
[byrd@LAMP script]$ echo $?
0    #运行成功
[byrd@LAMP script]$ ls /root
ls: cannot open directory /root: Permission denied
[byrd@LAMP script]$ echo $?
2    #权限拒绝
[byrd@LAMP script]$ hahaha
-bash: hahaha: command not found
[byrd@LAMP script]$ echo $?

127    #未找到该命令
```

###########################################
```
[byrd@LAMP ~]$ cat /byrd/script/question_mark.sh 
#!/bin/bash
#Example
ls -al /root >/dev/null 2>&1 
if [ $? -eq 0 ];then
    echo "User is root"
else
    echo "The user is not root"
fi
```
```
[root@LAMP script]# sh question_mark.sh 
User is root
[root@LAMP script]# su - byrd
[byrd@LAMP ~]$ sh /byrd/script/question_mark.sh 
The user is not root
```