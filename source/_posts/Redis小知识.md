---
title: Redis小知识
date: 2017-12-05 09:11:35
categories: [redis]
tags: [redis,小知识]
description: Redis 相关知识点
copyright: true
comments: true
---
<!-- more -->

##  更改Redis报序列化错误，ClassNotFind
> 问题描述： 更改环境上线，由于依赖的A项目更改为旧的redis，因此B项目也改为旧的redis，发送短信时，系统报错，报错信息是：**序列化错误，找不到对应的模型**

* 检查发现，涉及到对应的逻辑代码已经几个月没有更新过
* 重启Tomcat，清除Tomcat缓存，无效
* 还原redis配置，短信发送成功

**原因：** `由于更改为旧的redis配置，登录redis客户端，发现对应的消息队列中还有一些没有被消费掉的消息，这些消息是之前旧的模型序列化进来，现在第一条序列化出去就找不到对应的模型，因此报错`
** 更改旧版redis配置 &rArr; 旧消息队列没有被消费 &rArr; 重新连接总是读取第一条进行序列化 &rArr; 如果代码更改或者模型变更 &rArr; 导致序列化错误，找不到对应的模型 &rArr; redis需要设置过期时间进行自动清除 **
**解决：**
`设置缓存过期时间一天，且redis错误提示信息应当更加友好！`
redisTemplate.expire("testKey",1l,TimeUnit.DAYS);