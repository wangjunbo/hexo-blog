title: last block does not have enough number of replicas
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-04-17 16:16:00
---
【问题解决办法】

可以通过调整参数  dfs.client.block.write.locateFollowingBlock.retries的值来增加retry的次数，可以将值设置为6，那么中间睡眠等待的时间为400ms、800ms、1600ms、3200ms、6400ms、12800ms，也就是说close函数最多要50.8秒才能返回。


但是该dfs.client.block.write.locateFollowingBlock.retries 在开源配置中不开放，调整参数也只能规避问题，若CPU负荷很大的情况，依然会存在该问题。

建议降低任务并发量或者控制cpu使用率来减轻网络的传输，使得DN能顺利向NN汇报block情况。 

问题结论：

减轻系统负载。集群发生的时候负载很重，CPU的32个核（100%）全部分配跑MR认为了，至少要留20%的CPU

[问题原因分析过程](http://forum.huawei.com/enterprise/zh/thread-150819-1-1.html)