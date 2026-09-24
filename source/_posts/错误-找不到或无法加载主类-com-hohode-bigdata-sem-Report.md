title: '错误: 找不到或无法加载主类 com.hohode.bigdata.sem.Report'
author: peace
tags:
  - 编程
categories:
  - Java
date: 2020-12-10 02:22:00
---
每次看到类似的错误总会有一种莫名的恐慌，不是因为解决不了，而是刚开始的不知所措，真像是吃了苍蝇一样的难受。  
遇到过很多次，每次解决完，都没有留下笔记可能错误比较低级，也完全忽略了，真是很无语啊。  

今天又遇到这个错误，终于痛下决心记录下来，虽然错误很低级，解决很简单，但是不能保证又会神经错乱啊。  
<!-- more -->

找到以前的这个jar包里其他类的执行命令startup.sh

```
source /etc/profile
java -cp /data/apps/sem_keywords/hohode-bigdata-java-1-jar-with-dependencies.jar  com.hohode.bigdata.sem.BaiduKeyword /data/apps/public/conf.properties -1
```

将jar包随意上传到一个路径/data/tmp/test下，替换一下上边路径中的类名，于是执行如下命令
```
java -cp /data/apps/sem_keywords/hohode-bigdata-java-1-jar-with-dependencies.jar  com.hohode.bigdata.sem.Report 
```

然后报错
```
错误: 找不到或无法加载主类 com.hohode.bigdata.sem.Report
```

哪出错了呢？

检查一下打包程序，没有问题。  
将服务器上的jar解压，看了看发现有com/hohode/bigdata/sem/Report.class文件。 

额，很无语...  

结果是因为把jar包上传到了/data/tmp/test路径下，结果执行的时候却使用的/data/apps/sem_keywords/hohode-bigdata-java-1-jar-with-dependencies.jar，是不是很无语...  
......  

算了，接着写  

切换到/data/tmp/test目录，然后执行如下命令就可以了
```
java -cp hohode-bigdata-java-1-jar-with-dependencies.jar  com.hohode.bigdata.sem.Report 
```
注意，执行的时候把/data/apps/sem_keywords/路径去掉，或者写正确完整路径。



