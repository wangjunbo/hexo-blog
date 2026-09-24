title: Python为程序添加系统路径
author: peace
tags:
  - Python
categories:
  - 编程
date: 2021-11-04 07:00:00
---
将当前文件夹添加到Python系统路径下，也就是sys.path变量中
```
from os.path import abspath, join, dirname
filepath = abspath(dirname(__file__))
print("filepath={}".format(filepath))
sys.path.insert(0, filepath)
print(sys.path)
```
参考 https://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p09_add_directories_to_sys_path.html