title: java发送邮件
tags:
  - Java
categories: []
date: 2016-01-26 18:58:00
---
```java
package com.google.utils;

import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.Message.RecipientType;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

/**
<dependency>
	<groupId>javax.mail</groupId>
	<artifactId>mail</artifactId>
	<version>1.4.7</version>
</dependency>
*/
public class EmailUtil {
	
	private static Properties properties = null;
	
	public static void init(Properties properties)
	{
		EmailUtil.properties = properties;
	}
	
	public static void send(String subject, String message) {
		try {
			Authenticator authenticator = new Authenticator() {
			    @Override
			    protected PasswordAuthentication getPasswordAuthentication() {
			        String userName = properties.getProperty("mail.user");
			        String password = properties.getProperty("mail.password");
			        return new PasswordAuthentication(userName, password);
			    }
			};
			Session session = Session.getInstance(properties, authenticator);
			MimeMessage mime = new MimeMessage(session);
			InternetAddress form = new InternetAddress(properties.getProperty("mail.user"));
			mime.setFrom(form);
			InternetAddress to = new InternetAddress(properties.getProperty("main.to"));
			mime.setRecipient(RecipientType.TO, to);

			mime.setSubject(subject);
			mime.setContent(message, "text/html;charset=UTF-8");
			
			Transport.send(mime);
		} catch (AddressException e) {
			e.printStackTrace();
		} catch (MessagingException e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		Properties properties = new Properties();
		properties.put("mail.smtp.auth", "true");
		properties.put("mail.smtp.host", "smtp.exmail.qq.com");
		properties.put("mail.user", "user1@example.com");
		properties.put("mail.password", "password");
		properties.put("main.to","user2@qq.com");
		
		EmailUtil.init(properties);
		EmailUtil.send("hello", "这是来自大洋彼岸的声音!");
	}

}
```