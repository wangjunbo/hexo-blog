title: kafka topic partition出现找不到leader的情况
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2020-02-01 11:34:00
---

![upload successful](/images/pasted-21.png)
一般可能是broker挂掉了，通过kafka manager 查看是哪个broker挂掉了，或出问题了。

然后将对应的broker重新启动。

一般建议将topic的副本数设置为2或3.