title: >-
  java.lang.NoClassDefFoundError: Could not initialize class
  com.dianping.cat.message.internal.DefaultMessageProducer
author: peace
tags:
  - CAT
categories:
  - 编程
date: 2020-09-27 05:57:00
---
在使用的时候程序中经常会报如下错误：

```
[WARN][2020-09-27 11:41:24:908][][][nioEventLoopGroup-3-6][io.netty.channel.DefaultChannelPipeline][1152]- An exceptionCaught() event was fired, and it reached at the tail of the pipeline. It usually means the last handler in the pipeline did not handle the exception.
java.lang.NoClassDefFoundError: Could not initialize class com.dianping.cat.message.internal.DefaultMessageProducer
    at com.dianping.cat.Cat.initializeInternal(Cat.java:311)
    at com.dianping.cat.Cat.checkAndInitialize(Cat.java:72)
    at com.dianping.cat.Cat.getProducer(Cat.java:211)
    at com.sample.logserver.handler.ActionHandler.channelRead0(ActionHandler.java:38)
    at com.sample.logserver.handler.ActionHandler.channelRead0(ActionHandler.java:32)
    at io.netty.channel.SimpleChannelInboundHandler.channelRead(SimpleChannelInboundHandler.java:99)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:379)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:365)
    at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357)
    at io.netty.handler.codec.MessageToMessageDecoder.channelRead(MessageToMessageDecoder.java:102)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:379)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:365)
    at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357)
    at io.netty.channel.CombinedChannelDuplexHandler$DelegatingChannelHandlerContext.fireChannelRead(CombinedChannelDuplexHandler.java:436)
    at io.netty.handler.codec.ByteToMessageDecoder.fireChannelRead(ByteToMessageDecoder.java:321)
    at io.netty.handler.codec.ByteToMessageDecoder.channelRead(ByteToMessageDecoder.java:295)
    at io.netty.channel.CombinedChannelDuplexHandler.channelRead(CombinedChannelDuplexHandler.java:251)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:379)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:365)
    at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:357)
    at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1410)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:379)
    at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:365)
    at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:919)
    at io.netty.channel.nio.AbstractNioByteChannel$NioByteUnsafe.read(AbstractNioByteChannel.java:163)
    at io.netty.channel.nio.NioEventLoop.processSelectedKey(NioEventLoop.java:714)
    at io.netty.channel.nio.NioEventLoop.processSelectedKeysOptimized(NioEventLoop.java:650)
    at io.netty.channel.nio.NioEventLoop.processSelectedKeys(NioEventLoop.java:576)
    at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:493)
    at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:989)
    at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
    at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
    at java.lang.Thread.run(Thread.java:745)
```
CAT日志文件/data/appdatas/cat/cat_client_20200927.log中的错误
<!-- more -->
```
[ERROR] [CatLogger] error when read url:http://org.cat/cat/s/launch?ip=&env=unknown&hostname=,exception is org.cat
java.net.UnknownHostException: org.cat
```
原因是在/etc/profile中配置了
```
export CAT_HOME=/data/appdatas/cat
```
CAT的jar版本是3.0.0，源代码中读取配置的代码如下

![https://github.com/dianping/cat/blob/v3.0.0/cat-client/src/main/java/com/dianping/cat/Cat.java](/images/pasted-37.png)


![https://github.com/dianping/cat/blob/82475a28f7839753bc9f09e1e9b8395b6e0008b2/cat-client/src/main/java/com/dianping/cat/Cat.java](/images/pasted-38.png)

可以看到是直接读取CAT_HOME配置，然后和client.xml拼接在一块，形成了client.xml文件的路径，但是我在/etc/profile中配置CAT_HOME值的时候，/data/appdatas/cat路径的最后却少了斜杠/, 导致最终client.xml的绝对路径被识别层了“/data/appdatas/catclient.xml”， 这样肯定配置肯定会读取失败，所以/data/appdatas/cat的末尾加上/之后，程序运行就正常了。
这个问题已经在新版的CAT代码中修复了，所以无论CAT_HOME的值是否加/，都不会影响程序的正常运行。