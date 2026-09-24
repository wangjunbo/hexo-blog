title: 初见kafka stream
author: Peace
tags:
  - 大数据
categories:
  - 编程
date: 2019-03-17 23:26:44
---
kafka stream架构
https://docs.confluent.io/current/streams/architecture.html#parallelism-model

kafka stream应用入门教程
https://kafka.apache.org/21/documentation/streams/tutorial

参数num.stream.threads   
The number of threads to execute stream processing.	  
默认只使用一个线程。  

https://docs.confluent.io/current/streams/developer-guide/config-streams.html#streams-developer-guide-optional-configs

多实例部署
https://docs.confluent.io/current/streams/faq.html#streams-faq-scalability-maximum-parallelism

### 注意
使用官方的例子的时候,可能会出现时间戳的错误，这时候自定义一个类，如下：
```
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.streams.processor.TimestampExtractor;
public class DefaultEventTimeExtractor implements TimestampExtractor {
    @Override
    public long extract(ConsumerRecord<Object, Object> record, long previousTimestamp) {
        return System.currentTimeMillis();
    }
}
```
然后在主类中进行如下设置：
```
props.put(StreamsConfig.DEFAULT_TIMESTAMP_EXTRACTOR_CLASS_CONFIG, DefaultEventTimeExtractor.class.getName());
```