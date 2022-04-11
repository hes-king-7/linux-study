# 高性能消息队列：kafka
## kafka由来：
1. kafka是为了解决linkedin的数据管道问题。初期linkedin采用ActiveMQ进行数据交换，2010年后，经常由于各种缺陷而导致消息阻塞或服务无法正常访问。linkedin的jay kreps便自己研发了kafka。
## 什么是kafka
1. kafka由apache软件基金会开发的一个开源流处理平台。由Scala和Java编写。是一种高吞吐量的分布式发布订阅消息系统。可以处理网站中的所有动作流数据。
2. kafka的目的是通过Hadoop的并行加载机制来统一线上和离线的消息处理。也是为了通过集群来提供实时的消息。
## kafka的特性
1. 具有横向扩展，容错，wicked fast(变态快)等优点。
1. 通过0/1的磁盘数据结构提供消息的持久化，能够保持长时间的稳定性能。
2. 高吞吐量； 普通硬件kafka也可以支持每秒数百万的消息
3. 支持通过kafka服务器和消费机集群来分区消息
4. 支持Hadoop并行数据加载
5. Kafka通过官网发布了最新版本2.3.0
## kafka相关术语
1. Broker：kafka集群中的服务名称
2. Topic：每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic
3. Partition：物理上的概念。每个Topic包含一个或多个
4. Producer： 负责发布消息到Kafka broker。。生产消息到Topic的一方
5. Consumer： 消息消费者，向Kafka broker读取消息的客户端。
6. Consumer Group： 每个consumer属于一个特定的组，若不指定组名。则属于默认组。
7. 消息发送时都被发送到一个topic，其本质就是一个目录，而topic由是由一些Partition Logs(分区日志)组成
## kafka的核心组件
1. Replications、Partitions和Leaders：
2. Producers：
3. Consumers：
## kafka核心特性
1. 压缩： 支持对消息集合进行压缩。Producer端可以通过GZIP或Snappy格式对消息集合进行压缩。
2. 消息可靠性：
3. 备份机制：是kafka0.8的新特性。kafka允许集群中的节点挂掉后而不影响整个集群工作。
4. kafka默认端口号：9092
## kafka常用面试：
1. apache kafka是什么：
   1. 是一款分布式流处理框架，用于实时构建流处理应用。作为企业级的消息引擎被广泛使用。
2. 什么是消费者组
   1. 消费者组是kafka提供的可扩展且具有容错性的消费者机制。消费者组的位移提交机制。消费者组与Borker之间的交互。消费者组要消费的数据完全来自于Producer端生产的消息。
3. 在Kafka中，ZooKeeper的作用
   1. kafka使用ZooKeeper存放集群元数据、成员管理、Controller选举，以及其他一些管理类任务。之后，等KIP-500提案完成，Kafka不再依赖ZooKeeper。
   2. KIP-500:是使用社区自研的基于Raft的共识算法。替代Zookeeper，实现Controller自选举。
4. Kafka中位移(offset)的作用
   1. kafka中，每个主题分区下的每条消息都有一个唯一的ID值，用于标识它在分区的位置。这个ID值，就被称为位移或偏移量。一旦消息被写入分区日志。位移值不能改
   2. 日志中的存放格式、Log Cleaner组件不能影响位移值。
   3. 位移值和消费者位移值之间的区别。
5. kafka中领导者副本(leader Replica)和追随者副本(Follower Replica)的区别
   1. 只有leader副本才能对外提供读写服务。响应clients端的请求。follower只是采用拉pull的方式，被动地同步leader副本中的数据。
   2. 自kafka2.4版本后，社区通过引入broker端参数，允许follower副本有限度地提供读服务。
6. 如何设置kafka能接收的最大消息的大小
   1. Broker端参数设置(需要同步调整follower副本能够接收的最大消息的大小。否则副本同步会失败)。Consumer端参数设置。
7. 监控KAFKA的框架有哪些
   1. kafka Manager、kafka monitor、CruiseContol、JMX、JMXTool命令行工具等。
8. Broker的heap size如何设置
   1. 将Broker的Heap Size固定为6GB。这个大小多公司验证，足够且良好。
9. 如何估算kafka集群的机器数量
   1.  确定带宽。阐述带宽占用接近总带宽90%时。丢包情况的发生。
10. leader总是-1，怎么破
    1.  leader为-1代表此主题分区不能工作了。这个时候可以重启(不建议)或删除Zookeeper节点/controller。触发Controller重选举。重选举能够为所有主题分区刷新分区状态。可以有效解决因不一致导致的leader不可用问题。
11. LEO、LSO、AR、LSR、HW都表示什么。
    1.  LEO:Log End Offset 日志末端位移值或末端偏移量。
    2.  LSO:Log Stable Offset kafka事务的概念。
    3.  AR：Assigned Replicas： 分区创建时被分配的副本集合。个数由副本因子决定。
    4.  ISR：In-Sync Replicas： 指代AR中与leader保持同步的副本集合。
    5.  HW：High watermark： 高水位值。控制消费者可读取消息范围的重要字段。
12. kafka能手动删除消息吗
    1.  kafka本身提供留存策略，能够自动删除过期消息，当然，也支持手动删除。
13. kafka可以按照过期时间和存储消息的大小保留。
14. 使用kafka集群数量最好不好超过7个，因为节点越多，消息复制需要时间就越长，群组吞吐量就越底。七群数量最好单数。设置为单数容错率更高。

