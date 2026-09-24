title: Mysql数据实时同步到Elasticsearch
author: peace
date: 2019-03-22 17:20:24
tags:
---
### 1. Mysql安装
在某些情况下，我们只需要 MySQL 的客户端，而不需要完整的MySQL服务器。比如当你需要连接到远程的 MySQL 服务器的时候。

如果只需安装客户端的话，可以使用命令：

```
yum install mysql
```

如果需要安装Mysql服务，使用如下命令：

```
yum install mysql-server
```
将Mysql的日志格式改为`row`
日志格式可以再mysql运行时改变
```
mysql> SET GLOBAL binlog_format = 'ROW';
```
参考 https://dev.mysql.com/doc/refman/5.6/en/binary-log-setting.html
### 2. 安装go
安装Go（1.9+）并设置你的GOPATH
* 进入下载页面 https://golang.google.cn/dl/
* 下载相应版本的go安装包，解压
* export PATH=$PATH:/path/to/go/bin

### 3.[go-mysql-elasticsearch](https://github.com/siddontang/go-mysql-elasticsearch)

go-mysql-elasticsearch可以将MySQL数据自动实时同步到Elasticsearch。

它首先用于mysqldump获取表中已有的数据，然后根据binlog将表的所有操作同步到ES。

数据的插入，更新，删除和增加字段操作，都可以及时同步到ES，ES索引中的字段可以比mysql中多。

##### 安装
```
git clone https://github.com/siddontang/go-mysql-elasticsearch.git
```
将其下载到服务器，它会在控制台中打印一些消息，跳过它。:-)
```
cd /path/to/go-mysql-elasticsearch
make
```
##### 如何使用
* 在MySQL中创建表。
* 如果可能，创建关联的Elasticsearch索引，文档类型和映射，否则，Elasticsearch将自动创建这些索引。
* 配置库，请参阅示例[config river.toml](https://github.com/siddontang/go-mysql-elasticsearch/blob/master/etc/river.toml)。
* 在配置文件中设置MySQL源，请参阅下面的[source](https://github.com/siddontang/go-mysql-elasticsearch#source)。
* 在配置文件中自定义MySQL和Elasticsearch映射规则，请参阅下面的[规则](https://github.com/siddontang/go-mysql-elasticsearch#rule)。
* 启动 `./bin/go-mysql-elasticsearch -config=./etc/river.toml`

##### 提示
* MySQL支持的版本<8.0
* ES支持的版本<6.0（其实ES6.4.1也基本可以用）
* binlog format必须是`row`。
* 无法在运行时更改表结构。(经过验证，其实是可以的)
* 将同步的MySQL表应该有PK（主键），现在允许多列PK，例如，如果PK是（a，b），将使用“a：b”作为键。PK数据将在Elasticsearch中用作“id”。您还可以使用其他列作为ID的组成部分。
* 你应首先在Elasticsearch中创建关联的`mapping`，我不认为使用默认`mapping`是明智的决定，你必须知道如何更好更精确的搜索。
* `mysqldump` 必须与go-mysql-elasticsearch存在于同一节点中（所以至少要安装mysql客户端），否则，go-mysql-elasticsearch将仅尝试同步binlog。
* 不要在一个SQL中同时更改太多行。

##### Source
在go-mysql-elasticsearch中，您必须确定要在源配置中将哪些表同步到elasticsearch。

etc/river.toml 配置文件中的格式如下：

```
  [[source]]
  schema = "test"
  tables = ["t1", t2]

  [[source]]
  schema = "test_1"
  tables = ["t3", t4]
```

schema是数据库名称，tables包括需要同步的表。
如果要同步数据库中的所有表，可以使用星号（*）。
```
[[source]]
schema = "test"
tables = ["*"]
# When using an asterisk, it is not allowed to sync multiple tables
# tables = ["*", "table"]
```

##### Rule
默认情况下，go-mysql-elasticsearch将使用MySQL表名作为Elasticserach的索引和类型名称，使用MySQL表字段名作为Elasticserach的字段名。
例如，如果一个名为blog的表，默认索引和Elasticserach中的类型都被命名为blog，如果表字段名为title，则默认字段名也称为title。

注意：go-mysql-elasticsearch将使用ES索引和类型的小写名称。例如，如果您的表名为BLOG，则ES索引和类型都被命名为blog。

Rule 可以让您更改此名称映射。配置文件中的规则格式如下：
```
[[rule]]
schema = "test"
table = "t1"
index = "t"
type = "t"
parent = "parent_id"
id = ["id"]

    [rule.field]
    mysql = "title"
    elastic = "my_title"
```

在上面的示例中，我们将使用新索引并键入名为“t”而不是默认“t1”，并使用“my_title”而不是字段名称“title”。

##### Rule field types
为了在不同的elasticsearch类型上映射mysql列，您可以按如下方式定义字段类型：
```
[[rule]]
schema = "test"
table = "t1"
index = "t"
type = "t"

    [rule.field]
    // This will map column title to elastic search my_title
    title="my_title"

    // This will map column title to elastic search my_title and use array type
    title="my_title,list"

    // This will map column title to elastic search title and use array type
    title=",list"

    // If the created_time field type is "int", and you want to convert it to "date" type in es, you can do it as below
    created_time=",date"
```
修饰符“list”将mysql字符串字段，如”a，b，c“，转换成elastic数组类型“{”a“，”b“，”c“}'，如果您需要在这些字段上做filter查询，这将非常有用。

##### Wildcard table
go-mysql-elasticsearch只允许你确定要同步哪个表，但有时候，如果你把一个大表分成多个子表，比如1024，table_0000，table_0001，... table_1023，那么很难为每个表编写规则。

go-mysql-elasticserach支持使用通配符表，例如：
```
[[source]]
schema = "test"
tables = ["test_river_[0-9]{4}"]

[[rule]]
schema = "test"
table = "test_river_[0-9]{4}"
index = "river"
type = "river"
```
“test_river_ [0-9] {4}”是通配符表定义，表示“test_river_0000”到“test_river_9999”，同时规则中的表必须与它相同。

在上面的示例中，如果您有1024个子表，则所有表将同步到Elasticsearch，索引为“river”并键入“river”。

##### Filter fields
您可以使用`filter`同步指定的字段，例如：
```
[[rule]]
schema = "test"
table = "tfilter"
index = "test"
type = "tfilter"
# Only sync following columns
filter = ["id", "name"]
```
在上面的例子中，我们只会同步MySQL表tfiler的列id和name到Elasticsearch。

##### 忽略没有主键的表
当您在没有主键的情况下同步表时，您可以看到以下错误消息。
```
schema.table must have a PK for a column
```
您可以在配置中忽略这些表，如：
```
# Ignore table without a primary key
skip_no_pk_table = true
```

##### Elasticsearch Pipeline
您可以使用[Ingest Node Pipeline](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html)在索引之前预处理文档，例如JSON字符串解码，合并文件等。
```
[[rule]]
schema = "test"
table = "t1"
index = "t"
type = "_doc"

# pipeline id
pipeline = "my-pipeline-id"
```
节点：您应该手动[创建管道](https://www.elastic.co/guide/en/elasticsearch/reference/current/put-pipeline-api.html)并且Elasticsearch> = 5.0。

##### Why not other rivers?
虽然Elasticsearch还有其他一些MySQL同步工具，比如[elasticsearch-river-jdbc](https://github.com/jprante/elasticsearch-jdbc)，[elasticsearch-river-mysql](https://github.com/scharron/elasticsearch-river-mysql)，作者还是想用Go新写了，为什么？

* 自定义，我想决定要同步哪个表，关联的索引和类型名称，甚至Elasticsearch中的字段名称。
* 使用binlog进行增量更新，并且可以在服务再次启动时从上次同步位置恢复。
* 一个通用的同步框架，不仅适用于Elasticsearch，也适用于其他内容，如memcached，redis等......
* 通配符表支持，我们有很多子表，如table_0000 - table_1023，但是想要使用唯一的Elasticsearch索引和类型。

参考 
https://github.com/siddontang/go-mysql-elasticsearch
https://leonax.net/p/4345/install-mysql-client-on-linux/