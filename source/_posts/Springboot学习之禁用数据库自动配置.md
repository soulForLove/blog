---
title: Springboot学习之禁用数据库自动配置
date: 2018-05-15 15:16:45
updated: 2018-05-15 15:16:45
categories: SpringBoot
tags: SpringBoot
description: springboot项目启动时，如果没有配置数据库配置，启动时会抛出异常...
copyright: true
comments: true
---
<!--more-->
SpringBoot默认会自动配置数据库，如果业务不需要，就要手动禁用数据库自动配置,在Application的SpringBootApplication注解里加上
```
@SpringBootApplication(exclude = {
	DataSourceAutoConfiguration.class,
	DataSourceTransactionManagerAutoConfiguration.class,
	HibernateJpaAutoConfiguration.class
})
```