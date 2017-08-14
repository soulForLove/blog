---
title: SpringMVC 视图解析出错
date: 2017-07-31 14:44:04
categories: SpringMVC #文章分类
tags: [SpringMVC,配置] #文章标签
description: 配置问题
comments: true
---
<!-- more -->
当没有在配置文件中配置``<mvc:annotation-driven/>``,程序执行没有出现问题，但是postman会报如下图的问题：
![image](http://otkzd4sua.bkt.clouddn.com/driven.png)
<center>总结</center>
如果没有``<mvc:annotation-driven/>``，那么所有的Controller可能就没有解析，所有当有请求时候都没有匹配的处理请求类，就都去``<mvc:default-servlet-handler/>``即default servlet处理了。添加上``<mvc:annotation-driven/>``后，相应的do请求被Controller处理。总之没有相应的Controller就会被default servlet处理。
所以要使用spring mvc中的@Controller注解
就必须要配置``<mvc:annotation-driven />``否则org.springframework.web.servlet.DispatcherServlet无法找到控制器并把请求分发到控制器
