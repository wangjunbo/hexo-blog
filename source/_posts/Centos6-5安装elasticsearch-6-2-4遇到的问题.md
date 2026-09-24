title: Centos6.5安装elasticsearch-6.2.4遇到的问题
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-04-25 15:25:00
---
```
ERROR: [4] bootstrap checks failed
[1]: max file descriptors [65535] for Elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [1024] for user [xiao] is too low, increase to at least [4096]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[4]: system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
```
### 问题一
```
vim /etc/security/limits.conf
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096 
```
<!-- more -->

### 问题二
```
vi /etc/security/limits.d/90-nproc.conf 
soft nproc 4096
```

### 问题三  
切换到root用户修改配置sysctl.conf
```
vi /etc/sysctl.conf 
添加：
vm.max_map_count=655360

并执行命令：
sysctl -p
```

### 问题四
[原因:这是在因为Centos6不支持SecComp，而ES5.2.0默认bootstrap.system_call_filter为true进行检测，所以导致检测失败，失败后直接导致ES不能启动。](http://blog.51cto.com/gz521/1927758)
 
解决：在elasticsearch.yml中配置bootstrap.system_call_filter为false，注意要在Memory下面:
```
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
```