---
title: Ajax 跨域请求
date: 2017-08-12 10:06:01
categories: [Ajax]
tags: [ajax,跨域]
description: Access-Control-Allow-Origin
copyright: true
comments: true
---
<!-- more -->
## 错误信息
```
jquery-3.1.1.min.js:4 XMLHttpRequest 
cannot load http://127.0.0.1:8080/user/login.action. 
No 'Access-Control-Allow-Origin' header 
is present on the requested resource. 
Origin 'http://localhost:8080' is therefore not allowed access.
```
## 解决办法

### 服务端设置支持跨域

#### @CrossOrigin注解
* 即在Controller控制器中，在Controller注解上方添加@CrossOrigin注解。
* 
```
@CrossOrigin(origins = {"*"}, maxAge = 3600)
public classUserController{}
```
* 官网：www.fhadmin.org也可以在Controller控制器中的每个方法中分别添加@CrossOrigin注解。
```
@CrossOrigin(origins = {"*"}, maxAge = 3600)
public String login(String username, String password)throws Exception {}
```
* 假如添加之后还是出现了跨域问题，需要给映射路径中配置请求方法（method）
```
@RequestMapping(value = "user", method = {RequestMethod.POST})
@CrossOrigin(origins = {"*"}, maxAge = 3600)
public class UserController {}
```
#### CORS全局配置
* 基于xml的配置，在springmvc.xml中配置
```
<!-- 跨域请求 -->
<mvc:cors>
    <mvc:mappingpath="/user/*"/>
</mvc:cors>
```
* 官网：www.fhadmin.org 可以进行详细的配置
```
<mvc:cors>
    <mvc:mappingpath="/api/**"allowed-origins="http://domain1.com, 
    http://domain2.com"allowed-methods="GET, PUT
    "allowed-headers="header1, header2, header3"
    exposed-headers="header1, header2"
    allow-credentials="false"max-age="123" />
    <mvc:mappingpath="/resources/**"allowed-origins="http://domain1.com" />
</mvc:cors>
```
* 假如出现"通配符的匹配很全面, 但无法找到元素 'mvc:cors' 的声明。"这样的错误
解决办法：
    查看文件上边beans中xsd文件引入的版本是不是不对。
如下所示：
    http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
引入的xsd版本为3.0，而mvc:cors是4.2版本的功能。因此，只需要将xsd版本更新就行。或者设置成4.2以上的。
    http://www.springframework.org/schema/mvc/spring-mvc.xsd
#### PHP 
   ```
    header("Access-Control-Allow-Origin:*"); //*号表示所有域名都可以访问  
   ```
### JSONP实现跨域

* 常用的jquery实现跨域调用
```
$.ajax({
    url: "",
    dataType: "jsonp",
    jsonp: "callback",
    success: function(data) {
        console.log(data);
    }
});
```
这个调用实际上的实现原理是：
在网页中构造一个script标签，将src设置为对应的url，并增加上相应的callback参数，形如如下格式：
```
<script src="http://127.0.0.1/index?callback=jQuery211018970995225637144_1465350372062&_=1465350372063"></script>
```
请求的服务端代码：
```
String jsoncallback=request.getParameter("callback");//指定接受参数为callback
return jsoncallback+"([{name:'jsonp',age:'30'},{name:'jack',age:'90'}])";
```
* jsonp实现的缺点
jsonp实现的跨域方式不支持post请求，只能支持get请求

