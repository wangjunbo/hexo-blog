title: Yikes! KeeperErrorCode = Unimplemented for /kafka-manager/mutex Try again
author: peace
date: 2020-07-02 10:54:46
tags:
---
Yikes! KeeperErrorCode = Unimplemented for /kafka-manager/mutex Try again

CMAK添加集群的时候报错，Yikes! KeeperErrorCode = Unimplemented for /kafka-manager/mutex Try again

参考网址 https://github.com/yahoo/CMAK/issues/731

```
my docker image version
zookeeper:3.4.14
wurstmeister/kafka:2.12-2.4.1
kafkamanager/kafka-manager:3.0.0.4
this worked for me
➜ docker exec -it zookeeper bash
root@98747a9eac65:/zookeeper-3.4.14# ./bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 2] ls /kafka-manager
[configs, deleteClusters, clusters]
[zk: localhost:2181(CONNECTED) 3] create /kafka-manager/mutex ""
Created /kafka-manager/mutex
[zk: localhost:2181(CONNECTED) 5] create /kafka-manager/mutex/locks ""
Created /kafka-manager/mutex/locks
[zk: localhost:2181(CONNECTED) 6] create /kafka-manager/mutex/leases ""
Created /kafka-manager/mutex/leases
```