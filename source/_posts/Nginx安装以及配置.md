---
title: Nginx安装以及配置
date: 2017-11-24 14:57:06
categories: [nginx]
tags: [nginx]
description: 高性能的HTTP和反向代理服务器
copyright: true
comments: true
---
<!-- more -->

## 安装编译工具及库文件

```
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```
## 安装 PCRE
###   下载 PCRE 安装包

```
[root@bogon src]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
```
###   解压安装包

```
[root@bogon src]# tar zxvf pcre-8.35.tar.gz 
```
###   进入安装包目录

```
[root@bogon src]# cd pcre-8.35
```
###   编译安装 

```
[root@bogon pcre-8.35]# ./configure
[root@bogon pcre-8.35]# make && make install
```
###   查看pcre版本
```
[root@bogon pcre-8.35]# pcre-config --version
```

##  安装 Nginx

### 下载Nginx
```
[root@bogon src]# wget http://nginx.org/download/nginx-1.6.2.tar.gz
```

###   解压安装包

```
[root@bogon src]# tar zxvf nginx-1.6.2.tar.gz
```
###   进入安装目录

```
[root@bogon src]# cd nginx-1.6.2
```

### 编译安装

```
[root@bogon nginx-1.6.2]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
[root@bogon nginx-1.6.2]# make
[root@bogon nginx-1.6.2]# make install
```

### 查看Nginx版本
```
[root@bogon nginx-1.6.2]# /usr/local/webserver/nginx/sbin/nginx -v
```

##  Nginx 配置

### 创建 Nginx 运行使用的用户 www
```
[root@bogon conf]# /usr/sbin/groupadd www 
[root@bogon conf]# /usr/sbin/useradd -g www www
```

### 配置nginx.conf 

* 将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容配置nginx.conf ，将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容:
```
user www www;
worker_processes 2; #设置值和CPU核心数一致
error_log /usr/local/webserver/nginx/logs/nginx_error.log crit; #日志位置和日志级别
pid /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;
events
{
  use epoll;
  worker_connections 65535;
}
http
{
  include mime.types;
  default_type application/octet-stream;
  log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
               '$status $body_bytes_sent "$http_referer" '
               '"$http_user_agent" $http_x_forwarded_for';
  
#charset gb2312;
     
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 8m;
     
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 60;
  tcp_nodelay on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  gzip on; 
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
 
  #limit_zone crawler $binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录
    location /aaa {
      proxy_pass http://127.0.0.1:8080/aaa;
    }
    location /abcd {
      proxy_pass http://127.0.0.1:8081/abcd;
    }
    location /yiwu {
     proxy_pass http://127.0.0.1:8081/yiwu;
    }
      location ~ .*\.(php|php5)?$
    {
      #fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
    {
      expires 30d;
  # access_log off;
    }
    location ~ .*\.(js|css)?$
    {
      expires 15d;
   # access_log off;
    }
    access_log off;
  }
  server {
    listen 443 ssl;
    server_name localhost;
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/214335641040602.pem;
    ssl_certificate_key  cert/214335641040602.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root html;
        index index.html index.htm;
    }
    location /aaa {
       proxy_pass http://127.0.0.1:8080/aaa;
    }
    location /abcd {
       proxy_pass http://127.0.0.1:8081/abcd;
    }
  }
}
```
* 在conf目录新建cert文件夹，将证书文件（阿里云免费证书：pem，key）放置cert，并且加入一个配置server：（这个server是https的配置，原先的server是对于http的配置）
```
server {
    listen 443 ssl;
    server_name localhost;
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/214335641040602.pem;
    ssl_certificate_key  cert/214335641040602.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root html;
        index index.html index.htm;
    }
    location /bjjc {
       proxy_pass http://127.0.0.1:8080/bjjc;
    }
    location /yiwu {
       proxy_pass http://127.0.0.1:8081/yiwu;
    }
  }

```
### 检查配置文件ngnix.conf的正确性命令
```
[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx -t
```

## 启动 Nginx
```
[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx
```
启动后可以根据ip访问成功！
* Nginx其他命令
```
/usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/webserver/nginx/sbin/nginx -s stop              # 停止 Nginx
```
> 安装后启动报错，原因是tomcat80端口冲突
