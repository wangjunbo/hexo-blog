title: Redis报错：ERR Operation against a key holding the wrong kind of value 解决处理
author: peace
tags:
  - 数据库
categories:
  - 编程
date: 2018-06-30 10:45:00
---
首先应该明白报这个错误说明了你用的jedis方法与redis服务器中存储数据的类型存在冲突。

例如：数据库中有一个key是usrInfo的数据存储的是Hash类型的，但是你使用jedis执行数据

操作的时候却使用了非Hash的操作方法，比如Sorted Sets里的方法。此时就会报

ERR Operation against a key holding the wrong kind of value这个错误！

问题解决：

先执行一条如下命令，usrInfo为其中的一个key值。

redis 127.0.0.1:6379>type usrInfo

此时会显示出该key存储在现在redis服务器中的类型，例如：

redis 127.0.0.1:6379>hash

则表示key为usrInfo的数据是以hash类型存储在redis服务器里的，此时操作这个数据就必须使用hset、hget等操作方法。

如果是zset如下：

redis 127.0.0.1:6379>zset

则表示数据类型为Sorted Sets的。此时就需要使用zadd、zrange等操作方法，否则就会报ERR Operation against a key holding the wrong kind of value这个错误！