title: ' kafka auto.offset.reset介绍'
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-06-30 10:40:00
---
http://shift-alt-ctrl.iteye.com/blog/1930791

kafka + zookeeper,当消息被消费时,会向zk提交当前groupId的consumer消费的offset信息,当consumer再次启动将会从此offset开始继续消费.

在consumter端配置文件中(或者是ConsumerConfig类参数)有个"autooffset.reset"(在kafka 0.8版本中为auto.offset.reset),有2个合法的值"largest"/"smallest",默认为"largest",此配置参数表示当此groupId下的消费者,在ZK中没有offset值时(比如新的groupId,或者是zk数据被清空),consumer应该从哪个offset开始消费.largest表示接受接收最大的offset(即最新消息),smallest表示最小offset,即从topic的开始位置消费所有消息.