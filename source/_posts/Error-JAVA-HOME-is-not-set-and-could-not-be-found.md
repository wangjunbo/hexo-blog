title: Java错误记录
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-04-26 09:27:00
---
### 一. Error: JAVA_HOME Is Not Set and Could Not Be Found

使用hdfs用户，通过crontab配置了定时任务，定时脚本调用了python脚本，Python脚本执行了Hadoop命令。原crontab如下
```
2 09 * * * cd /data/apps/py_hdfs2cos; ./start.sh >> log/crontab.log 2>&1
```
改为如下命令即可：
```
2 09 * * * cd /data/apps/py_hdfs2cos; source /etc/profile; ./start.sh >> log/crontab.log 2>&1
```
其中增加了source /etc/profile;