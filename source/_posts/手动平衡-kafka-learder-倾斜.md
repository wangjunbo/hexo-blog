title: 手动平衡 kafka learder 倾斜
author: peace
tags:
  - Kafka
categories:
  - 编程
date: 2020-02-06 16:02:00
---
### 1 配置需要reblance的topic列表
vim topics.conf
```
{"topics": [{"topic": "test"}],
 "version":1
}
```
### 2 进行reblance操作
注意休怪zkServer,和brokerIdList的值
vim reblance.sh

<!-- more -->

```
#!/bin/bash
zkServer="hadoop002:2181"
brokerIdList=144,145,146,1031,1032,1033
echo "zkConf:"  $zkServer
echo "brokerList:" $brokerIdList
echo "---------start generate reblance conf---------"
content=`kafka-reassign-partitions --zookeeper $zkServer --topics-to-move-json-file topics.conf  --broker-list $brokerIdList --generate`
content=`echo $content | awk -F 'Proposed partition reassignment configuration' '{print $2}'`
echo $content
echo $content > ressgin_topic.conf

if [ ! -d "log" ]; then
  mkdir log
fi

echo $content >> ./log/reblance.log
echo "" >> ./log/reblance.log
echo "---------end generate reblance conf---------"
#start reblance
echo "---------start reblance---------"
kafka-reassign-partitions --zookeeper $zkServer --reassignment-json-file ressgin_topic.conf --execute
```
### 3 查看reblance的进度
```
kafka-reassign-partitions --zookeeper $zkServer --reassignment-json-file ressgin_topic.conf --verify
```

from https://blog.csdn.net/qq_18838991/article/details/53394013