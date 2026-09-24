title: hive使用动态分区插入数据
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2019-01-09 23:25:00
---
 往hive分区表中插入数据时，如果需要创建的分区很多，比如以表中某个字段进行分区存储，则需要复制粘贴修改很多sql去执行，效率低。因为hive是批处理系统，所以hive提供了一个动态分区功能，其可以基于查询参数的位置去推断分区的名称，从而建立分区。

### 1.创建一个单一字段分区表
```
hive>
   create table dpartition(id int ,name string )
   partitioned by(ct string  );
```
<!-- more -->
### 2.往表里装载数据，并且动态建立分区，以city建立动态分区
   ```
hive>
 hive.exec.dynamici.partition=true;  #开启动态分区，默认是false
 set hive.exec.dynamic.partition.mode=nonstrict; #开启允许所有分区都是动态的，否则必须要有静态分区才能使用。
 insert overwrite table dpartition
 partition(ct)
 select id ,name,city from  mytest_tmp2_p; 
 ```
要点：因为dpartition表中只有两个字段，所以当我们查询了三个字段时（多了city字段），所以系统默认以最后一个字段city为分区名，因为分区表的
分区字段默认也是该表中的字段，且依次排在表中字段的最后面。所以分区需要分区的字段只能放在后面，不能把顺序弄错。如果我们查询了四个字段的话，则会报
错，因为该表加上分区字段也才三个。要注意系统是根据查询字段的位置推断分区名的，而不是字段名称。
hive>--查看可知，hive已经完成了以city字段为分区字段，实现了动态分区。
```
hive (fdm_sor)> show partitions dpartition;
partition
ct=beijing
ct=beijing1
```
注意：使用，insert...select 往表中导入数据时，查询的字段个数必须和目标的字段个数相同，不能多，也不能少,否则会报错。但是如果字段的类型不一致的话，则会使用null值填充，不会报错。而使用load data形式往hive表中装载数据时，则不会检查。如果字段多了则会丢弃，少了则会null值填充。同样如果字段类型不一致，也是使用null值填充。

### 3.多个分区字段时，实现半自动分区（部分字段静态分区，注意静态分区字段要在动态前面）

1.创建一个只有一个字段，两个分区字段的分区表
```
hive (fdm_sor)> create table ds_parttion(id int ) 
              > partitioned by (state string ,ct string );
```
2.往该分区表半动态分区插入数据 
```
hive>
 set hive.exec.dynamici.partition=true;
 set hive.exec.dynamic.partition.mode=nonstrict;
 insert overwrite table ds_parttion
 partition(state='china',ct)  #state分区为静态，ct为动态分区，以查询的city字段为分区名
 select id ,city from  mytest_tmp2_p; 
```

3.查询结果显示：

```
hive (fdm_sor)> select *  from ds_parttion where state='china' ;
ds_parttion.id  ds_parttion.state       ds_parttion.ct
4       china   beijing
3       china   beijing
2       china   beijing
1       china   beijing
4       china   beijing1
3       china   beijing1
2       china   beijing1
1       china   beijing1
 
hive (fdm_sor)> select *  from ds_parttion where state='china' and ct='beijing';
ds_parttion.id  ds_parttion.state       ds_parttion.ct
4       china   beijing
3       china   beijing
2       china   beijing
1       china   beijing
 
hive (fdm_sor)> select *  from ds_parttion where state='china' and ct='beijing1';
ds_parttion.id  ds_parttion.state       ds_parttion.ct
4       china   beijing1
3       china   beijing1
2       china   beijing1
1       china   beijing1
Time taken: 0.072 seconds, Fetched: 4 row(s)
```
4.多个分区字段时，全部实现动态分区插入数据
```
 set hive.exec.dynamici.partition=true;
 set hive.exec.dynamic.partition.mode=nonstrict;
 insert overwrite table ds_parttion
 partition(state,ct)
 select id ,country,city from  mytest_tmp2_p; 
 ```
注意：字段的个数和顺序不能弄错。
5.动态分区表的属性

  使用动态分区表必须配置的参数 ：
```
    set hive.exec.dynamic.partition =true（默认false）,表示开启动态分区功能
    set hive.exec.dynamic.partition.mode = nonstrict(默认strict),表示允许所有分区都是动态的，否则必须有静态分区字段
```

动态分区相关的调优参数：

set  hive.exec.max.dynamic.partitions.pernode=100 （默认100，一般可以设置大一点，比如1000）表示每个maper或reducer可以允许创建的最大动态分区个数，默认是100，超出则会报错。

   set hive.exec.max.dynamic.partitions =1000(默认值) 表示一个动态分区语句可以创建的最大动态分区个数，超出报错

set hive.exec.max.created.files =10000(默认) 全局可以创建的最大文件个数，超出报错。
--------------------- 
from https://blog.csdn.net/qq_26442553/article/details/80382174 
