---
title: Redis主从复制
date: 2018-05-10 22:44:20
updated: 2018-05-10 22:44:20
categories: redis
tags: redis
description:  一主二仆/薪火相传/反客为主...
copyright: true
comments: true
---
<!-- more -->
## 配置从库，不配主库
## 从库配置：slaveof **主库ip** **主库端口**
每次与master断开后，都需要重新连接，除非配置redis.conf文件
## 修改配置文件
## 常用3招
* 一主二仆
* 薪火相传
* 反客为主（slaveof no one）

## 复制原理
salve 启动成功连接到master后会发送一个sync命令，master接到命令后启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，进程执行完毕后，master将传送整个数据文件到slave，以完成一次完全同步，也就是**全量复制**，而slave服务在接收到数据后，存盘到内存中；master将新的修改命令依次传给slave，完成同步，此时为**增量复制**
> 只要重新连接master，一次完全同步（全量复制）将被自动执行

## 哨兵模式
> 反客为主的自动版本，监控主机是否故障，当主库挂了，根据投票数重新选定master

* 新建sentinel.conf
* 配置文件内容
```
sentinel monitor 自定义名称 监控库ip 监控库端口 1
```
* 启动哨兵
```
redis-sentinel /usr/local/redis/sentinel.conf
```
自动监控，选好新master后，原master恢复后会变成slave
> 一组sentinel可以同时监控多个master

## 复制的缺点
由于所有的写操作都在master上，然后同步更新到slave上，所以从master同步到slave   机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，slave机器数量的增加也会使得这个问题更加严重