---
title: Nginx小知识
date: 2017-12-26 17:17:40
updated: 2017-12-26 17:17:40
categories: [nginx]
tags: [nginx,小知识]
description: something about Nginx
copyright: true
comments: true
---
<!-- more -->
## Nginx 禁止IP访问
> 为了避免别人把未备案的域名解析到自己的服务器IP而导致服务器被断网

* 在配置文件nginx.conf中新建一个server
```
 server {
    listen 80 default;
    server_name _;
    return 500;
  }
```
* 原先的server中，server_name可以设置多个域名

## Nginx 代理，http识别不到js，但是https可以识别到？

*  由于在https的server中没有对js/css进行操作，但是在http的server中，发现了下面这一段：

```
location ~ .*\.(js|css)?$
    {
      expires 15d;
    }
```

* 将需要识别的文件所在的端口的地址放进去

```
location ~ .*\.(js|css)?$
    {
     # expires 15d;
      proxy_pass http://127.0.0.1:8088;
   }
```

* 或者直接将这一段删除即可

##  nginx 出现413 Request Entity Too Large问题的解决方法

> nginx默认上传文件的大小是1M，可nginx的设置中修改


1. 打开nginx配置文件 nginx.conf

2. 在http{}段中加入 client_max_body_size 20m; 20m为允许最大上传的大小

3. 保存后重启nginx