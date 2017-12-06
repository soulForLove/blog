---
title: CentOS7(firewall)开放端口和关闭防火墙
date: 2017-11-14 09:36:50
categories: linux #文章分类
tags: [linux] #文章标签
description: linux开放端口和关闭防火墙
comments: true
copyright: true
---

<!--more-->
## 开放端口
### 永久的开放需要的端口
```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

### 之后检查新的防火墙规则
```
firewall-cmd --list-all
```
## 关闭防火墙
```
//临时关闭防火墙,重启后会重新自动打开
systemctl restart firewalld
//检查防火墙状态
firewall-cmd --state
firewall-cmd --list-all
//Disable firewall
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld
//Enable firewall
systemctl enable firewalld
systemctl start firewalld
systemctl status firewallad
```