---
title: Bug小知识
date: 2017-09-28 19:56:40
categories: [Bug]
tags: [Bug,小知识]
description: 踩坑之旅
copyright: true
comments: true
---
<!-- more -->
1. 数据库字段类型与模型类型要一一对应
2. 空指针验证anytime
3. 会变动的配置数据，不应当存到数据库当中。例如，一个比赛发布地址（http://127.0.0.1/schedule/address/{scheduleId}），地址前缀是配置项，在数据库只需在比赛表设定一个状态，验证是否发布，调用时候再进行拼接即可。