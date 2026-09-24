title: Hive学习
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-05-31 10:30:00
---
### 在Spark配置hive的hive.metastore.uris  
在spark的conf/hive-site.xml中配置  
```
  <property>
    <name>hive.metastore.uris</name>
    <value>thrift://hadoop3:9083</value>
  </property>
```
hive.metastore.uris可以看cloudera manager的群集->Hive->实例：  
Hive Metastore Server对应的服务器地址。
### 秒杀一切udf的reflect函数
[hive新特性reflect函数介绍](https://www.cnblogs.com/hustzzl/p/7887022.html)

### Hive collect_set,concat_ws
collect_set 是 Hive 内置的一个聚合函数, 它返回一个消除了重复元素的对象集合, 其返回值类型是 array 。  
```
select av_seq, concat_ws(',', collect_set(cp_seq)) from dw.smbrandcp group by av_seq;
```
### Hive获取array数组长度
使用size(array)方法
```
select size(split(cook_ids)) from user_cook_recommed
```
### java.io.IOException: Failed to run job : Application rejected by queue placement policy
有可能是没有使用正确的用户去执行hive命令，比如本该用hdfs用户的，但是却用root用户执行了。

### 为hive -e设置mapper数量和MapReduce的name
```
hive -e "set mapred.reduce.tasks=1; $insert_sql "
hive -e "set mapred.job.name=justtest; use eqxdb; select phone,count(1) from base_user group by phone limit 10;"
```
### 使用sqoop创建hive表
```
/bin/sqoop import -D mapred.job.name=sqoop_import_ods_yqc_earn_record  --connect jdbc:oracle:thin:@server1:1521:test --username 'root' --password '123'  --table EARN_RECORD --hive-import -m 1  --hive-drop-import-delims --hive-table ods.earn_record --hive-overwrite --where ' rownum < 1 '  --columns 'ID,AD_TASK_ID,USER_ID,EARN_XD,CREATE_TIME,PV,UV,SCENE_PV,SCENE_UV,CLICKS,SCENE_ID,AD_POSITION_ID,PUBLISHER_ID'  --map-column-hive PV=int,UV=int,SCENE_PV=int,SCENE_UV=int,CLICKS=int,ID=string,AD_TASK_ID=string,EARN_XD=string,SCENE_ID=string,AD_POSITION_ID=string,PUBLISHER_ID=string
```