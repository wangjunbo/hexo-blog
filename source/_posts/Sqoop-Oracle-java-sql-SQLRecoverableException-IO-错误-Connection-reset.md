title: 'Sqoop Oracle java.sql.SQLRecoverableException: IO 错误: Connection reset'
author: peace
tags:
  - 大数据
categories:
  - 编程
date: 2018-09-28 10:25:00
---
最近使用Sqoop将Oracle中的数据导入到Hive的时候总是出现如下错误：
```
18/09/28 09:44:53 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.11.0
18/09/28 09:44:53 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
18/09/28 09:44:53 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
18/09/28 09:44:53 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
18/09/28 09:44:53 INFO oracle.OraOopManagerFactory: Data Connector for Oracle and Hadoop is disabled.
18/09/28 09:44:53 INFO manager.SqlManager: Using default fetchSize of 1000
18/09/28 09:44:53 INFO tool.CodeGenTool: Beginning code generation
18/09/28 09:46:20 ERROR manager.SqlManager: Error executing statement: java.sql.SQLRecoverableException: IO 错误: Connection reset
java.sql.SQLRecoverableException: IO 错误: Connection reset
	at oracle.jdbc.driver.T4CConnection.logon(T4CConnection.java:467)
	at oracle.jdbc.driver.PhysicalConnection.<init>(PhysicalConnection.java:546)
	at oracle.jdbc.driver.T4CConnection.<init>(T4CConnection.java:236)
	at org.apache.sqoop.Sqoop.main(Sqoop.java:252)
Caused by: java.net.SocketException: Connection reset
	at java.net.SocketOutputStream.socketWrite(SocketOutputStream.java:113)
	at java.net.SocketOutputStream.write(SocketOutputStream.java:153)
	... 25 more
18/09/28 09:46:20 ERROR tool.ImportTool: Import failed: java.io.IOException: No columns to generate for ClassWriter
	at org.apache.sqoop.orm.ClassWriter.generate(ClassWriter.java:1664)
...
```
今天终于解决了。
```
export HADOOP_OPTS=-Djava.security.egd=file:/dev/../dev/urandom

sqoop import -D mapred.child.java.opts="-Djava.security.egd=file:/dev/../dev/urandom"
```
先在命令行设置HADOOP_OPTS，然后再执行sqoop import就可以了，根据我的测试结果，最好加上<code>mapred.child.java.opts</code>参数,并且在第二次导入的时候一定要加上```--hive-overwrite```参数。  
注：有时候-m需要设置成1才可以。刚开始我将-m设置成2报错，我设置成1就可以了。

在脚本中export...和sqoop import -D mapred.child.java.opts都是不能少的。

This problem occurs primarily due to the lack of a fast random number generation device on the host where the map tasks execute.

参考 https://community.hortonworks.com/questions/57497/sqoop-from-oracle-connection-reset-error.html