title: Sqoop学习笔记
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-05-31 17:10:00
---
将数据库中的数据导入hive中

列出数据库中所有的表
```
sqoop list-tables --connect 
jdbc:oracle:thin:@192.168.1.22:1521:oragbk --username 'dev' --password '123'
```
任务重命名,使用多个mapper
```
sqoop import -D mapred.job.name=sqoop_import_base_user$i --append --connect jdbc:oracle:thin:@2.1.8.10:1521:aaa --username 'bbb' --password 'r6' --target-dir /data/db/ -m 10 --fields-terminated-by '\001' --split-by es.id  --query "$q" -hive-drop-import-delims
```
如果不制定 -m 参数,导入的时候数据可能会被分成多个split，可能会导致导入后的数据少或者重复
```
18/06/28 11:37:35 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`id`), MAX(`id`) FROM `base_user`
18/06/28 11:37:35 WARN db.TextSplitter: Generating splits for a textual index column.
18/06/28 11:37:35 WARN db.TextSplitter: If your database sorts in a case-insensitive order, this may result in a partial import or duplicate records.
18/06/28 11:37:35 WARN db.TextSplitter: You are strongly encouraged to choose an integral split column.
18/06/28 11:37:35 INFO mapreduce.JobSubmitter: number of splits:6
```
自动创建表结构   
自定义任务名   
oracle数据的表名和列名都需要大写
```
sqoop import -D mapred.job.name=sqoop_import_EQS_ORDER_INVOICE --connect jdbc:oracle:thin:@hohode.cn:1521:hello --username 'root' --password '123' --table INVOICE --hive-import -m 1 --hive-table default.invoice2 --hive-overwrite --map-column-hive ID=string,INVOICE_TYPE=string
```
--map-column-java ID=String 完全不起作用，还是使用--map-column-hive ID=string   
## WARN
如果一个oracle或者mysql数据表的一个字段中含有\n换行符，这会导致导入hive中的数据比之前数据表中的数据要多，这个时候要使用<code>--hive-drop-import-delims</code>将换行符等去掉。  
### 导入hive终极语句
```
/bin/sqoop import -D mapred.job.name=sqoop_import_student --connect jdbc:oracle:thin:@hohode.com:1521:w --username 'root' --password '123456' --table student --hive-import -m 1  --hive-drop-import-delims --hive-table test.student --hive-overwrite --map-column-hive ID=string,INVOICE_TYPE=string
```

### mapper为20，并且指定了split-by参数
```
/bin/sqoop import -D mapred.job.name=sqoop_import_base_user --connect jdbc:oracle:thin:@2.3.8.10:1521:qx --username 'qx' --password 'e$q3s#hr6' --table BASE_USER --hive-import -m 20  --hive-drop-import-delims --hive-table qxdb.base_user --hive-overwrite --map-column-hive TYPE=string,STATUS=string,CHECK_EMAIL=string,CHECK_PHONE=string,SECURITY_LEVEL=string --split-by REG_TIME
```
### 指定map-column-hive的decimal类型
```
/bin/sqoop import -D mapred.job.name=sqoop_import_orders --connect jdbc:mysql://101.0.2.2:3306/print --username 'root' --password 'hello' --table orders --hive-import -m 1  --hive-drop-import-delims --hive-table db.orders --hive-overwrite  --map-column-hive 'total_fee=decimal(15%2C2)'
```

### [常见问题](https://github.com/zhilangtaosha/docs-1/blob/master/technology/hadoop-docs/sub-project/sqoop/sqoop-client.md)
1.出现 org.apache.sqoop.Sqoop 找不到主类

解决 : 把 sqoop 目录下的 sqoop-1.4.4.jar 拷贝到 hadoop 的 lib 目录下
cd /opt/cloudera/parcels/CDH/lib/hadoop
sudo ln -s ../../jars/sqoop-1.4.5-cdh5.3.3.jar ./

2.mysql 类加载不到

解决 : 下载 mysql JDBC 放到 hadoop 目录下即可
cd /opt/cloudera/parcels/CDH/lib/hadoop
sudo ln -s ../../jars/mysql-connector-java-5.1.31.jar ./

3.HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce 找不到
ERROR tool.ImportTool: Imported Failed: Parameter 'directory' is not a directory

解决 : sudo ln -s /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce /usr/lib/hadoop-mapreduce