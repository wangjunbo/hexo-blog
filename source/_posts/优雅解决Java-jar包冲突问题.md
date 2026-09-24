title: 优雅解决Java jar包冲突问题
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-11-02 19:43:00
---
### 前言
当我们开发的Spark Application变得越来越复杂，依赖的jar包越来越多时，难免会碰到jar包冲突的问题。

### 举个例子：

我们的业务代码用到了一个第三方库，好比：guava（虽然好用，但是版本间的兼容性差的一坨翔）
Spark本身也依赖了guava，但是和业务代码中依赖的guava版本不同
这种情况下，把我们的Spark Application提交到集群里执行，很有可能因为版本问题导致运行出错。

大家都知道，JVM的ClassLoader加载类的时候，同一个ClassLoader加载的类，如果出现重复，只有第一个会被加载，后面重复的类会被忽略掉。

就我们的例子来说，整个Spark Application会优先加载Spark jars目录下的guava包，那么我们的业务代码自然很有可能受到影响了。
<!-- more -->
虽然Spark提供了一个spark.driver.userClassPathFirst配置，用来解决这个问题，但这个实验性的参数用起来比较鸡肋。首先只能应用于cluster模式，另外，设定为ture的时候还有可能会影响Spark本身的依赖。总之，不能很好地解决jar包冲突的问题。

接下来，我们探讨一种更加优雅的解决方案。

### 对依赖包做shade处理
Java的一大优势，就是基于字节码，我们也可以动态修改字节码文件。我们可以将项目中依赖的jar包中的类名改掉。

还是以guava为例，guava包中的包名以com.google.common.*开头，我们将guava包及代码依赖中的包名全部改掉，如：my_guava.common.*，然后打包到一起，就可以解决包冲突的问题。这种处理的效果，看起来就像是我们不在依赖guava了，自然也就不会和Spark自带的guava包产生冲突了。

这种处理我们称之为：shade化。好在我们常用的包管理工具已经有了shade化的处理方案了。

### 基于sbt构建的项目
修改项目目录的project/plugins.sbt，添加assembly插件
```
addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.5")
```

然后修改build.sbt在项目配置中添加以下设置：
```
assemblyShadeRules in assembly := Seq(
  // 处理guava版本和spark自带guava包版本冲突问题
  ShadeRule.rename("com.google.common.**" -> "my_guava.common.@1").inAll
)
```
> sbt的assembly插件会将项目中所有的依赖都打包到一起，通常情况下我们的集群中已经有Spark的部署包了，不需要把Spark的包也打进来。
我们在添加依赖的时候通过provided将其排除掉即可，如下：

```
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % "2.3.1" % "provided"
)
```
最后执行sbt assembly打包就可以了。

### 基于maven构建的项目
maven项目可以通过maven-shade-plugin插件，将有冲突的jar包shade化。

关键代码如下：
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    ...

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.1.0</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <relocations>
                                <relocation>
                                    <pattern>com.google.common</pattern>
                                    <shadedPattern>my_guava.common</shadedPattern>
                                </relocation>
                            </relocations>
                            <filters>
                                <filter>
                                    <artifact>*:*</artifact>
                                    <excludes>
                                        <exclude>META-INF/maven/**</exclude>
                                    </excludes>
                                </filter>
                            </filters>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```
最后通过mvn package打包项目就可以了。

### 验证
为了确保，我们确实shade化成功了，可以通过JD-GUI工具将打好的jar包反编译，正常情况下应该看不到com.google.common开头的包，而是包含my_guava.common开头的的包。如下图所示：

![jar shade](/css/images/jar-shade.jpg)
验证没问题的话就可以安心地提交到集群运行了。

### 结语
通过shade化第三方jar包，避免jar包版本冲突问题是个通用的解决方案，不仅适用于Spark Application，其他Java项目依然适用。

最近关注了下HBase 2.0，发现HBase也引入了shade机制，这样大家使用HBase时，就不用担心项目的第三方包跟HBase冲突的问题了。

相比之下Spark没有shade化，出现冲突问题，只能用户侧自己解决了.
from https://zhuanlan.zhihu.com/p/44956574