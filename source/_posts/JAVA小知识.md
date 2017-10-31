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
