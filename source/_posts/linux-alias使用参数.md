title: Linux 向alias传参数
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-05-09 09:41:00
---
![Linux 向alias传参数](/css/images/uploadfile.png)
参考以下代码
在~/.bashrc文件中增加如下行
```
alias uploadfile='a() { scp $1 root@google.com:/data/resource/;}; a'
```
然后使文件生效
```
source ~/.bashrc
```
使用如下代码上传
```
MacBookPro:Desktop hohode$ uploadfile sms.jpeg
```
注意分号，之前因为少了这个分号，困扰了我好久。