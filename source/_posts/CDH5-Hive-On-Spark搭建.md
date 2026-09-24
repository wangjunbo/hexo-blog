title: CDH5 Hive On Spark搭建
author: peace
tags:
  - Hive
  - Spark
categories:
  - 编程
date: 2019-01-23 16:15:00
---
Hive On Spark的搭建，参考此文 https://www.jianshu.com/p/f4f058d0b0a4

先使用 hadoop classpath 查询HADOOP_CLASSPATH的路径

在当前用户的.bashrc文件中添加:
export SPARK_DIST_CLASSPATH=$(hadoop classpath)

先将chrome浏览器的语言设置为英语
然后按照下边链接方式更改cdh的配置
https://stackoverflow.com/questions/29800239/how-to-set-up-dynamic-allocation-on-cloudera-5-in-yarn

http://xn--jlq582ax31c.xn--fiqs8s/post/242
按照上边的方式对链接中的参数进行修改

进入hive命令行
然后执行类似sql语句： select count(1) from eqxdb.mall_tag;
如果出现Query Hive on Spark job信息，并且计算结果正确的话，说明配置成功了
或者
hive  --hiveconf  hive.root.logger=DEBUG,console  -e  "select count(1) from eqxdb.mall_tag;"