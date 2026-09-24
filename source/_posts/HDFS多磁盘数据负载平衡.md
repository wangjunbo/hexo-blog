title: HDFS多磁盘数据负载平衡
author: peace
tags:
  - hadoop
categories:
  - 编程
date: 2020-08-21 02:21:00
---
配置datanode block存放目录的时候，机器多磁盘能分摊磁盘IO负载，以下配置

![dfs.datanode.fsdataset.volume.choosing.policy](/images/pasted-36.png)
<!-- more -->

```
<property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/data2/dfs/data,file:/data3/dfs/data.........</value>
</property>

<property>
        <name>dfs.datanode.fsdataset.volume.choosing.policy</name> <value>org.apache.hadoop.hdfs.server.datanode.fsdataset.AvailableSpaceVolumeChoosingPolicy</value>
        <description>
        datanode数据副本存放的磁盘选择策略,有2种方式一种是轮询方式（org.apache.hadoop.hdfs.server.datanode.fsdataset.RoundRobinVolumeChoosingPolicy，为默认方式），
        另一种为选择可用空间足够多的磁盘存储方式,这个为了防止各个节点上的各个磁盘的存储均匀采用这个方式。
        </description>
</property>
<property>
        <name>dfs.datanode.available-space-volume-choosing-policy.balanced-space-threshold</name>
        <value>10737418240</value>
        <description>
        当在上面datanode数据副本存放的磁盘选择可用空间足够多的磁盘存储方式开启时，此选项才生效。这个参数主要功能是：
        首先计算出两个值，算出一个节点上所有磁盘中具有最大可用空间，另外一个值是所有磁盘中最小可用空间，如果这
        两个值相差小于该配置项指定的阀值时，则就用轮询方式的磁盘选择策略选择磁盘存储数据副本,如果比这个阀值大的话则
        还是选择可用空间足够多的磁盘存储方式。此项默认值为10737418240即10G
        </description>
</property>
<property>
        <name>dfs.datanode.available-space-volume-choosing-policy.balanced-space-preference-fraction</name>
        <value>0.75f</value>
        <description>
        默认值是0.75f，一般使用默认值就行。具体解析：有多少比例的数据副本应该存储到剩余空间足够多的磁盘上。
        该配置项取值范围是0.0-1.0，一般取0.5-1.0，如果配置太小，会导致剩余空间足够的磁盘实际上没分配足够的数据副本,
        而剩余空间不足的磁盘取需要存储更多的数据副本，导致磁盘数据存储不均衡。
        </description>
 </property>
```

yarn-site.xml中yarn.nodemanager.local-dirs配置中间结果目录，yarn.nodemanager.log-dirs日志配置也要分摊io配置


from https://blog.csdn.net/qingzhenli/article/details/72783763