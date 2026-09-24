title: The field file exceeds its maximum permitted size of 1048576 bytes
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-04-09 16:01:00
---
SpringBoot做文件上传时出现了The field file exceeds its maximum permitted size of 1048576 bytes.错误，显示文件的大小超出了允许的范围。查看了官方文档，原来Spring Boot工程嵌入的tomcat限制了请求的文件大小，这一点在Spring Boot的官方文档中有说明，原文如下

65.5 Handling Multipart File Uploads
Spring Boot embraces the Servlet 3 javax.servlet.http.Part API to support uploading files. By default Spring Boot configures Spring MVC with a maximum file of 1Mb per file and a maximum of 10Mb of file data in a single request. You may override these values, as well as the location to which intermediate data is stored (e.g., to the /tmp directory) and the threshold past which data is flushed to disk by using the properties exposed in the MultipartProperties class. If you want to specify that files be unlimited, for example, set the multipart.maxFileSize property to -1.The multipart support is helpful when you want to receive multipart encoded file data as a @RequestParam-annotated parameter of type MultipartFile in a Spring MVC controller handler method.

文档说明表示，每个文件的配置最大为1Mb，单次请求的文件的总数不能大于10Mb。要更改这个默认值需要在配置文件（如application.properties）中加入两个配置

需要设置以下两个参数
```
multipart.maxFileSize
multipart.maxRequestSize
```
Spring Boot 1.3.x或者之前
```
multipart.maxFileSize=100Mb
multipart.maxRequestSize=1000Mb
```

Spring Boot 1.4.x或者之后
```
spring.http.multipart.maxFileSize=100Mb
spring.http.multipart.maxRequestSize=1000Mb
```

很多人设置了multipart.maxFileSize但是不起作用，是因为1.4版本以上的配置改了，详见官方文档：spring boot 1.4