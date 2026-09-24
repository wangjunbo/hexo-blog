title: Python将文件读取成不带换行符的字符串数组
author: peace
tags:
  - Python
categories:
  - 编程
date: 2019-02-24 16:11:00
---
Reading a file without newlines

You can read the whole file and split lines using str.splitlines:
```
temp = file.read().splitlines()
```
Or you can strip the newline by hand:
```
temp = [line[:-1] for line in file]
```
*Note*: this last solution only works if the file ends with a newline, otherwise the last line will lose a character.

from https://stackoverflow.com/questions/12330522/reading-a-file-without-newlines