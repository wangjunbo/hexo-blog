title: Clickhouse删除基于mysql引擎的数据库
author: peace
tags:
  - Clickhouse
categories:
  - 编程
date: 2020-01-08 17:39:00
---
不能使用普通的drop database xxx命令，而要使用如下命令：
```
DETACH DATABASE your_database_name
```

之后删除磁盘上基于mysql引擎的数据库的元信息：

```
~  rm -rf metadata/your_database_name
```


参考  https://github.com/ClickHouse/ClickHouse/issues/6063