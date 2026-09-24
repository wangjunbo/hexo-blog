title: Elasticsearch scroll_id一直不变的原因
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-07-06 15:19:00
---
[为什么scroll_id不变](https://discuss.elastic.co/t/scroll-id-is-not-changing-while-querying/106202/2)
Short answer: yes, if you have a single shard index (as seems to be in your case) - it is expected behavior, but it can happen even if you have multiple shards. Longer answer: the scroll basically contains a list of shards where your search is running plus information about how to find your scroll data on each shard. As you exhaust results from each shard, you will notice that the scroll id becomes shorter, because we no longer need to search these shards and therefore don't need to list them on scroll. But if only have one shard or all shards will get processed at the same time, your scroll id might never change. Saying this, I wouldn't rely on this behavior since it might change in the future and always copy scroll id from the previous response.