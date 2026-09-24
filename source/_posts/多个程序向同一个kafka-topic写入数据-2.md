title: Could not calculate build plan
tags:
  - Java
categories: []
date: 2016-02-25 14:16:00
---
参考http://stackoverflow.com/questions/12533885/could-not-calculate-build-plan-plugin-org-apache-maven-pluginsmaven-resources  
Could not calculate build plan: Plugin org.apache.maven.plugins:maven-resources-plugin:2.5 or one of its dependencies could not be resolved

``
I had the exact same problem and since I read somewhere that the error was caused by a cached file, I fixed it by deleting all the files under de .m2 repository folder. The next time I built the project I had to download all the dependencies again but it was worth it - 0 errors!!
``
