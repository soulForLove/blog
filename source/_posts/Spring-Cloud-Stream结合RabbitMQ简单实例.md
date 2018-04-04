---
title: Spring Cloud Stream结合RabbitMQ简单实例
date: 2018-03-27 14:10:18
updated: 2018-03-27 14:10:18
categories: [RabbitMQ]
tags: [RabbitMQ,Stream]
description: Spring Cloud Stream是一个用来为微服务应用构建消息驱动能力的框架。它可以基于Spring Boot 来创建独立的，可用于生产的Spring 应用程序。他通过使用Spring Integration来连接消息代理中间件以实现消息事件驱动。Spring Cloud Stream 为一些供应商的消息中间件产品提供了个性化的自动化配置实现，引用了发布-订阅、消费组、分区的三个核心概念。目前仅支持RabbitMQ、Kafka。
copyright: true
comments: true
---
<!-- more -->
## [了解Spring Cloud Stream](https://springcloud.cc/spring-cloud-dalston.html#_spring_cloud_stream)
## [了解RabbitMQ](http://blog.linzhongtai.cn/2018/03/%E4%B9%9D%E6%B5%85%E4%B8%80%E6%B7%B1%E4%B9%8BRabbitMQ/)
## 各项目中引入依赖
```
<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```
## 生产者A
### A项目中新建一个接口作为通道
> NotifyChannelConstant为自定义常量
> @Output注解代表这是一个输出通道

```
@Component
public interface PdfNotifyChannel {

    @Output(NotifyChannelConstant.PDF_NOTIFY_CHANNEL)
    MessageChannel output();
}
```

### 绑定接口@EnableBinding
* 进行消息通知的类，需要添加@EnableBinding(PdfNotifyChannel.class)，指定绑定的接口通道
* 在类中进行初始化
```
 @Autowired
 private PdfNotifyChannel pdfNotifyChannel;
```
* 业务方法中进行调用
```
pdfNotifyChannel.output().send(MessageBuilder.withPayload(msg).build());
```
## 消费者B

### B项目中新建一个接口作为通道
> NotifyChannelConstant为自定义常量，此处引用的是A项目中的常量
> @Input注解代表这是一个输入通道，通道名需要与生产者对应才能接收消息
```
@Component
public interface PdfNotifyChannel {
    @Input(NotifyChannelConstant.PDF_NOTIFY_CHANNEL)
    MessageChannel input();
}
```
### 绑定接口@EnableBinding
* 进行消息接收的类，需要添加@EnableBinding(PdfNotifyChannel.class)，指定绑定的接口通道
* 监听
使用@StreamListener进行监听该通道中的信息
```
/**
  * 监听推送信息
  * @param message
  */
 @StreamListener(NotifyChannelConstant.PDF_NOTIFY_CHANNEL)
 public void receiverPdfNotify(Message<String> message) {
        LOGGER.info("频道[{}]监听信息为:[{}]", 
        NotifyChannelConstant.PDF_NOTIFY_CHANNEL, message.getPayload());  
}
```

## 配置RabbitMQ
> 需要AB配置一致
```
spring.rabbitmq.host=localhost
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
#默认5672
spring.rabbitmq.port=5673
#默认/
spring.rabbitmq.virtual-host=/
```
## RabbitMQ相关命令
```
# 查看当前所有用户
$ sudo rabbitmqctl list_users

# 查看默认guest用户的权限
$ sudo rabbitmqctl list_user_permissions guest

# 由于RabbitMQ默认的账号用户名和密码都是guest。为了安全起见, 先删掉默认用户
$ sudo rabbitmqctl delete_user guest

# 添加新用户
$ sudo rabbitmqctl add_user username password

# 设置用户tag
$ sudo rabbitmqctl set_user_tags username administrator

# 赋予用户默认vhost的全部操作权限
$ sudo rabbitmqctl set_permissions -p / username ".*" ".*" ".*"

# 查看用户的权限
$ sudo rabbitmqctl list_user_permissions username
```