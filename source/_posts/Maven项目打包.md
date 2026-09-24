title: Maven项目打包
author: peace
tags:
  - Java
categories:
  - 编程
date: 2021-11-01 07:44:00
---
Maven项目打包的方式有多种，我常用的就是使用`assembly`的方式进行打包。

### 1 首先需要在pom项目中加入如下依赖
```
    ...
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.2</version>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
            <plugin>
                <!-- see http://davidb.github.com/scala-maven-plugin -->
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
                <version>3.1.3</version>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
            </plugin>
        </plugins>
    </build>
    
</project>
```
### 2 使用如下命令进行编译打包
```
mvn assembly:assembly
```