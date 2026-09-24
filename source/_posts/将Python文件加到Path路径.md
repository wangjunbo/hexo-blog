title: Python 引用其他目录的文件
author: peace
tags:
  - Python
categories:
  - 编程
date: 2019-04-22 20:02:00
---
将你的文件加入Python路径方法有很多种，以下我认为比较好的一种方式。
创建一个.pth文件，将目录列举出来，像这样：
```
# myapplication.pth
/Users/xiaowang/PycharmProjects/hohode/src/util
```

这个.pth文件需要放在某个Python的site-packages目录，通常位于/usr/local/lib/python3.3/site-packages 或者 ~/.local/lib/python3.3/sitepackages。比如我的文件路径为/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/myapplication.pth目录。
当解释器启动时，.pth文件里列举出来的存在于文件系统的目录将被添加到sys.path。
查看sys.path
```
Peace:site-packages hohode$ python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 26 2018, 23:26:24)
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/Library/Frameworks/Python.framework/Versions/3.7/lib/python37.zip', '/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7', '/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/lib-dynload', '/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages', '/Users/xiaowang/PycharmProjects/hohode/src/util']
>>> import wcommon
>>> wcommon.getLocalIp()
'222.21.126.112'
```
安装一个.pth文件可能需要管理员权限，如果它被添加到系统级的Python解释器。
from https://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p09_add_directories_to_sys_path.html