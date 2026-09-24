title: mysql批量导入文本
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2018-02-13 11:09:00
---
参考http://blog.csdn.net/u014082714/article/details/53173975   

先创建好表data。

```sql
 load data local infile "~/Desktop/data.txt" into table data fields terminated by '|';
```
terminated by后边的'|'，说明字段是用|进行分割的。
下边是要导入的部分数据：
```
id 		      | name
113972451813201  | xujiayin
113972451813202  | mahuateng
113972451813103  | jackMa
```
前几导入的时候没有成功，最后发现是id的长度8设置的太小，改为32就足够了，接下来导入一次成功。
