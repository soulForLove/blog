---
title: Redis发布订阅
date: 2017-09-19 09:20:21
categories: [redis]
tags: [redis]
description: Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
copyright: true
comments: true
---
<!-- more -->
> 发布订阅(pub/sub)是一种消息通信模式，主要的目的是解耦消息发布者和消息订阅者之间的耦合，这点和设计模式中的观察者模式比较相似。pub /sub不仅仅解决发布者和订阅者直接代码级别耦合也解决两者在物理部署上的耦合。redis作为一个pub/sub server，在订阅者和发布者之间起到了消息路由的功能。订阅者可以通过subscribe和psubscribe命令向redis server订阅自己感兴趣的消息类型，redis将消息类型称为通道(channel)。当发布者通过publish命令向redis server发送特定类型的消息时。订阅该消息类型的全部client都会收到此消息。这里消息的传递是多对多的。一个client可以订阅多个 channel,也可以向多个channel发送消息。
* 配置文件
```
<bean id="jedisConnectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
        <property name="hostName" value="${redis.host}"/>
        <property name="password" value="${redis.password}"/>
        <property name="port" value="${redis.port}"/>
        <property name="database" value="${redis.database}"/>
    </bean>

    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxIdle" value="${redis.maxIdle}"/>
        <property name="maxTotal" value="${redis.maxActive}"/>
        <property name="testOnBorrow" value="${redis.testOnBorrow}"/>
        <property name="maxWaitMillis" value="${redis.timeout}"/>
    </bean>

    <bean scope="prototype" id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="jedisConnectionFactory"/>
        <property name="keySerializer" ref="stringRedisSerializer"/>
        <property name="hashKeySerializer" ref="stringRedisSerializer"/>
    </bean>

    <bean id="stringRedisSerializer" class="org.springframework.data.redis.serializer.StringRedisSerializer"/>

    <bean id="redisMessageListener" class="com.gengee.gsm.util.RedisMessageListener"/>

    <redis:listener-container connection-factory="jedisConnectionFactory">
        <redis:listener ref="redisMessageListener" topic="schedule:pdfUrl" />
    </redis:listener-container>
```
* RedisMessageListener
```
public class RedisMessageListener implements MessageListener {

    private static final Logger LOGGER = LoggerFactory.getLogger(RedisMessageListener.class);
   @Autowired
    private ScheduleLogMapper scheduleLogMapper;
    @Override
    public void onMessage(Message message, byte[] bytes) {
        if (message != null && !message.toString().equals("")) {
            LOGGER.info(message.toString());
            //do you job
        }
    }
}
```
>参考文档：http://www.cnblogs.com/yitudake/p/6747995.html
> http://blog.csdn.net/valenon/article/details/46414455