title: CDH集群扩容记录
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-12-05 11:37:00
---
系统为Centos7.5，向集群添加hadoop019服务器(下边图片中的服务器名为hadoop04...，大致流程是这样的，之后会完善的)。

### 准备工作
* 查看服务器系统版本
```
[root@VM_10_10_centos ~]# cat /etc/redhat-release
CentOS Linux release 7.5.1804 (Core)
```
* 首先修改各新服务器的主机名
```
hostnamectl set-hostname  hadoop019
localectl set-locale LANG=zh_CN.utf8
```
<!-- more -->
* 同步时间(用的腾讯云)
```
# crontab -e
*/20 * * * * /usr/sbin/ntpdate ntpupdate.tencentyun.com >/dev/null &
```

* 生成秘钥
```
ssh-keygen -t rsa
```

* 修改/etc/hosts
```
vim /etc/hosts
```
* 配置免密码登录
将Cloudera Manager Server的秘钥放到追加到新服务器/root/.ssh/authorized_keys

* 添加用户和组
```
useradd --system --home=/data/work/cm-5.7.2 --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
以下为可选操作
useradd sqoop2 -g sqoop
groupadd hdfs
groupadd hadoop
useradd hdfs -g hadoop -d /var/lib/hadoop-hdfs/ -c 'Hadoop HDFS'
```

* 修改部分配置  
以root 用户执行命令
```
echo "10" > /proc/sys/vm/swappiness
echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```

* 安装JDK8并配置好环境
```
root@hadoop001 work]# tar -cvf jdk8.tar /data/work/java1.8
root@hadoop001 work]# scp jdk8.tar root@hadoop019:/data/software/
[root@hadoop019 work]# vim /etc/profile
export JAVA_HOME=/data/work/java1.8
export JRE_HOME=/data/work/java1.8/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export LD_LIBRARY_PATH=/usr/lib/oracle/11.2/client64/lib/:$LD_LIBRARY_PATH
export PATH=$PATH:$JAVA_HOME/bin:$MVN_HOME/bin:$JRE_HOME/bin
[root@hadoop019 work]# source /etc/profile
[root@hadoop019 work]# java -version
```


### 挂载磁盘
查看所有磁盘 fdisk -l
查看现有磁盘信息 df -hT
格式化磁盘 mkfs.xfs /dev/vdb

卸载已经启动的cloudera-scm-agent(不是必要操作)
umount /data/work/cm-5.7.2/run/cloudera-scm-agent/process

挂载磁盘
	mkdir /data
    mount /dev/vdb /data
   
### 下载Cloudera Manager
下载相应的cloudera-manager-centos7-cm*.tar.gz到/opt/cloudera-manager/
下载地址：http://archive.cloudera.com/cm5/cm/5/


### 解压并修改配置文件
```
tar -zxvf cloudera-manager-centos7-cm5.11.0_x86_64.tar.gz
cd 
cd /opt/cloudera-manager/cm-5.11.0/etc/cloudera-scm-agent/
vim config.ini
修改server_host为Cloudera Manager Server的地址
```

### 启动Agent
```
/opt/cloudera-manager/cm-5.11.0/etc/init.d/cloudera-scm-agent start
```

### 在管理页面进行配置
* 登录http://hadoop1:7180
* 点击“主机”->“所有主机” ，可以看到所有节点，包括刚才添加的节点。这个时候新添加节点的状态是红的，正常情况下，过2分钟，节点的状态变为绿色正常。
![](/css/images/cdh1.png)
* 点击右上角“向集群添加主机”，然后点击 继续
* 输入hadoop05,点击 搜索 ,出现如下界面
![](/css/images/cdh2.png)
* 点击 当前管理的主机(1)
* 按照提示进行下边的操作

### 为新节点分配角色
例如：回到首页面，点击 HDFS ，点击 实例 ，选中新添加节点，点击 添加角色实例，然后按照提示依次进行。