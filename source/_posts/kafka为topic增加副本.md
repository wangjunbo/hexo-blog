title: kafka为topic增加副本
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2020-02-06 15:53:00
---
### 1. 查看topic的原来的副本分布
[hadoop@hadoop006 ~]$ kafka-topics --zookeeper hadoop002:2181 --describe --topic tracker_view

### 2. 增加Topic的副本的json文件的编写
vim addReplicasToTracker_view.json
<!-- more -->
```
{
    "version": 1,
    "partitions": [
        {
            "topic": "tracker_view",
            "partition": 4,
            "replicas": [
                144,
                145
            ]
        },
        {
            "topic": "tracker_view",
            "partition": 5,
            "replicas": [
                145,
                146
            ]
        },
        {
            "topic": "tracker_view",
            "partition": 0,
            "replicas": [
                146,
                1031
            ]
        },{
            "topic": "tracker_view",
            "partition": 1,
            "replicas": [
                1031,
                1032
            ]
        },
        {
            "topic": "tracker_view",
            "partition": 2,
            "replicas": [
                1032,
                1033
            ]
        },
        {
            "topic": "tracker_view",
            "partition": 3,
            "replicas": [
                1033,
                144
            ]
        }
    ]
}
```

### 3. 执行topic增加副本操作
```
kafka-reassign-partitions --zookeeper hadoop002:2181 --reassignment-json-file addReplicasToTracker_view.json --execute
```

### 4. 查看执行的状态
```
kafka-reassign-partitions  --zookeeper hadoop002:2181 --reassignment-json-file addReplicasToTracker_view.json --verify
```




### 5. 其它
```
kafka-consumer-groups --bootstrap-server hadoop006:9092 --describe --group test

kafka-run-class kafka.tools.GetOffsetShell --broker-list hadoop006:9092 --topic test --time -1

kafka-topics --zookeeper hadoop002:2181  --list

kafka-topics  --delete --zookeeper hadoop002:2181   --topic raw_log

```

本文参考了其它文章