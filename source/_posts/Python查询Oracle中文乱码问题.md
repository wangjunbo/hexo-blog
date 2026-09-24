title: Python查询Oracle中文乱码问题
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-12-11 11:19:00
---
用python连接Oracle是总是乱码，最后发现时oracle客户端的字符编码设置不对。

编写的python脚本中需要加入如下几句：

import os
os.environ['NLS_LANG'] = 'SIMPLIFIED CHINESE_CHINA.UTF8'

这样可以保证select出来的中文显示没有问题。

要能够正常的insert和update中文，还需要指定python源文件的字符集密码和oracle一致。
```

# -*- coding: utf-8 -*-
```
参考 https://blog.csdn.net/jianhong1990/article/details/26487479
