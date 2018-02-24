---
title: Maven小知识
date: 2017-10-20 14:49:28
updated: 2017-12-15 17:49:28
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

## maven 快照 更新策略
> 为什么会有快照？


* 没有快照之前：
            A项目依赖于项目B，B每次改动就赋予一个新版本号，然后告诉A我改版本好了啊，每次改动都得告诉，有时忘了就麻烦了。
            可以看出没有快照会带来“浪费版本号”、沟通成大加大的问题。

* 有了快照之后：
            A项目依赖于项目B，B每次改动都会打上时间戳，A编译时会检查B的时间戳，如果晚于本地仓库B的时间戳，那么就会进行更新，否则不予更新。
            可以看出快照省去了沟通成本、版本号成本。

> 快照更新策略

注意，**快照并不是每次install就会更新**，这取决于更新策略；快照更新策略，有每日更新、永远检查更新、从不检查更新和自定义时间间隔更新，**默认是每日更新也就是说一日更新一次**，如果想总是更新，那么可以在settings.xml中配置。比如:
```
<profile>  
            <id>nexus</id>  
            <repositories>  
                <repository>  
                    <id>central</id>  
                    <url>http://central</url>  
                    <releases>  
                        <enabled>true</enabled>  
                    </releases>  
                    <snapshots>  
                        <enabled>true</enabled>  
                        <updatePolicy>always</updatePolicy>  
                    </snapshots>  
                </repository>  
            </repositories>  
</profile>  
```

**updatePolicy更新snapshot包的频率，属性有四个值always(实时更新) daily（每天更新） interval:xxx（隔xxx分钟更新一次）  never（从不更新） 默认为daily **
也可以通过命令强制更新，mvn clean install-U 
[参考博文](http://blog.csdn.net/wangjun5159/article/details/49095881)

## maven打包本地jar包，在plugin中设置includeSystemScope为true
```
    <dependency>
        <groupId>llw-base-rpc</groupId>
        <artifactId>llw-base-rpc</artifactId>
        <version>1.0</version>
        <scope>system</scope>
        <systemPath>${project.basedir}/lib/llw-base-rpc.jar</systemPath>
    </dependency>
```
```
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <includeSystemScope>true</includeSystemScope>
            </configuration>
        </plugin>
```