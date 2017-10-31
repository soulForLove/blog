---
title: Linux小知识
date: 2017-10-24 17:32:18
categories: [Linux]
tags: [Linux,小知识]
description: Linux 相关知识点
copyright: true
comments: true
---
<!-- more -->
## linux 执行jar

* nohup java -jar -XX:InitialHeapSize=32m -XX:MaxHeapSize=128m -XX:+HeapDumpOnOutOfMemoryError background-1.0.0-SNAPSHOT.jar >> background.log 2>&1 &

## linux命令

* 查看是否存在相关进程(Back)： ps ax | grep Back	