title: parse_url函数 (Spark SQL)
author: peace
tags:
  - Spark
categories:
  - 编程
date: 2021-11-30 03:18:00
---
from https://docs.microsoft.com/zh-cn/azure/databricks/sql/language-manual/functions/parse_url

从 url 中提取一部分。

### 语法

```
parse_url(url, partToExtract [, key] )
```
### 参数
- url：一个 STRING 表达式。
- partToExpract：一个 STRING 表达式。
- key：一个 STRING 表达式。
### 返回
字符串。

partToExtract 必须是以下各项之一：

- 'HOST'
- 'PATH'
- 'QUERY'
- 'REF'
- 'PROTOCOL'
- 'FILE'
- 'AUTHORITY'
- 'USERINFO'
key 区分大小写。

如果未找到请求的 partToExtract 或 key，则返回 NULL。

### 示例
SQL

```
 SELECT parse_url('http://spark.apache.org/path?query=1', 'HOST');
 spark.apache.org
 SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY');
 query=1
 SELECT parse_url('http://spark.apache.org/path?query=1', 'QUERY', 'query');
 1
```