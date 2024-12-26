![[_attachments/RocketMq 2.doc|RocketMq 2.doc]]

[[RocketMQ配置]]

# MQ结构

- **消费者组**内的所有消费者实例必须订7阅完全相同的Topic和Tag，以保持订阅关系的一致性。如果订阅关系不一致，可能会导致消息消费逻辑混乱，甚至消息丢失
- 消费者点位：指向最后消费了的消息，消费者**返回ok**就移位，不是消费了就移位
- 代理者点位：指向最新生产的消息
- 代理者点位 - 消费者点位 = 差值（还未消费的消息数）

  

# 消息模型

- push模式和pull模式
    - pull模式长轮询：隔一会拉一次消息，例如5s，隔一会再拉一次消息。
    - push模式：消息由消息队列主动推送（或者说分发）到消费者。消费者无需主动请求消息，而是被动地接收消息，并进行处理（用的多）
    - Spring SDK consumer默认是push模式。
- 原生SDK中不管是DefaultMQPushConsumer还是DefaultMQPullConsumer，底层其实都是pull模式
- 批量发送消息会发送到一个队列
- List<MessageExt> msgs
    - `**MessageExt**` 是 RocketMQ 的一个类，表示从消息队列中消费的一条消息。它是 `Message` 类的扩展，包含了更多关于消息的元数据，例如消息的主题、标签、消息 ID、消息的产生时间等。
    - `**List<MessageExt> msgs**` 表示消费端一次接收到的一批消息。RocketMQ 支持批量消费，所以消息消费监听器（如 `MessageListenerConcurrently` 的实现类）每次调用 `consumeMessage` 方法时，可能会收到多条消息。
    - 不管是push还是pull模式，一次都有可能收到多条消息。
- 在 RocketMQ 中，如果两个消费者组订阅了同一个 Topic，那么每条消息都会被发送到每个消费者组中的一个消费者。也就是说，每个消费者组都会各自收到一份消息。跟广播/集群模式没关系。
- 如果消息已经被Broker销毁，那么这条消息就不会再被发送到新的consumer group。

# 同步/异步/单向/延迟消息

- 发送一条类消息，rocketMQ会自动把消息内容转换为json
    
    ![[_attachments/image 12.png|image 12.png]]
    
- 接收消息时如果Listener的类型是一个类类型，但消息内容并不是符合要求的json字符串，程序会报错，消息会消费失败。MessageExt 类型不会出现这种问题。
- 发送异步消息，主线程提前结束会导致发送失败，需要想办法让主线程在回调之前一直运行。
- 延迟消息的延迟时间是消息被延迟监听的时间，而不是过了延迟时间后才发送。消息实际上是立即被发送的。

# 顺序消息

- MessageListenerOrderly通过加分布式锁和本地锁保证同时只有一条线程去消费一个队列上的数据。

# 事务消息

- [https://rocketmq.apache.org/zh/docs/4.x/producer/06message5/](https://rocketmq.apache.org/zh/docs/4.x/producer/06message5/)
- 用途：在分布式系统下，保证本地事务与消息发送的一致性
    1. 只有本地事务成功了才真正发送消息。防止先发消息再进行本地事务，结果消息发出去了但本地事务失败了。

# tag

- 同一个消费者组内必须订阅相同的topic和topic下相同的tag
    - [https://rocketmq.apache.org/zh/docs/4.x/bestPractice/07subscribe](https://rocketmq.apache.org/zh/docs/4.x/bestPractice/07subscribe)
- 普通消息、事务消息、定时（延时）消息、顺序消息，不同的消息类型使用不同的 Topic，无法通过 Tag 进行区分。
- 如果消费者在初始化时只指定了Topic而不指定Tag，它将订阅该Topic的所有Tag

# key

- 业务自己定义的唯一值，用于避免消费者重复消费消息

# 重复消费解决方案

# 消息重试和死信消息

![[_attachments/image 1 6.png|image 1 6.png]]

- 如果队列中堆积了多个消费失败的消息，谁的重试时间到了就重试谁，同时消费新消息。
- 消息的充实时间是Broker控制的。也就是说Consumer下线了重试时间也会继续记着
- Consumer每次重启的第一次消费都不算重试次数
- 如果setMaxReconsumeTime为2，那就不会进行第3次重试，第二次重试后直接扔到死信队列。死信队列不算在这个主题里。

# 消费模式

- 集群模式：消费组里所有消费者均分消息（用的多）
    
    ![[_attachments/image 2 3.png|image 2 3.png]]
    
- 广播模式：消费者组里每一条消息会被每一个消费者处理一次。
    
    ![[_attachments/image 3 3.png|image 3 3.png]]
    
    ![[_attachments/image 4 3.png|image 4 3.png]]
    
    - 广播模式不会记录点位，消费失败不会重试
- 在同一个应用程序中，一个消费者组中不能同时存在两个广播模式消费者
- 尽管技术上可行，但混合使用不同的消费模式在实际应用中并不常见。因为它会带来复杂性，增加系统的不可预测性。通常，每个 `Topic` 的消费者要么都是 `Clustering`，要么都是 `Broadcasting`。

# 解决消息堆积问题

- 原因1：生产者生产太快了
    1. 增加消费者数量，但注意消费者数量 ≤ 队列数量，因为消费者平分队列。
    2. 调整消费者线程池，设置最大消费线程数量。
    3. 动态扩容队列数量，从而增加消费者数量（一般不这么干）
- 原因2：消费者挂了
    1. 排查消费者程序问题
    2. 可跳过堆积的消息。

# 如何确保消息不丢失

- 将mq的刷盘机制设置为同步刷盘。消息几乎不会丢失，但磁盘IO较慢
- 使用集群模式 ，搞主备模式，将消息持久化在不同的硬件上
- 在本地做持久化
    - 生产者同步发送消息，收到mq返回确认后，往自己数据库里记录 msgId create time， status 记为 1
    - 消费者消费之后，记录 msgId, handletime 把 status 改为 2
    - 写一个定时任务，间隔两天去查一次数据。如果有 status = 1 并且 create time < day - 2。说明消息大概率丢了。
- 开启 trace（消息跟踪）机制  
      
      
    

# Spring集成

- Message 的 payload 就是消息体