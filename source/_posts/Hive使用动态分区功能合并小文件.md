title: Hive使用动态分区功能合并小文件
author: peace
tags:
  - Hive
categories:
  - 编程
date: 2020-01-06 16:23:00
---
下列语句在Hive命令行可以执行，但是在hue上好像不可以。

```
hive> SET hive.exec.dynamic.partition = true;           // 开启动态分区，默认是false
hive> SET hive.exec.dynamic.partition.mode = nonstrict; // 允许所有分区都是动态的，否则必须要有静态分区才能使用
hive> set hive.merge.smallfiles.avgsize = 100000000;    // 平均文件大小，是决定是否执行合并操作的阈值，默认16000000

hive> insert into TABLE test.tmp_work_mc1 partition(day,product)  SELECT id,emp,pv,uv,spv,fpv,fpv_add,fpv_del,day,product from ods.work_mc  ;
```

https://blog.csdn.net/qq_26442553/article/details/80382174
http://shzhangji.com/cnblogs/2014/04/07/hive-small-files/