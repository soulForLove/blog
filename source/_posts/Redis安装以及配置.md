---
title: Redis安装以及配置
date: 2018-01-21 14:45:49
updated: 2018-01-21 14:45:49
categories: [redis]
tags: [redis]
description: linux 环境安装部署redis
copyright: true
comments: true
---
<!-- more -->
## 下载
http://redis.io/download
## 解压
tar zxvf redis-2.8.17.tar.gz
## 编译并安装
```
cd redis-2.8.17
make 
cd src
make install PREFIX=/usr/local/redis
```
* make编译如果失败，因为没有安装gcc服务
```
yum install gcc---安装gcc
rpm -qa |grep gcc---查看安装是否成功 
```
## 将配置文件移动到redis安装目录下
进入redis目录，创建etc文件夹
mv redis.conf /usr/local/redis/etc
## 启动服务、配置
```
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
第一个是启动redis服务器
第二个是启动服务器所需的配置
```
## 设置后台运行
vim /usr/local/redis/etc/redis.conf
将daemonize的值改为yes
## 设置开机自启
vim /etc/rc.local
加入
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis-conf
## 客户端链接
/usr/local/redis/bin/redis-cli 
## 停止服务
/usr/local/redis/bin/redis-cli shutdown
或者
pkill redis-server
