# 第一章 #
## 1.1 背景、JMS概述 ##

**rpc的局限性**  

- 同步通信
- 生命周期耦合，A，B两端必须都正常，不然异常
- 点对点通信，客户端A的某次调用只能发给某个单独目标

**MOM**  
面向消息的中间件(Message Orented Middleware,MOM)   
 
- 接收，发送都是异步的  
- 生命周期不耦合，A，B什么状态没关系  
- 一对多通信，一个消息多个接收者  



## 1.2 JMS术语 ##
- provider:生产者  
- consumer:消费者  
- PTP：point 2 point ，点对点模型  
- pub/sub:发布-订阅模型  
- connectionFactory:链接工厂，jms用它创建连接  
- connetction:jms客户端到jms provider的链接  
- destination:消息目的地  
- seesion：会话，一个发送或接收消息的线程
- message接口：消息头，消息属性，消息体  


# 第二章 #
## 2.1 ActiveMQ简介 ##
- apache开源
- jms的基础

jetty.xml：配置端口号  
jetty-relam.properties:账号，密码  
active.xml:tcp连接地址61616  


## 2.2 消费者中session签收模式 ##
1. 自动签收：消费者建立连接mq，去取消息，取到之后返回数据给消费者，消费者会给mq发送一条确认消息,mq会删除存在队列中的消息。这个就是自动签收。
2. client_acknowledge：消费者收到消息之后，必须手动调用message.acknowledge()方法进行签收。目的也是告诉mq我已经获取到消息了。
3. dups\_ok_acknowledge:没有消息签收机制。可能会重复消费消息。多个consumer并发去读消息的时候，可能会出现多个consumer同时消费一个消息。


## 2.3 事务支持 ##
采用事务支持的时候，需要主动调用
session.commit()
这个数据才会写到mq中  


## 2.4 生产者发消息参数 ##
send(Des,TextMessage,xx，xx，1min)  
(目的地，文本，持久化模式，优先级，过期时间)  
**优先级**：理论上优先被消费。不绝对保证消费者优先消费。优先级级别0--9，默认是4。  


## 2.5 消费者消费消息 ##
顺序消费：指的是先消费1数据，再消费2数据，再消费3数据。（业务逻辑上，2要1消费的结果，所以有消费的顺序）  
优先级：和顺序是不一样的。  
  
    

