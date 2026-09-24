title: 多个程序向同一个kafka topic写入数据
tags:
  - 大数据
categories: []
date: 2016-02-25 14:15:00
---
经测试, 在多个程序同时向同一个kafka topic中写入数据, 不会出现数据冲突或者丢失的情况.

##### 测试数据:  
###### 向kafka中写数据  
program instance 1 : 10000条数据  
program instance 2 : 10000条数据  
program instance 3 : 10000条数据  

同时启动上述3个程序, 程序每次写完一条数据,sleep 50ms.

###### 从kafka中读取数据
共读取到30000条数据

###### 结论
在多个程序同时向同一个kafka topic中写入数据, 不会出现数据冲突或者丢失的情况.
