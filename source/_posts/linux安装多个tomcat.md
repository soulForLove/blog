---
title: linux安装多个tomcat
date: 2017-09-04 20:13:15
categories: [linux]
tags: [linux,tomcat]
description: linux环境下多个tomcat部署
copyright: true
comments: true
---
<!-- more -->
## 编辑环境变量：vi /etc/profile
1. 加入以下代码(tomcat路径要配置自己实际的tomcat安装目录)
```
##########first tomcat###########
CATALINA_BASE=/usr/local/tomcat
CATALINA_HOME=/usr/local/tomcat
TOMCAT_HOME=/usr/local/tomcat
export CATALINA_BASE CATALINA_HOME TOMCAT_HOME
##########first tomcat############
##########second tomcat##########
CATALINA_2_BASE=/usr/local/tomcat_2
CATALINA_2_HOME=/usr/local/tomcat_2
TOMCAT_2_HOME=/usr/local/tomcat_2
export CATALINA_2_BASE CATALINA_2_HOME TOMCAT_2_HOME
##########second tomcat##########
```

2. 保存退出（:wq）
3. source /etc/profile   生效
 
##  修改tomcat配置文件
1. 第一个tomcat，保持解压后的原状不用修改, 
 
2. 来到第二个tomcat的bin目录下，
打开catalina.sh ，找到下面红字：

```
 # OS specific support.  $var _must_ be set to either true or false.
```

3. 在下面增加如下代码
```
export CATALINA_BASE=$CATALINA_2_BASE
export CATALINA_HOME=$CATALINA_2_HOME
```
4. 来到第二个tomcat的conf目录下
打开server.xml更改端口：
>修改server.xml配置和第一个不同的启动、关闭监听端口。

修改后示例如下：
```
 <Server port="9005" shutdown="SHUTDOWN">　               端口：8005->9005
<!-- Define a non-SSL HTTP/1.1 Connector on port 8080 -->
    <Connector port="8081" maxHttpHeaderSize="8192"　       端口：8080->8081
maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
               enableLookups="false" redirectPort="8443" acceptCount="100"
               connectionTimeout="20000" disableUploadTimeout="true" />
<!-- Define an AJP 1.3 Connector on port 8009 -->
    <Connector port="9009"                                  端口：8009->9009
               enableLookups="false" redirectPort="8443" protocol="AJP/1.3" />
```
5. 分别进入两个tomcat的bin目录，启动tomcat--./startup.sh

然后访问http://localhost:8080 和 http://localhost:8081 都可以看到熟悉的tomcat欢迎界面。

如果想启动多个可以依此法类推……