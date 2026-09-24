title: 为什么每次进入命令都要重新source /etc/profile 才能生效？
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2019-01-23 16:10:00
---
```
#编辑JDK8
export JAVA_HOME="/usr/java/java8"
#编辑maven
export M2_HOME="/opt/idea-IU-162.1121.32/plugins/maven/lib/maven3"
#编辑PATH
export PATH="$JAVA_HOME/bin:$M2_HOME/bin:$PATH"
```
这是我的/etc/profile末尾的配置，JDK是没有问题的，不用source，echo $JAVA_HOME能出来，问题是如果要用mvn，每次就要source一遍才行，maven我用的是IDEA自带的。

回答 1、也可以放在~/.bashrc里面。或者在~/.bashrc里面加一句
```
source /etc/profile
```
（采用此方法，已成功生效）
回答 2、你可以把这几条命令写在 /etc/bash里面   就会自动执行了

from https://blog.csdn.net/lwplvx/article/details/79192182