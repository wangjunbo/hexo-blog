title: Hive Metastore服务异常
author: peace
tags:
  - 大数据
  - ''
categories:
  - 编程
date: 2019-03-11 11:28:00
---
```
The health test result for HIVEMETASTORE_PAUSE_DURATION has become bad: Average time spent paused was 37.7 second(s) (62.80%) per minute over the previous 5 minute(s). Critical threshold: 60.00%.
```

```
The health test result for HIVE_HIVEMETASTORES_HEALTHY has become bad: Healthy Hive Metastore Server: 0. Concerning Hive Metastore Server: 0. Total Hive Metastore Server: 1. Percent healthy: 0.00%. Percent healthy or concerning: 0.00%. Critical threshold: 51.00%.
```
最近每天早晚都会收到这样的hive报警，今天google了一下，看cloudera上有人回答，应该是hive metastore的内存不足引起的，看了一下CDH manager上 Hive Metastore Server的Charts Library下的JVM Heap Memory Usage，发现使用率基本都是98%以上。嗯，应该是分配给hive metastore的内存太小了，从574M调到了2G。这两天观察一下。

凡是CDH PAUSE_DURATION相关的报警，应该都是java heap不够引起的。因为这个时候jvm在做GC，无法做其他响应。

参考 https://webcache.googleusercontent.com/search?q=cache:C3NJR-GjfPkJ:https://community.cloudera.com/t5/Batch-SQL-Apache-Hive/hive-server-2-pause-duration/m-p/57000+&cd=1&hl=zh-CN&ct=clnk&gl=hk 需要翻墙才能看到，并且这是google的缓存

以下是上边参考链接的文字内容：
```
hive server 2 pause duration

sim6
Expert Contributor
7/5/17
Pause DurationSuppress...
Average time spent paused was 46.4 second(s) (77.32%) per minute over the previous 5 minute(s). Critical threshold: 60.00%
 
I have suddenly started getting this issue. It never used to happen before. Have not even changed any configurations. What could be the possible reasons and how to inspect and resolve this?
Labels:
Cloudera Manager
Hive
Add Comment
Kudo 0
 
mbigelow
Champion
7/5/17
This is a garbage collection (GC) pause. A GC trigger will depend on the type of GC in use for HS2. An obvious trigger that you can detect is that the heap was too full. Go the HiveServer2 role an click Chart Library. It typically defaults to the Process Resources charts, if not go there. You should see the JVM Heap Memory Usage and GC pause charts. If the heap is constantly high (70% or above) then that is the likely reason. In that case the solution could be a simple as increasing the HS2 heap.

Note: the heap will increase as usage increases. So you could just have more concurrent or larger queries being processed by HS2.
Add Comment
Kudo 1
 
sim6
Expert Contributor
7/6/17
I have increased the heap size. It was set to default of 256 MB which I guess was causing the problem. I will revert if it keeps working alright :) Thanks much for your response .It helped
```