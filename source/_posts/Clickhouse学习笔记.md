title: Clickhouse学习笔记
author: peace
date: 2019-10-17 19:45:17
tags:
---
设置行数据的过期时间为2分钟，每60秒合并一次数据。
```
CREATE TABLE test.ttlt ( d DateTime, a Int ) ENGINE = MergeTree PARTITION BY toYYYYMM(d) ORDER BY d TTL d + INTERVAL 2 minute SETTINGS merge_with_ttl_timeout= 60 
```

按照时长30分钟切割session
```
SELECT u_i,sequenceCount1('(?1).*(?2)(?t>1800000)')(time, 1=1 ,1=1)+1 as cc from tracker_log group by u_i;
```