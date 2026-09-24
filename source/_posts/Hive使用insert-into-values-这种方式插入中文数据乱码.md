title: 'Hive使用insert into values 这种方式插入中文数据乱码 '
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-11 11:05:00
---
插入数据时，要将中文数据转码为 iso8859-1  
new String("测试数据".getBytes(),"iso8859-1");
