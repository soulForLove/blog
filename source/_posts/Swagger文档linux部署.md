---
title: Swagger文档linux部署
date: 2017-08-29 18:15:41
categories: [Swagger]
tags: [swagger,linux]
description: 接口文档部署
copyright: true
comments: true
---
<!-- more -->
##  安装httpd
## 配置httpd.conf文件
* vim /etc/httpd/conf/httpd.conf
* 添加监听端口：Listen 6080
```
<VirtualHost *:6080>
    DocumentRoot /swagger
    Header set Access-Control-Allow-Origin *
    ServerName *.*.*.*:6080
</VirtualHost>

```

* 修改后需要重启httpd服务：/etc/init.d/httpd restart

## 放置文档文件夹

* /swagger
## 由于SELinux开启造成的Apache 2 Test Page powered by CentOS
* 把SELinux的状态改一下就可以：执行setenforce 0