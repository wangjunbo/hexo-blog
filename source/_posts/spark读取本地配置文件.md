title: spark读取本地配置文件
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-07-24 17:21:00
---
spark读取本地配置文件
在1.6等1.x版本中读取本地配置文件需要先在submit脚本中添加如下配置
```
--files /data/apps/config.properties
```
然后在spark的main方法中就可以使用如下方式读取了
```
val filePath = "config.properties"
LogUtil.info(filePath)
val props = new Properties()
props.load(new FileInputStream(filePath))
```
具体的代码可以参考 
<!-- more -->
https://blog.csdn.net/u012307002/article/details/53308937

但是自从2.0以后，可以使用一种简单的方式读取本地配置文件。
不需要先配置--files参数。
```
val filePath = "/data/apps/public/config.properties"
LogUtil.info(filePath)
val props = new Properties()
props.load(new FileInputStream(filePath))
```

/data/apps/public/config.properties在启动脚本所在本地服务器磁盘上。

也可以通过在打成的jar文件的后边跟上参数比如
```
#!/bin/bash
. ~/.bash_profile
/data/work/spark-2.2/bin/spark-submit \
--name AppTempTransformation \
--class com.hohode.statistics.AppTempTransformation \
--master yarn \
--driver-memory 3g --executor-memory 5g --total-executor-cores 5 \
/var/lib/transformatio-dependencies.jar  /data/apps/public/config.properties
exit
```
然后程序中仍然通过如下方式进行读取：
```
val filePath = "/data/apps/public/config.properties"
LogUtil.info(filePath)
val props = new Properties()
props.load(new FileInputStream(filePath))
```


