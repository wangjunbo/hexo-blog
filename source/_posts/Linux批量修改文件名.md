title: Linux批量修改文件名
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2020-02-22 18:08:00
---
Linux自带的有rename命令。具体可以执行谷歌其用法，不过这个命令使用起来比较局限，还有更强大的批量修改文件名的rename的命令，不过这个是perl版本的，这样就和Linux系统自带的命令冲突了。

不过，不要担心，Follow me。

1 安装perl版本的rename
```
yum -y install prename
```

2 使用prename批量修改文件名
```
prename 's/log/log.bak/' *
```
将所有文件中log文字修改成log.bak

