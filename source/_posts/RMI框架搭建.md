---
title: RMI框架搭建
date: 2017-08-08 15:00:48
categories: RMI #文章分类
tags: [搭建,RMI] #文章标签
description: RMI远程调用
comments: true
<!-- top: 10 -->
copyright: true
---
<!-- more -->
## 创建子模块
一个作为公有接口模块（Common），一个作为接口实现模块（Component），一个作为引用接口模块（ApiCenter）
Component 引入Common 去实现接口方法
ApiCenter    引入Common 去调用接口方法
### 公有模块
```
<artifactId>Common</artifactId>
<version>1.0.0-SNAPSHOT</version>
<packaging>jar</packaging>
```
只需要创建接口文件即可，配置文件resource以及webapp都可以删除
### 接口实现模块
```
<artifactId>Component</artifactId>
<packaging>war</packaging>
```
在接口实现模块的pom.xml，需要引入对接口模块的依赖
```
<dependency>
    <groupId>**********</groupId>
    <artifactId>Common</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```
新建接口实现类BaseService（自动扫描）
```
@Service
public class BaseService implements CommonInterface {

    public String getName() {
        return "success";
    }
}

```
Spring Mvc配置文件中引入rmi配置文件（新建rmi配置文件）
```
 <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
# 此时这个/baseService即为之后调用接口的方法
    <bean name="/baseService" class="org.springframework.remoting.httpinvoker.HttpInvokerServiceExporter">
        <property name="service" ref="baseService"/>
        <property name="serviceInterface" value="***.***.***.CommonInterface"/>#指的是Common模块的接口
    </bean>
</beans>
```
###  引用接口模块
```
<artifactId>ApiCenter</artifactId>
<packaging>war</packaging>
```
在接口实现模块的pom.xml，需要引入对接口模块的依赖
```
<dependency>
    <groupId>**********</groupId>
    <artifactId>Common</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```
Spring Mvc配置文件中引入rmi配置文件（新建rmi配置文件）
```
 <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
<bean id="baseService" class="org.springframework.remoting.httpinvoker.HttpInvokerProxyFactoryBean">
        <property name="serviceInterface" value="***.***.***.CommonInterface"/>
        <property name="serviceUrl" value="${rmi.server.host}/baseService"/>
    </bean>
</beans>
```
rmi.server.host应当配置在配置文件里头
```
# RMI 远程调用服务接口
rmi.server.host=http://127.0.0.1:8083/Component
# Component模块war包部署在tomcat8083端口
```
调用接口方法：
```
@Autowired
private CommonInterface commonService;
/**
 * 获取商品列表
 *
 * @param idleResModel
 * @return
 */
@GetMapping("/list")
public Map<String, Object> getIdleList(@Valid IdleResModel idleResModel) {
    Map<String, Object> resultMap = new HashMap<>();
    resultMap.put("test", commonService.getName());
    return resultMap;
}
```

