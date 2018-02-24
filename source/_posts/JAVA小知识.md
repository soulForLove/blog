---
title: JAVA小知识
date: 2017-09-15 15:39:08
categories: [JAVA]
tags: [JAVA,小知识]
description: java 相关小知识点
copyright: true
comments: true
---
<!-- more -->

## &和&&的区别
* &和^,<<,<<<,|同属于位运算符,其中&是按位与,例如,1&1=1，1&0=0
* &&是逻辑运算符,处理真假值，例如,true&&true=true。

## mybatis中的CDATA标签的用法
* 在mapper文件中写sql语句时，遇到特殊字符时，如：< 等，建议使用<![CDATA[ sql 语句 ]]>标记，将sql语句包裹住，不被解析器解析


## Http跨域OPTIONS请求
跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

## OPTIONS请求报错

由于预检请求（OPTIONS请求）不会包含Cookie信息（浏览器本身的实现决定其是否发送Cookie，前端无法控制，并且Chrome是不发送的），因此被权限拦截器提前结束，没有输出包含指定头部信息的响应。而一个被浏览器认为合格的预检请求响应必须包含如下的Http头部。(
预检请求Options的执行顺序在拦截器之后，因此预检请求被拦截，导致出错,)

* 方法一：在拦截器中加响应头

```
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        if (request.getHeader(HttpHeaders.ORIGIN) != null) {
            response.addHeader("Access-Control-Allow-Origin", "*");
            response.addHeader("Access-Control-Allow-Credentials", "true");
            response.addHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE, PUT, HEAD");
            response.addHeader("Access-Control-Allow-Headers", "Content-Type,token");
            response.addHeader("Access-Control-Max-Age", "3600");
            return true;
        }
    }

```

* 方法二：在mvcConfig中添加跨域设置：-----无效！待验证！
```
    //请求跨域
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins(CorsConfiguration.ALL)
                .allowCredentials(true)//表示是否允许发送Cookie。
                默认情况下，Cookie不包括在CORS请求之中。
                设为true，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。
                这个值也只能设为true，如果服务器不要浏览器发送Cookie，删除该字段即可
                .allowedHeaders("Content-Type", "token")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS");
        super.addCorsMappings(registry);
    }


```

## Tomcat报错： Note: further occurrences of HTTP header parsing errors will be logged at DEBUG level.
	网上是说在tomcat的server.xml文件里面的<Connector> 里面设定maxHttpHeaderSize属性，但是并没有效果，最后的原因是：用了https请求了本地接口，GG。。。

##  java作用域public ,private ,protected 及不写时的区别

> 对于继承自己的class，base class可以认为他们都是自己的子女，而对于和自己一个目录下的classes，认为都是自己的朋友


* public：public表明该数据成员、成员函数是对所有用户开放的，所有用户都可以直接进行调用 
* private：private表示私有，私有的意思就是除了class自己之外，任何人都不可以直接使用，私有财产神圣不可侵犯嘛，即便是子女，朋友，都不可以使用
* protected：protected对于子女、朋友来说，就是public的，可以自由使用，没有任何限制，而对于其他的外部class，protected就变成private

```
作用域       当前类    同一package   子孙类     其他package 

public        √       √             √           √ 

protected     √       √             √           × 

friendly      √       √             ×           × 

private       √       ×             ×           × 

不写时默认为friendly
```

## 图片上传耗时太久
> **问题描述：**页面上传图片，提交后耗时8s才上传 成功，debug页面发现TTFB是主要原因，等待服务器返回的时间过长。

1. 首先将服务器对应的ip端口开放，直接用postman调用接口，发现耗时一样很长，排除服务器nginx代理以及路由导致的原因
2. 发现服务器带宽只有2M，下载理论峰值256k左右，上传只有128左右，1024/128=8s ，原因就是因为服务器带宽不足，导致上传时间消耗比较多