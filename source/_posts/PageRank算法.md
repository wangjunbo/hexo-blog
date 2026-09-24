title: PageRank算法
tags:
  - Algorithm
categories: []
date: 2016-01-19 11:25:00
---
详细内容参考http://blog.csdn.net/Leonis_v/article/details/50531032
### PageRank算法原理
PageRank的计算充分利用了两个假设：数量假设和质量假设。步骤如下：
  1）在初始阶段：网页通过链接关系构建起Web图，每个页面设置相同的PageRank值，通过若干轮的计算，会得到每个页面所获得的最终PageRank值。随着每一轮的计算进行，网页当前的PageRank值会不断得到更新。

  2）在一轮中更新页面PageRank得分的计算方法：在一轮更新页面PageRank得分的计算中，每个页面将其当前的PageRank值平均分配到本页面包含的出链上，这样每个链接即获得了相应的权值。而每个页面将所有指向本页面的入链所传入的权值求和，即可得到新的PageRank得分。当每个页面都获得了更新后的PageRank值，就完成了一轮PageRank计算。