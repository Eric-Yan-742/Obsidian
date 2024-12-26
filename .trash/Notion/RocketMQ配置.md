## 融合云

- 点进Topic 权限管理，write用于生产者端，read用于消费者端。权限决定程序拿着某一用户组的ak/sk能不能读/写到Topic。注意权限修改后等三分钟才生效。

## Producer

- 版本：
    - Java 1.8
    - `spring-boot-starter-parent` 2.2.2.RELEASE
    - `rocketmq-spring-boot-starter` 2.2.2
    - **For spring boot 3.3.2, rocketmq starter must be 2.3.1**
- `rocketmq.name-server` 一定要有
- `rocketmq.producer.group` 一定要有，起什么名字都可以，不一定是用户组，也不一定是订阅组。
- `rocketmq.producer.access-key` 和 `rocketmq.producer.secret-key` 一定要有。就是topic权限管理的用户组中的ak和sk。
    
    ![[_attachments/image 8 2.png|image 8 2.png]]
    
- 除了在 application.properties中配置，也可以用 `@ExtRocketMQTemplateConfiguration` 配置。可以在这里覆写全局的nameServer或group，**但全局的也一定要有。**
    
    ```Java
    @ExtRocketMQTemplateConfiguration(
            group = "yyq-producer-group",
            nameServer = "staging-cnbj2-rocketmq.namesrv.api.xiaomi.net:9876",
            accessKey = "AKBUFH4MKZQ2K33IQ5",
            secretKey = "ppjKV8+BkX0xjBgvBq4sOHFyYUTHILIV3T7Vu/4K"
    )
    public class MyRocketMQTemplate extends RocketMQTemplate {}
    ```
    

## Consumer

- `rocketmq.name-server` , `rocketmq.consumer.access-key` , `rocketmq.consumer.secret-key` 一定要有。除了在application.properties中配置也可以在 `@RocketMQMessageListener` 中配置，这两者是互补的，一个全局一个局部。
- `rocketmq.consumer.group` 一定是订阅组管理中创建的某个组
    
    ![[_attachments/image 9 2.png|image 9 2.png]]
    

## 消息轨迹

- `enableMsgTrace = true` 一定要写在 `@ExtRocketMQTemplateConfiguration` 和`@RocketMQMessageListener` 。写在全局 [application.properties](http://application.properties) 没效果。