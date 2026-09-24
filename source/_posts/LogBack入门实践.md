title: LogBack入门实践
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-10-22 22:52:00
---
一、简介
----

`LogBack`是一个日志框架，它是Log4j作者Ceki的又一个日志组件。

**LogBack,Slf4j,Log4j之间的关系**

`slf4j`是The Simple Logging Facade for Java的简称，是一个简单日志门面抽象框架，它本身只提供了日志Facade API和一个简单的日志类实现，一般常配合`Log4j`，`LogBack`，`java.util.logging`使用。Slf4j作为应用层的Log接入时，程序可以根据实际应用场景动态调整底层的日志实现框架(Log4j/LogBack/JdkLog...)；

`LogBack`和Log4j都是开源日记工具库，LogBack是Log4j的改良版本，比Log4j拥有更多的特性，同时也带来很大性能提升。

`LogBack`官方建议配合Slf4j使用，这样可以灵活地替换底层日志框架。
<!-- more -->
**LogBack的结构**  
LogBack分为3个组件，logback-core, logback-classic 和 logback-access。  
其中logback-core提供了LogBack的核心功能，是另外两个组件的基础。  
logback-classic则实现了Slf4j的API，所以当想配合Slf4j使用时，则需要引入这个包。  
logback-access是为了集成Servlet环境而准备的，可提供HTTP-access的日志接口。

**Log的行为级别：**

**OFF**、  
**FATAL**、  
**ERROR**、  
**WARN**、  
**INFO**、  
**DEBUG**、  
**ALL**  
从下向上，当选择了其中一个级别，则该级别向下的行为是不会被打印出来。  
举个例子，当选择了INFO级别，则INFO以下的行为则不会被打印出来。

二、slf4j与logback结合使用原理
---------------------

我们从java代码最简单的获取logger开始
```
    Logger logger = LoggerFactory.getLogger(xxx.class.getName());
```
LoggerFactory是slf4j的日志工厂，获取logger方法就来自这里。
```
    public static Logger getLogger(String name) {
        ILoggerFactory iLoggerFactory = getILoggerFactory();
        return iLoggerFactory.getLogger(name);
    }
```
这个方法里面有分为两个过程。第一个过程是获取ILoggerFactory，就是真正的日志工厂。第二个过程就是从真正的日志工厂中获取logger。  
第一个过程又分为三个部分。

**第一个部分加载org/slf4j/impl/StaticLoggerBinder.class文件**
```
    paths = ClassLoader.getSystemResources(STATIC_LOGGER_BINDER_PATH);//STATIC_LOGGER_BINDER_PATH = "org/slf4j/impl/StaticLoggerBinder.class"
```
**第二部分随机选取一个StaticLoggerBinder.class来创建一个单例**  
当项目中存在多个StaticLoggerBinder.class文件时，运行项目会出现以下日志：
```
    SLF4J: Class path contains multiple SLF4J bindings.
    SLF4J: Found binding in [jar:file:/C:/Users/jiangmitiao/.m2/repository/ch/qos/logback/logback-classic/1.1.3/logback-classic-1.1.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: Found binding in [jar:file:/C:/Users/jiangmitiao/.m2/repository/org/slf4j/slf4j-log4j12/1.7.12/slf4j-log4j12-1.7.12.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
    SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
```
最后会随机选择一个StaticLoggerBinder.class来创建一个单例
```
    StaticLoggerBinder.getSingleton()
```
**第三部分返回一个ILoggerFactory实例**

```
StaticLoggerBinder.getSingleton().getLoggerFactory();
```

所以slf4j与其他实际的日志框架的集成jar包中，都会含有这样的一个`org/slf4j/impl/StaticLoggerBinder.class`类文件，并且提供一个ILoggerFactory的实现。

第二个过程就是每一个和slf4j集成的日志框架中实现ILoggerFactory方法getLogger()的实例所做的事了。

三、slf4j与logback结合使用实践
---------------------

**第一步引入jar包**  
slf4j-api  
logback-core  
logback-classic（含有对slf4j的集成包）
```
    <!-- slf4j-api -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.12</version>
    </dependency>
    <!-- logback -->
    <dependency> 
        <groupId>ch.qos.logback</groupId> 
        <artifactId>logback-core</artifactId> 
        <version>1.1.3</version> 
    </dependency> 
    <dependency> 
        <groupId>ch.qos.logback</groupId> 
        <artifactId>logback-classic</artifactId> 
        <version>1.1.3</version> 
    </dependency>
```
**第二步编写简单的logback配置文件**
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>[%level][%d{yyyy-MM-dd} %d{HH:mm:ss:SSS}][%thread][%logger.%M:%L] - %msg%n</pattern>
        </encoder>
    </appender>
    <!--设置不打印某个类的日志-->
    <logger name="com.alibaba.dubbo.monitor.dubbo.DubboMonitor" level="OFF" />
    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```
文件位置位于`src/main/resources`下，名字默认为`logback.xml`。  
当然，logback也支持groovy格式的配置文件，如果你会用那更好。  
接下来，自己随便写一个类调用一下logger
```
    package log.test;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    /**
     * @author jiangmitiao
     * @date 2016/3/24
     * @description TODO
     */
    public class Foo {
        public static void doIt(){
            Logger logger = LoggerFactory.getLogger(Foo.class.getName());
            logger.debug("let`s do it");
        }
    }
    
    package log.test;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    /**
     * @author jiangmitiao
     * @date 2016/3/24
     * @description TODO
     */
    public class MyApp1 {
        public static void main(String[] args) {
            Logger logger = LoggerFactory.getLogger(MyApp1.class.getName());
            logger.info("before");
            Foo.doIt();
            logger.info("after");
    
            try {
                int i = 10 / 0;
            } catch (Exception e) {
                logger.error("errorTest",e);
            }
        }
    }

最后的结果是：

    16:22:13.459 [main] INFO  log.test.MyApp1 - before
    16:22:13.463 [main] DEBUG log.test.Foo - let`s do it
    16:22:13.463 [main] INFO  log.test.MyApp1 - after
    16:22:13.466 [main] ERROR log.test.MyApp1 - errorTest
    java.lang.ArithmeticException: / by zero
        at log.test.MyApp1.main(MyApp1.java:19) ~[classes/:na]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.6.0_25]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39) ~[na:1.6.0_25]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25) ~[na:1.6.0_25]
        at java.lang.reflect.Method.invoke(Method.java:597) ~[na:1.6.0_25]
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:140) [idea_rt.jar:na]
```
这么简单的配置当然是没有用的，下面这个就能够说明logback配置文件的编写规则了。
```
    <!-- scan 是否定期扫描xml文件， scanPeriod是说扫描周期是30秒-->
    <configuration scan="true" scanPeriod="30 seconds" debug="false" packagingData="true">
        <!-- 项目名称 -->
        <contextName>myApp1 contextName</contextName>
        <!-- 属性 -->
        <property name="USER_HOME" value="./log"/>
    
        <!-- Insert the current time formatted as "yyyyMMdd'T'HHmmss" under
           the key "bySecond" into the logger context. This value will be
           available to all subsequent configuration elements. -->
        <timestamp key="bySecond" datePattern="yyyyMMdd" timeReference="contextBirth"/>
        
        <!-- appender很重要，一个配置文件会有多个appender -->
        <!-- ConsoleApperder意思是从console中打印出来 -->
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <!-- 过滤器，一个appender可以有多个 -->
            <!-- 阈值过滤，就是log行为级别过滤，debug及debug以上的信息会被打印出来 -->
            <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
                <level>debug</level>
            </filter>
    
            <!-- encoders are assigned the type
                 ch.qos.logback.classic.encoder.PatternLayoutEncoder by default -->
            <!-- encoder编码规则 -->
            <encoder>
                <!--<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>-->
                <!--<pattern>%d %contextName %msg%n</pattern>-->
                <!-- pattern模式 %d时间 %thread 线程名 %level行为级别 %logger logger名称 %method 方法名称 %message 调用方法的入参消息 -->
                <pattern>%-4d [%thread] %highlight%-5level %cyan%logger.%-10method - %message%n</pattern>
            </encoder>
        </appender>
        
        <!-- FileAppender 输出到文件 -->
        <appender name="FILE" class="ch.qos.logback.core.FileAppender">
            <!-- 文件存放位置 %{xxx} 就是之前定义的属性xxx -->
            <file>${USER_HOME}/myApp1log-${bySecond}.log</file>
            
            <encoder>
                <!-- %date和%d是一个意思 %file是所在文件 %line是所在行 -->
                <pattern>%date %level [%thread] %logger{30} [%file:%line] %msg%n</pattern>
            </encoder>
        </appender>
        
        <!-- 输出到HTML格式的文件 -->
        <appender name="HTMLFILE" class="ch.qos.logback.core.FileAppender">
            <!-- 过滤器，这个过滤器是行为过滤器，直接过滤掉了除debug外所有的行为信息 -->
            <filter class="ch.qos.logback.classic.filter.LevelFilter">
                <level>debug</level>
                <onMatch>ACCEPT</onMatch>
                <onMismatch>DENY</onMismatch>
            </filter>
    
            <encoder class="ch.qos.logback.core.encoder.LayoutWrappingEncoder">
                <!-- HTML输出格式 可以和上边差不多 -->
                <layout class="ch.qos.logback.classic.html.HTMLLayout">
                    <pattern>%relative%thread%mdc%level%logger%msg</pattern>
                </layout>
            </encoder>
            <file>${USER_HOME}/test.html</file>
        </appender>
    
        <!-- 滚动日志文件，这个比较常用 -->
        <appender name="ROLLINGFILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 当project等于true的时候file就不会起效果-->
            <prudent>true</prudent>
            <!--<file>${USER_HOME}/logFile.log</file>-->
            <!-- 按天新建log日志 -->
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <!-- daily rollover -->
                <fileNamePattern>${USER_HOME}/logFile.%d{yyyy-MM-dd}_%i.log</fileNamePattern>
                <!-- 保留30天的历史日志 -->
                <maxHistory>30</maxHistory>
                
                <!-- 基于大小和时间，这个可以有，可以没有 -->
                <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                    <!-- or whenever the file size reaches 100MB -->
                    <!-- 当一个日志大小大于10KB，则换一个新的日志。日志名的%i从0开始，自动递增 -->
                    <maxFileSize>10KB</maxFileSize>
                </timeBasedFileNamingAndTriggeringPolicy>
            </rollingPolicy>
    
            <encoder>
                <!-- %ex就是指抛出的异常，full是显示全部，如果在{}中写入数字，则表示展示多少行 -->
                <pattern>%-4date [%thread] %-5level %logger{35} - %msg%n%ex{full, DISPLAY_EX_EVAL}</pattern>
            </encoder>
        </appender>
    
        <!-- 重点来了，上边都是appender输出源。这里开始就是looger了 -->
        <!-- name意思是这个logger管的哪一片，像下面这个管的就是log/test包下的所有文件 level是只展示什么行为信息级别以上的，类似阈值过滤器 additivity表示是否再抛出事件，就是说如果有一个logger的name是log，如果这个属性是true，另一个logger就会在这个logger处理完后接着继续处理 -->
        <logger name="log.test" level="INFO" additivity="false">
            <!-- 连接输出源，也就是上边那几个输出源 ，你可以随便选几个appender-->
            <appender-ref ref="STDOUT"/>
            <appender-ref ref="ROLLINGFILE"/>
            <appender-ref ref="HTMLFILE"/>
        </logger>
        <!-- 这个logger详细到了类 -->
        <logger name="log.test.Foo" level="debug" additivity="false">
            <appender-ref ref="STDOUT"/>
            <appender-ref ref="ROLLINGFILE"/>
            <appender-ref ref="HTMLFILE"/>
        </logger>
    
        <!-- Strictly speaking, the level attribute is not necessary since -->
        <!-- the level of the root level is set to DEBUG by default.       -->
        <!-- 这就是上边logger没有管到的情况下 root默认接管所有logger -->
        <root level="debug">
            <appender-ref ref="STDOUT"/>
        </root>
    </configuration>
```

四、过滤器的一些疑问
----------

Logback的过滤器基于三值逻辑，允许把它们组装或成链，从而组成任意的复合过滤策略。过滤器很大程度上受到Linux的iptables启发。这里的所谓三值逻辑是说，过滤器的返回值只能是ACCEPT、DENY和NEUTRAL的其中一个。  
如果返回DENY，那么记录事件立即被抛弃，不再经过剩余过滤器；  
如果返回NEUTRAL，那么有序列表里的下一个过滤器会接着处理记录事件；  
如果返回ACCEPT，那么记录事件被立即处理，不再经过剩余过滤器。  
写一个简单的过滤器大家就明白了。
```
    package log.test;
    
    import ch.qos.logback.classic.spi.ILoggingEvent;
    import ch.qos.logback.core.filter.Filter;
    import ch.qos.logback.core.spi.FilterReply;
    
    public class SampleFilter extends Filter<ILoggingEvent> {
      @Override
      public FilterReply decide(ILoggingEvent event) {    
        if (event.getMessage().contains("let")) {
          return FilterReply.ACCEPT;
        } else {
          return FilterReply.DENY;
        }
      }
    }
```
可以选择任意几个输出源加入这个filter
```
    <filter class="log.test.SampleFilter" />
```
最后的结果是，加入该filter的输出源只能输出Foo.doIt()中的日志了。

五、总结
----

LogBack配置比较简单，官网手册也是比较容易看懂的。除上边几种输出源之外，logback还支持输出到远程套接字服务器、 MySQL、 PostreSQL、Oracle和其他数据库、 JMS和远程UNIX Syslog守护进程等等。  
第一次学习log方面的知识，如有错误，请不吝赐教。  
**相关资源：**  
[官方手册](http://logback.qos.ch/manual/index.html)  
[LogBack简易教程](http://www.cnblogs.com/mailingfeng/p/3499436.html)  
[实际的xml配置](http://yuri-liuyu.iteye.com/blog/954038)  
[Logback浅析](http://www.cnblogs.com/yongze103/archive/2012/05/05/2484753.html)  
[logback 配置详解（一）](http://blog.csdn.net/haidage/article/details/6794509)