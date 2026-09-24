title: Linux安装npm
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-02-27 23:15:00
---
a 下载地址
https://nodejs.org/en/download/

b 解压，分两步进行
```
xz -d node-v8.9.4-linux-x64.tar.xz
tar -xvf node-v8.9.4-linux-x64.tar
```

c 配置环境变量
修改/etc/profile的PATH变量
```
export PATH=$PATH:/data/software/node-v8.9.4-linux-x64/bin
```

d 使环境变量生效
```
source /etc/profile
```