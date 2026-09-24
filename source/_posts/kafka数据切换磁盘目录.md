title: kafka数据切换磁盘目录
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2019-07-05 14:49:00
---
查看所有磁盘 
```
fdisk -l
```
查看现有磁盘信息 
```
df -hT
```

格式化磁盘

```
mkfs.xfs /dev/vdd
```
创建目录
mkdir /data2
挂载磁盘
```
mount /dev/vdd /data2
```
查看原来kafka数据文件目录的大小
```
du -sh /data/kafka/data
```

在新挂载的磁盘上创建目录
```
mkdir -p /data2/kafka/data
```

为了减少kafka数据的大小，可以先动态改变kafka数据的保留时间，默认为7天，先改为12个小时
```
kafka-topics --zookeeper node1:2181 -topic xxxx --alter --config retention.ms=86400000

```
等重启kafka之后，可以将此配置参数改过来

停掉这台机器上的kafka broker

将原来kafka数据拷贝到新的目录
```
cp -r /data/kafka/data/* /data2/kafka/data/
```
对新目录的kafka数据授权
```
chown -R kafka:kafka /data2/kafka/data
```

将原来目录中的kafka数据备份
```
mv /data/kafka/data /data/kafka/data.bak
```

将这台机器上的kafka的数据目录(log.dirs)改为/data2/kafka/data

启动这台机器上的kafka broker

过3分钟，观察yarn上的任务，大部分已经恢复正常工作。