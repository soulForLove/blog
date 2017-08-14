---
title: 使用MyBatis Generator自动创建代码
date: 2017-07-24 20:40:41
categories: MyBatis #文章分类
tags: [mybatis] #文章标签
description: 跟风小技能
comments: true
---
> 由于MyBatis属于一种半自动的ORM框架，所以主要的工作就是配置Mapping映射文件，但是由于手写映射文件很容易出错，所以可利用MyBatis生成器自动生成实体类、DAO接口和Mapping映射文件。这样可以省去很多的功夫，将生成的代码copy到项目工程中即可

![image](http://otkzd4sua.bkt.clouddn.com/mybatis.png)
上图文件下载地址：http://download.csdn.net/detail/u012909091/7206091

其中有mybatis框架的jar包，数据库驱动程序jar包以及MyBatis生成器jar包。其中的generatorConfig.xml是需要我们来配置的文件，配置如下：
![image](http://otkzd4sua.bkt.clouddn.com/generator.png)
       
当以上这些完成之后，只需要打开控制台，进入lib目录下，执行脚本：
Java -jar mybatis-generator-core-1.3.2.jar -configfile generatorConfig.xml -overwrite
即可。

这样在生成之后，就可以在src目录下找到相应的文件夹，每个表格都会对应三个文件：实体类、接口、配置文件


转载要注明出处：谢谢[shu_lin](http://blog.csdn.net/zhshulin/article/details/23912615)