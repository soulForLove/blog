---
title: 'SpringBoot学习之配置文件的加载顺序'
date: 2017-10-18 14:40:03
categories: [SpringBoot]
tags: [SpringBoot]
description: 习惯优于配置
copyright: true
comments: true
---
<!-- more -->
## spring boot通过jar包启动时，配置文件的加载顺序

1. @TestPropertySource 注解 
2.  命令行参数 
3.  Java系统属性（System.getProperties()） 
4.  操作系统环境变量 
5.  只有在random.*里包含的属性会产生一个RandomValuePropertySource 
6.  在打包的jar外的应用程序配置文件
（通过  java  -jar demo.jar  --spring.config.location=/path/test_evn.properties ） 
7.  在打包的jar内的应用程序配置文件（application.properties，包含YAML和profile变量） 
8.  在@Configuration类上的@PropertySource注解 
9.  默认属性（使用SpringApplication.setDefaultProperties指定） 
所以，针对6和7的情况，我们一般在应用程序外部放置配置yml文件，防止每次发布时被篡改！[参考](http://www.cnblogs.com/softidea/p/5759180.html)
