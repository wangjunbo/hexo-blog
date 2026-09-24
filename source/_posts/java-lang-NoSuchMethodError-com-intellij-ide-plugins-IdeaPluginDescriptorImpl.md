title: >-
  java.lang.NoSuchMethodError:
  'com.intellij.ide.plugins.IdeaPluginDescriptorImpl
author: peace
tags:
  - 编程
categories:
  - IDEA
date: 2021-01-08 10:07:00
---
在Mac上安装最新的社区版IDEA的时候，出现以下报错
```
Internal error. Please refer to https://jb.gg/ide/critical-startup-errors
java.lang.NoSuchMethodError: 'com.intellij.ide.plugins.IdeaPluginDescriptorImpl[] com.intellij.ide.plugins.PluginManagerCore.loadDescriptors()'
    at com.a.a.b.b.ar.a(ar.java:121)
    at com.a.a.b.b.ar.a(ar.java:71)
    at com.intellij.idea.MainImpl.start(MainImpl.java:19)
    at com.intellij.idea.StartupUtil.startApp(StartupUtil.java:302)
    at com.intellij.idea.StartupUtil.prepareApp(StartupUtil.java:242)
    at com.intellij.ide.plugins.MainRunner.lambda$start$1(MainRunner.java:41)
    at java.base/java.lang.Thread.run(Thread.java:834)
-----
Your JRE: 11.0.9.1+11-b1145.63 x86_64 (JetBrains s.r.o.)
/Applications/IntelliJ IDEA CE.app/Contents/jbr/Contents/Home
```

试了好几种解决办法，下边的一种办法倒是可以

I hit this same issue with OSX Catalina 10.14.6 when I upgraded from PycharmCE2019.3 to 2020.1.  The fix was to remove the ~/Library/Application Support/JetBrains/PyCharmCE2020.1/plugins/marketplace directory and start Pycharm again.  Works fine now.

参考链接 https://intellij-support.jetbrains.com/hc/en-us/community/posts/360008088579-intellij-dose-not-start-and-shows-start-error-