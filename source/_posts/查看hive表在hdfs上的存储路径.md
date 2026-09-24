title: 查看hive表在hdfs上的存储路径
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-10 17:27:00
---

1、执行hive，进入hive窗口   

2、执行show databases,查看所有的database;   

3、执行use origin_ennenergy_onecard; 则使用origin_ennenergy_onecard数据库   

4、执行show create table M_BD_T_GAS_ORDER_INFO_H;则可以查看table在hdfs上的存储路径 

<!-- more -->

如下：   
```
hive (origin_ennenergy_onecard)> show create table M_BD_T_GAS_ORDER_INFO_H;

OK
CREATE TABLE `M_BD_T_GAS_ORDER_INFO_H`(
  `fguid` string, 
  `fstationno` string, 
  `fstationname` string, 
  `fgunno` int, 
  `fserialno` int, 
  `fgas` double, 
  `fprice` double, 
  `fmoney` double, 
  `fsumgas` double, 
  `ftradedatetime` date, 
  `fstopdatetime` date, 
  `fsavedatetime` date, 
  `ffueltype` string, 
  `recorddate` date)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '\t' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  'hdfs://mycluster/user/hive/warehouse/origin_ennenergy_onecard.db/m_bd_t_gas_order_info_h'      -----标红部分为hdfs的路径
TBLPROPERTIES (
  'COLUMN_STATS_ACCURATE'='true', 
  'numFiles'='6', 
  'numRows'='3546198', 
  'rawDataSize'='435279808', 
  'totalSize'='438826006', 
  'transient_lastDdlTime'='1468831756')
Time taken: 0.279 seconds, Fetched: 30 row(s)
```

备注：   

hive其他命令：   
```
show functions      ----->查看所有的hive函数   
desc tablesname ------>查看table的表结构  
```