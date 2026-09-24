title: HIVE  INSERT OVERWRITE和WITH AS语句同时使用
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-09-14 09:52:00
---
with as子句要放到insert语句的前边，并且with语句和insert之间不能又分号。
```
CREATE TABLE ramesh_test
(key          BIGINT,
 text_value   STRING,
 roman_value  STRING)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY '\t' 
LINES TERMINATED BY '\n' 
STORED AS TEXTFILE;

WITH v_text
AS
(SELECT 1 AS key, 'One' AS value),
v_roman
AS
(SELECT 1 AS key, 'I' AS value)
INSERT OVERWRITE TABLE ramesh_test
SELECT v_text.key, v_text.value, v_roman.value
  FROM v_text JOIN v_roman
                ON (v_text.key = v_roman.key);
```
参考 https://stackoverflow.com/questions/38246042/hive-insert-overwrite-using-with-clause