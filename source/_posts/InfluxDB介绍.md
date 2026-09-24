title: InfluxDB介绍
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2018-03-13 13:39:00
---
参考 http://googleapi.cc/000002  
Influxdb是目前比较流行的开源分布式时序性数据库。
什么是时间序列数据库，最简单的定义就是数据格式里包含Timestamp字段的数据。  

它有三大特性：
1. 时序性（Time Series）：与时间相关的函数的灵活使用（诸如最大、最小、求和等）；
2. 度量（Metrics）：对实时大量数据进行计算；
3. 事件（Event）：支持任意的事件数据，换句话说，任意事件的数据我们都可以做操作。

其它特点：
* schemaless(无结构)，可以是任意数量的列；
* min, max, sum, count, mean, median，内置很多实用的函数
* 查询与类似sql
* 很好的图形化管理工具

Influxdb相关名词
* database：数据库；
* measurement：数据库中的表；
* points：表里面的一行数据。

InfluxDB中独有的一些概念
Point由时间戳（time）、数据（field）和标签（tags）组成。
* time：每条数据记录的时间，也是数据库自动生成的主索引；
* fields：各种记录的值；
* tags：各种有索引的属性。
