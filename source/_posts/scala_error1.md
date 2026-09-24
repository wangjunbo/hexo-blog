title: scala error overloaded method value
author: peace
tags:
  - Java
categories:
  - 编程
date: 2019-06-25 15:20:30
---
先声明一个mutable.HashMap[String,Long]()个变量
var topicCountForIntervalMap = mutable.HashMap[String,Long]()
使用如下语句，进行加减操作
```
val t:Long =  topicCountForIntervalMap.getOrElse(key,0) - TopicPartitionOffsetInfo.last.topicCountForIntervalMap.getOrElse(key,0)
```

得到如下错误
```
[ERROR] /xxxxx-java/src/main/scala/com/xxx/bigdata/kafka/monitor/TopicPartitionOffsetInfo.scala:86: error: overloaded method value - with alternatives:
[ERROR]   (x: Long)Long <and>
[ERROR]   (x: Int)Long <and>
[ERROR]   (x: Char)Long <and>
[ERROR]   (x: Short)Long <and>
[ERROR]   (x: Byte)Long
[ERROR]  cannot be applied to (AnyVal)
[ERROR]                 val t:Long =  topicCountForIntervalMap.getOrElse(key,0L) - TopicPartitionOffsetInfo.last.topicCountForIntervalMap.getOrElse(key,0)
[ERROR]                                                                          ^
[ERROR] one error found
```
解决办法，在使用getOrElse的使用一定要保证数据类型一致，改成如下的语句
```
val t:Long =  topicCountForIntervalMap.getOrElse(key,0L) - TopicPartitionOffsetInfo.last.topicCountForIntervalMap.getOrElse(key,0L)
```
就可以了
