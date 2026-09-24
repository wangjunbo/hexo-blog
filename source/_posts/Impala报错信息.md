title: Impala报错信息
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-08-15 09:49:00
---
### RPC client failed to connect: Couldn't open transport for hadoop007:22000 (connect() failed: 拒绝连接)

可能是元数据信息的问题，在hue Impala上刷新依稀元数据信息就可以了。

### AnalysisException: Failed to load metadata for table

https://stackoverflow.com/questions/16444340/cloudera-impala-queries-failing

Seems like a restart of the Impala service in Cloudera Manager fixed the issue.