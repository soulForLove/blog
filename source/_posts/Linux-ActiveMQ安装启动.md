---
title: Linux ActiveMQ安装启动
date: 2017-11-16 17:45:14
categories: [ActiveMQ]
tags: [ActiveMQ,linux]
description: Linux环境下ActiveMQ安装启动
copyright: true
comments: true
---
## 下载
* ActiveMQ官网：http://activemq.apache.org ， 下载apache-activemq-5.14.4-bin.tar.gz
* 安装jdk，配置环境变量

## 安装

* 上传apache-activemq-5.12.1-bin.tar.gz到linux服务器，并解压
* 部分目录说明：
```
    bin目录：(windows下面的bat和unix/linux下面的sh) 启动ActiveMQ的启动服务就在这里

    conf目录： activeMQ配置目录，包含最基本的activeMQ配置文件

    data目录：activeMQ的日志文件目录

    webapps目录：系统管理员web控制界面文件
```
## 启动
* 进入bin目录，执行命令：
```
./activemq start
```
## 访问
*  启动成功就可以以 http://ip地址:8161 方式访问WEB管理界面，默认用户名和密码admin/admin
* ActiveMQ启动后，默认会启用8161和61616两个端口
>    8161端口为ActiveMQ的web管理控制端口， 61616为ActiveMQ的通讯端口


## 配置
* web管理界面默认的用户名为admin/admin，其配置文件位于./conf/jetty-realm.properties
* 通信端口的定义在ActiveMQ的主配置文件，./conf/activemq.xml
```
        <transportConnectors>
            <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
            <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
  <!--   <transportConnector name="amqp" uri="amqp://0.0.0.0:5672?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>-->
  <!--   <transportConnector name="stomp" uri="stomp://0.0.0.0:61613?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>-->
            <transportConnector name="mqtt" uri="mqtt://0.0.0.0:1883?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
  <!--   <transportConnector name="ws" uri="ws://0.0.0.0:61614?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>-->
        </transportConnectors>
```
* web管理端口的修改，activemq使用了jetty服务器来进行管理， 我们可以在conf/jetty.xml文件中对其配置，web管理端口默认为8161，定义在jetty.xml文件
```
    <bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
             <!-- the default port number for the web console -->
        <property name="host" value="0.0.0.0"/>
        <property name="port" value="8161"/>
    </bean>
```
* 自動清空沒在使用的 topic
```
------broker 元素加上 schedulePeriodForDestinationPurge 的屬性 (10秒):------
<broker xmlns="http://activemq.apache.org/schema/core" brokerName="localhost" dataDirectory="${activemq.data}" schedulePeriodForDestinationPurge="10000">

------policyEntry 元素加上 gcInactiveDestinations 跟 inactiveTimoutBeforeGC 的屬性 (分別是開啟, 跟 20秒):------
<policyEntry topic=">" gcInactiveDestinations="true" inactiveTimoutBeforeGC="20000" >
```
## 关闭服务
* 进入bin目录，执行命令：
```
./activemq stop
```