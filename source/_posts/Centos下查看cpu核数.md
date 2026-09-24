title: Centos下查看cpu核数
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-12-03 10:24:00
---
1.概念
物理CPU：实际Server中插槽上的CPU个数。
物理cpu数量：可以数不重复的 physical id 有几个。
<!-- more -->
2.逻辑CPU
Linux用户对 /proc/cpuinfo 这个文件肯定不陌生. 它是用来存储cpu硬件信息的，信息内容分别列出了processor 0 – n 的规格。这里需要注意，如果你认为n就是真实的cpu数的话, 就大错特错了。一般情况，我们认为一颗cpu可以有多核，加上intel的超线程技术(HT), 可以在逻辑上再分一倍数量的cpu core出来逻辑CPU数量=物理cpu数量 x cpu cores 这个规格值 x 2(如果支持并开启ht)
备注一下：Linux下top查看的CPU也是逻辑CPU个数

3.CPU核数
一块CPU上面能处理数据的芯片组的数量、比如现在的i5 760,是双核心四线程的CPU、而 i5 2250 是四核心四线程的CPU，一般来说，物理CPU个数×每颗核数就应该等于逻辑CPU的个数，如果不相等的话，则表示服务器的CPU支持超线程技术。

4.查看CPU信息
当我们 cat /proc/cpuinfo 时，具有相同core id的CPU是同一个core的超线程，具有相同physical id的CPU是同一个CPU封装的线程或核心。

下面举例说明
【1】查看CPU型号：cpu型号是E7-4820
```
[root@node1 ~]# cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
    32  Intel(R) Xeon(R) CPU E7- 4820  @ 2.00GHz
```
【2】查看物理cpu个数：物理核心数是2核
```
[root@node1 ~]# cat /proc/cpuinfo | grep "physical id" | sort | uniq|wc -l
2
```
【3】查看逻辑cpu的个数：逻辑cpu个数是32个
```
[root@node1 ~]# cat /proc/cpuinfo | grep "processor" |wc -l
32
```
【4】查看cpu是几核：cpu是8核
```
[root@node1 ~]# cat /proc/cpuinfo | grep "cores"|uniq
cpu cores       : 8
```
余木脑袋 https://www.jianshu.com/p/4cdd46f5e543
