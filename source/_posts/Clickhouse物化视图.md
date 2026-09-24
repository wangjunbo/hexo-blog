title: Clickhouse物化视图
author: peace
tags:
  - Clickhouse
categories:
  - 编程
date: 2019-10-16 10:00:00
---
Clickhouse创建物化视图，指定POPULATE
```
CREATE MATERIALIZED VIEW app.tracker_log (`day` Date, `st` UInt64, `u_i` String, `d_i` String, `tk_id` String, `time` DateTime, `cat` String, `act` String, `e_t` String, `c_p` String) ENGINE = MergeTree()  PARTITION BY day ORDER BY (st) POPULATE AS SELECT day, toUInt64(s_t) AS st, u_i, d_i, tk_id, toDateTime(toUInt64(st) / 1000) AS time, cat, act, e_t, multiIf((c_p = 'iOS') OR (c_p = 'IOS') OR (c_p = 'Android'), 'APP', (c_p = 'wap') OR (c_p = 'WAP'), 'WAP', 'PC') AS c_p FROM app.scene_tracker WHERE (s_t != '') AND ((u_i != '') OR (d_i != '')) AND (length(d_i) > 5) AND (length(cat) > 1) AND (length(act) > 1)
```