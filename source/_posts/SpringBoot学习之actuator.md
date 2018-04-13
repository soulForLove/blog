---
title: SpringBoot学习之actuator
date: 2018-04-12 16:50:14
updated: 2018-04-12 16:50:14
categories: [SpringBoot]
tags: [SpringBoot]
description: 提供一系列用户监控的端点
copyright: true
comments: true
---
<!-- more -->
## 应用配置类

### /autoconfig
用来获取应用的自动化配置报告，其中包括所有自动化配置的候选项。该端点可以帮助我们方便找到一些自动化配置为什么没有生效的具体原因。
* positiveMatches：返回条件匹配成功的自动化配置
* negativeMatches：返回条件匹配不成功的自动化配置

### /beans
获取应用上下文创建的所有Bean

### /configprops
获取应用中配置的属性信息报告。prefix属性代表了属性的配置前缀，properties代表了各个属性的名称和值。如果要关闭该端点，通过使用endpoints.configprops.enabled=false来完成设置

### /env
该端点主要是用来获取应用中所有可用的环境属性报告。包括环境变量、JVM属性、应用的配置属性、命令行的参数。该端点会进行隐私保护，对于password、serect、key等关键词会使用*来替代实际的属性值

### /mappings
返回所有SpringMVC的控制器映射关系报告

### /info
返回自定义的配置信息，该自定义配置信息需要以info为前缀进行配置，例如：
```
info.app.name=spring-hello
info.app.version=v0.0.1
```

## 度量指标类

### /metrics
返回当前应用的各类重要度量指标，比如内存信息、线程信息、垃圾回收信息等。
* 系统信息：包括处理器数量processors、运行时间uptime和instance.uptime、系统平均负载systemload.average
* mem.* ：内存概要情况
* heap.* ：堆内存使用情况
* noheap.* ： 非堆内存使用情况
* threads.*：线程使用情况，包括线程数、守护线程数（daemon）、线程峰值（peak）等
* classes.*：应用加载和卸载的类统计
* gc.*：垃圾收集器的详细信息，包括垃圾回收次数gc.ps_scavenge.count、垃圾回收消耗时间gc.ps_scavenge.time、标记-清楚算法的次数gc.ps_marksweep.count、标记-清楚算法的消耗时间gc.ps_marksweep.time
* httpsessions.*：Tomcat容器的会话使用情况，包括最大会话数httpsessions.max和活跃会话数httpsessions.active，该度量指标仅在引入嵌入式Tomcat作为应用容器才会提供
* gauge.* ：HTTP请求的性能指标之一，主要用来反映一个绝对数值，比如gauge.response.hello:5，表示上一次hello请求的延迟时间为5毫秒
* counter.*：HTTP请求的性能指标之一，主要作为计数器使用，记录了增加量和减少量，counter.status.200.hello:11，表示hello请求返回200状态的次数为11


> 可以通过/metrics/{name}接口来获取更细粒度的度量信息，比如通过/metrics/mem.free来获取当前可用内存数量

### /health
获取应用的各类健康指标信息

###  /dump
用来暴露程序运行中的线程信息

### /trace
返回基本的HTTP跟踪信息，始终保留最近的100条请求记录

## 操作控制类
在原生端点中，只提供了一个用来关闭应用的端点：/shutdown，通过如下配置开启
```
endpoints.shutdown.enabled=true
```
配置好了之后，只要访问该端点，就能实现关闭该应用的远程操作，后续Eureka中还会有许多控制端点...
