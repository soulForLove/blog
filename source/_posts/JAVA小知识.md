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