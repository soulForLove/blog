---
title: ActiveMQ与Spring整合
date: 2017-09-29 17:27:17
categories: [ActiveMQ]
tags: [ActiveMQ]
description: 点对点以及发布订阅例子
copyright: true
comments: true
---
<!-- more -->
## 配置文件中引入ActiveMQ.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:amq="http://activemq.apache.org/schema/core"
       xmlns:jms="http://www.springframework.org/schema/jms"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/jms
        http://www.springframework.org/schema/jms/spring-jms-4.0.xsd
        http://activemq.apache.org/schema/core
        http://activemq.apache.org/schema/core/activemq-core-5.8.0.xsd">

    <context:property-placeholder location="classpath:app*.properties" local-override="true" file-encoding="UTF-8"/>

    <!-- ActiveMQ 连接工厂 -->
    <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供-->
    <!-- 如果连接网络：tcp://ip:61616；未连接网络：tcp://localhost:61616 以及用户名，密码-->
    <amq:connectionFactory id="amqConnectionFactory"
                           brokerURL="${jms.broker-url}" userName="admin" password="admin"/>


    <!-- Spring Caching连接工厂 -->
    <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
    <bean id="connectionFactory" class="org.springframework.jms.connection.CachingConnectionFactory">
        <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
        <property name="targetConnectionFactory" ref="amqConnectionFactory"/>
        <!-- 同上，同理 -->
        <!-- <constructor-arg ref="amqConnectionFactory" /> -->
        <!-- Session缓存数量 -->
        <property name="sessionCacheSize" value="100"/>
    </bean>

    <!-- Spring JmsTemplate 的消息生产者 start-->

    <!-- 定义JmsTemplate的Topic类型 -->
    <bean id="jmsTopicTemplate" class="org.springframework.jms.core.JmsTemplate">
        <!-- 这个connectionFactory对应的是我们定义的Spring提供的那个ConnectionFactory对象 -->
        <constructor-arg ref="connectionFactory"/>
        <!-- pub/sub模型（发布/订阅） -->
        <property name="pubSubDomain" value="true"/>
    </bean>

    <!-- 定义JmsTemplate的Queue类型 -->
    <bean id="jmsQueueTemplate" class="org.springframework.jms.core.JmsTemplate">
        <!-- 这个connectionFactory对应的是我们定义的Spring提供的那个ConnectionFactory对象 -->
        <constructor-arg ref="connectionFactory"/>
        <!-- 非pub/sub模型（发布/订阅），即队列模式 -->
        <property name="pubSubDomain" value="false"/>
    </bean>


    <!--Spring JmsTemplate 的消息生产者 end-->


    <!-- 消息消费者 start-->

    <!-- 定义Queue监听器 -->
    <jms:listener-container destination-type="queue" container-type="default" connection-factory="connectionFactory"
                            acknowledge="auto">
        <jms:listener destination="test.queue" ref="queueReceiver1"/>
        <jms:listener destination="test.queue" ref="queueReceiver2"/>
    </jms:listener-container>

    <!-- 定义Topic监听器 -->
    <jms:listener-container destination-type="topic" container-type="default" connection-factory="connectionFactory"
                            acknowledge="auto">
        <jms:listener destination="test.topic" ref="topicReceiver1"/>
        <jms:listener destination="test.topic" ref="topicReceiver2"/>
    </jms:listener-container>
    <!-- 消息消费者 end -->
</beans>
```
## Queue生产者
```
/**
 * @description 队列消息生产者，发送消息到队列
 */
@Component("queueSender")
public class QueueSender {

    @Autowired
    @Qualifier("jmsQueueTemplate")
    private JmsTemplate jmsQueueTemplate;//通过@Qualifier修饰符来注入对应的bean

    /**
     * 发送一条消息到指定的队列（目标）
     *
     * @param queueName 队列名称
     * @param message   消息内容
     */
    public void send(String queueName, final String message) {
        jmsQueueTemplate.convertAndSend(queueName, message);
    }

}
```
## Topic生产者
```

@Component("topicSender")
public class TopicSender {

    @Autowired
    @Qualifier("jmsTopicTemplate")
    private JmsTemplate jmsTemplate;

    /**
     * 发送一条消息到指定的队列（目标）
     *
     * @param topicName 队列名称
     * @param message   消息内容
     */
    public void send(String topicName, final String message) {
        jmsTemplate.convertAndSend(topicName, message);
    }
}
```
## 接口测试
```
@Controller
@RequestMapping("/activemq")
public class ActivemqController {

    @Resource
    private QueueSender queueSender;

    @Resource
    private TopicSender topicSender;

    /**
     * todo 开启activeMQ服务
     * 发送消息到队列
     * Queue队列：仅有一个订阅者会收到消息，消息一旦被处理就不会存在队列中
     * @param message
     * @return String
     */
    @ResponseBody
    @PostMapping("/queueSender")
    public String queueSender(@RequestParam("message") String message) {
        String opt = "";
        try {
            queueSender.send("test.queue", message);
            opt = "success";
        } catch (Exception e) {
            opt = e.getCause().toString();
        }
        return opt;
    }

    /**
     * 发送消息到主题
     * Topic主题 ：放入一个消息，所有订阅者都会收到
     * 这个是主题目的地是一对多的
     *
     * @param message
     * @return String
     */
    @ResponseBody
    @PostMapping("/topicSender")
    public String topicSender(@RequestParam("message") String message) {
        String opt = "";
        try {
            topicSender.send("test.topic", message);
            opt = "success";
        } catch (Exception e) {
            opt = e.getCause().toString();
        }
        return opt;
    }
}
```