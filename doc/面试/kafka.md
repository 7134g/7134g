### kafka

*   拉的好处: 可以根据自己能力拉取消息处理,客户端自由决定拉取一条还是多条

*   突然新增一个消费者导致什么:
    触发重新均衡分配, partition.size/consumer.size 向上取整得 N
    将topic中partition均匀发放到group上的consumer
    例:
    topic1 : 1,2,3,4,5,6    partition数为3
    consumer group 数为2:
    c1: 1,2,3,4
    c2: 5,6
    consumer group 数为3:
    c1: 1,2
    c2: 3,4
    c3: 5,6

*   消费组好处:
    *   一个分区只可以被消费组中的一个消费者所消费
    *   一个消费组中的一个消费者可以消费多个分区
    *   一个消费组中的不同消费者消费的分区一定不会重复
    *   所有消费者一起消费所有的分区
    *   在不同消费组中，每个消费组都会消费所有的分区
    *   同一个消费组里面的消费者对分区是互斥的

*   高可用:
    *   `Broker`中`leader`: A挂了:
        选择`offset`最大的`follower`: B作为新的`leader`,
        选择`offset`最小的`follower`: C作为分割线
        B 通过日志同步数据, A 恢复后从C的分割线开始同步 B数据

    *   `Broker`挂了:
        创建新的`broken`, 新 `bronken.id` 等于宕机的 `bronken.id` 就会自动同步

    *   一个`Topic`的`Partition`数量大于`Broker`的数量, 将会把`partition`的`repalica`分布到不同的`Broker`上，是为了避免当该 `Broker` 发生故障时，对应 `Partition` 的数据将会不可用，从而导致数据丢失或服务不可用

#### kafka读写过程

当你向 Kafka 写入一条数据时，Kafka 会将这条数据追加到一个 topic 的一个 partition 中。

每个 topic 都有多个 partition，每个 partition 只属于一个 broker，而一个 broker 可以拥有多个 partition。

##### 写入数据的过程如下：

1.  客户端将数据发送给 Kafka Producer。

2.  Producer 将消息放入一个 RecordAccumulator 缓冲区中。

3.  当缓冲区被填满或者超时时间到达时，Producer 将缓冲区中的所有消息分配给相应的 partition。

4.  Producer 向 Kafka 集群中的 Broker 发送请求，将消息写入指定的 partition 中。

5.  Broker 接收到消息后将其持久化存储，并向 Producer 发送响应。

6.  如果消息发送失败，则 Producer 会进行重试，直到超过最大重试次数或者成功为止。

##### 读取数据

1.  Kafka 消费者向 Broker 发送拉取请求，请求获取指定 Topic 下指定 Partition 的消息。消费者可以根据需要设置拉取消息的起始位置和大小。

2.  Broker 返回待消费的消息集合给消费者。如果消费者指定了消息的起始位置，Broker 会从该位置开始返回消息；否则，Broker 会返回最新的消息。

3.  消费者将接收到的消息进行处理。处理方式由消费者自己决定，例如可以将消息存储到数据库中、进行业务计算等等。

4.  消费者通过 API 向 Kafka 确认已经消费完某个消息（即提交 Offset）。这样，Kafka 就知道该消息已经被成功处理，并且下次不会再将该消息发送给该消费者。如果消费者没有提交 Offset，Kafka 就会认为该消息还未被消费，下次仍然会将该消息发送给消费者。

5.  如果消费者没有发出确认消息的请求，而是一段时间后重新启动了消费者客户端，Kafka 会自动从上次提交的 Offset 开始将消息推送给消费者，并继续之前的消费流程。

