title: Linux自定义有用的脚本
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-05-09 09:47:00
---
### 1 在Linux服务器上启动一个简单的Python服务
python2
```
nohup python -m SimpleHTTPServer 55555 &
```
[python3](https://stackoverflow.com/questions/7943751/what-is-the-python-3-equivalent-of-python-m-simplehttpserver)
```
python -m http.server 55555
```

### 2 从简单Python服务器上下载文件
```
alias download='a() { wget 21.10.13.14:55555/$1;}; a'
```
### 3 [rm -rf,mv 的替换工具 wmv](https://github.com/wangjunbo/shell_tool)
```
wget http://47.91.233.243/static/res/wmv && chmod 777 wmv && mv wmv /usr/bin/
```