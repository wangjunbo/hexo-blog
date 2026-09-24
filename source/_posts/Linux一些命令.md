title: Linux一些命令
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-04-26 18:09:00
---
[Linux命令eval的用法](http://blog.51cto.com/363918/1341977)

[Shell脚本中实现切换用户并执行命令操作](http://www.jb51.net/article/59255.htm)

[查看linux中某个端口（port）是否被占用](https://blog.csdn.net/hsd2012/article/details/51384907)   
1.使用lsof   
lsof -i:端口号查看某个端口是否被占用  
2.使用netstat   
使用netstat -anp|grep 80

[linux expect详解(ssh自动登录)](http://www.cnblogs.com/lzrabbit/p/4298794.html)

[Linux下su与su -命令的本质区别](http://yanue.net/post-90.html)

### Linux shell日期的加减和格式化
```
d=$(date +%Y%m%d  --date='1 days ago ')
```
### Linux shell单引号内的参数
```
d=$(date +%Y%m%d  --date=''${1}' days ago ')
```
### vim 取消行号
```
set nonu
```
### [grep的用法](https://www.computerhope.com/unix/ugrep.htm)
```
grep --color -n -i -r 'dataThatYouWhatToFind' *
```
-n 显示在文件中的第几行  
-i 忽略大小写  
-r 搜索包含子目录

### [grep如何递归目录并在指定类型文件中查找](https://blog.csdn.net/JoeBlackzqq/article/details/7531762)
```
find . -name '*.xml' | xargs grep 'sometext'
```

### ls按照创建时间排序
```
man ls
```
```
-c     with -lt: sort by, and show, ctime (time of last modification of file status information); with -l: show ctime and sort by name; otherwise: sort by ctime, newest first
```
```
ls -ltc
```
### linux中的日期加减
```
d=$(date +%Y-%m -d '-1 month')
```

### Autojump
[一个可以在 Linux 文件系统快速导航的高级 cd 命令](https://linux.cn/article-5983-1.html)
对于linux系统:
```
echo '. /usr/share/autojump/autojump.sh' >> /etc/profile
source /etc/profile
```

### 删除文件中的空行或者空白行
```
sed '/^ *$/d' file1
```