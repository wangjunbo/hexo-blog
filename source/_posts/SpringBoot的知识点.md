title: SpringBoot的知识点
author: peace
tags:
  - Java
categories:
  - 编程
date: 2018-04-11 16:29:00
---
1.一些配置相关的类以及Controller等需要放到@SpringBootApplication注释的启动类的同级或者下级目录中。
比如启动类
```
package com.maple;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MapleApplication {

    public static void main(String[] args) {
        SpringApplication.run(MapleApplication.class, args);
    }
}
```

配置类：
```
package com.maple;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

//加上注释@Component，可以直接在其他地方使用@Autowired来创建其实例对象  
@Component  
@ConfigurationProperties(prefix = "hive")
public class HiveConf {
    public static  String url;  
    public static  String user;  
    public static  String password;
    
	public String getUrl() {
		System.out.println("the url is " + url);
		return url;
	}
	public void setUrl(String url) {
		System.out.println("the url is " + url);
		this.url = url;
	}
	public String getUser() {
		return user;
	}
	public void setUser(String user) {
		this.user = user;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}  
}

```