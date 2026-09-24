title: 'Python快速实现文件上传下载  '
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-05-31 11:12:00
---

[使用Python下载文件](http://hohode.com/2018/05/09/Linux%E8%87%AA%E5%AE%9A%E4%B9%89%E6%9C%89%E7%94%A8%E7%9A%84%E8%84%9A%E6%9C%AC/)

### 首先安装 Pyftpdlib 模块
```
pip install pyftpdlib
```

### 启动ftp服务器
```
python -m pyftpdlib -p 21  -w -d /data/transfer/ -u test -P suibian
```

### 在linux上安装ftp客户端
```
yum instal -y ftp
```

[在linux下载ftp服务器上的数据](http://www.cnblogs.com/weafer/archive/2011/06/13/2079509.html)

一直出现 227 Entering passive mode (192,168,85,81,143,77)  
很可能是[外网访问内网](https://blog.csdn.net/iPenX/article/details/78081361)，
在其他的机器上访问可能就没问题。


pip: 未找到命令  
使用which python找到python的路径  
在Python路径中一般有pip或者pip3

参考 https://www.hi-linux.com/posts/10247.html