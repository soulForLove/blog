---
title: Maven小知识
date: 2017-10-20 14:49:28
categories: [Maven]
tags: [Maven,小知识]
description: 项目管理相关知识
copyright: true
comments: true
---

## Maven 的classifier的作用
* maven中要引入json包
```
<dependency>  
    <groupId>net.sf.json-lib</groupId>   
    <artifactId>json-lib</artifactId>   
    <version>2.2.2</version>  
</dependency>  
```
* 当执行mvn install 命令时，却抛出一个错误，说找不到net.sf.json-lib:json-lib:2.2.2这个包，到仓库中看一下http://repo2.maven.org/maven2/net/sf/json-lib/json-lib/2.2.2/
![image](http://otkzd4sua.bkt.clouddn.com/maven1.png)
*  jar的名称中多了一个跟JDK相关的名称，例如jdk15，按照上面的配置，明显是找不到这个jar的，于是classifier就有它的用武之地了，它表示在相同版本下针对不同的环境或者jdk使用的jar,如果配置了这个元素，则会将这个元素名在加在最后来查找相应的jar，例如：
```
<dependency>  
    <groupId>net.sf.json-lib</groupId>   
    <artifactId>json-lib</artifactId>   
    <version>2.2.2</version>  
        <classifier>jdk15</classifier>    
</dependency>  
```
* 这样配置即可找到json-lib-2.2.2-jdk15.jar 