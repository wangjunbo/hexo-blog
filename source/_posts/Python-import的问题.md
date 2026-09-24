title: Python import的问题
author: peace
tags:
  - Python
  - ''
categories:
  - 编程
date: 2023-04-13 02:03:00
---
在目录下建立__init__.py文件，
任何Python文件在引用其他py文件的时候，包含引用同级的py文件，都使用完整包名。比如

![main.py](/images/pasted-65.png)

![a.py](/images/pasted-66.png)

![b.py](/images/pasted-67.png)
