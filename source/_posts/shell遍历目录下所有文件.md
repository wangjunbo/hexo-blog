title: shell遍历目录下所有文件
tags:
  - Linux
categories: []
date: 2016-02-19 14:52:00
---
```shell
filelist=`ls /home/work/file/`
for file in $filelist
do 
 echo $file
done
```
一定要切记filelist=后边的那个引号不是单引号，而是tab键上边的那个键，或者说是1左边的那个键。否则的话不起作用。

转自http://blog.163.com/clevertanglei900@126/blog/static/111352259201162553652150