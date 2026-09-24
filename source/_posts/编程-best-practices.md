title: 编程 best practices
author: peace
tags:
  - Elasticsearch
categories:
  - 编程
date: 2018-02-25 12:04:00
---
##### 项目管理
在一个项目中把服务器相关的配置(公共配置)和程序相关的配置(私有配置)分开，然后公共配置(根据约定大于配置)放到服务器固定的路径，而私有配置则随项目一起发布。亲自实践，项目发布效果显著提升。
##### Elasticsearch
* 同一类型的数据用相同的前缀和模板