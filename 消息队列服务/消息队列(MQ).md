# 消息队列MQ(Message queuing)
## MQ的由来
1. 19世纪80年代，美国高盛等公司采用Teknekron公司产品，叫TIB。再然后IBM开发了MQSeries。微软开发了MSMQ。 2004年摩根大通额iMatrix着手AMQP开放标准的开发。 2006年，AMQP规范发布，2007年，Rabbit技术公司基于AMQP标准开发的RabbitMQ1.0发布
## MQ的定义
1. MQ简单理解：要把传输的数据放在队列中。队列又是一种先进先出的数据结构(可以存储数据)。作为一个单独的中间件产品存在，独立部署。
2. 消息队列技术是分布式应用间交换信息的一种技术。消息队列是发送和接收消息的公用存储空间，它可以存在于内存中或者是物理文件中。消息可以以两种方式发送：
   1. 快递方式(express)：把消息放置于内存中，而非磁盘。获取较高的处理能力。
   2. 可恢复模式(recoverable)：把传递过程的每一步骤。都把消息写入物理磁盘。以得到较好的故障恢复能力。消息队列可以部署在发送方，接收方，或第三方设备均可。
3. 把数据放到消息队列的叫做生产者，从消息队列里边取数据的叫做消费者。
## 消息队列的实现方式
1. 消息在被发送者发送后就立即返回，由消息队列来负责消息的传递，消息发布者只管将消息发布到消息队列中而不用管消息的接受。消息使用者只管从消息队列中取出消息而不用管谁来发布的。
## 队列的类型
1. 队列可分为用户创建的队列和系统队列两种
2. 用户创建的队列：
   1. 公共队列：在整个“消息队列”网络中复制，并且有可能由网络连接的所有站点访问。
   2. 专用队列：仅在所驻留的本地计算机上可用。专用队列只能由知道队列的完整路径名或标签的应用程序访问。
   3. 管理队列：在给定“消息队列”网络中发送的消息回执的消息
   4. 响应队列：目标应用程序接收到消息时返回给发送应用程序的响应消息
3. 系统生成的队列：  在日常操作中很可能要使用几种不同的系统队列。
   1. 日记队列：可选地存储发送消息的副本和从队列中移除的消息副本。在服务器上为每个队列创建了一个单独的日记队列。此日记跟踪从该队列中移除的消息。
   2. 死信队列：存储无法传递或已过期的消息的副本
   3. 报告队列：包含指示消息到达目标所经过的路由的消息，还可以包含测试消息。每台计算机上只能有一个报告队列。
   4. 专用系统队列：是一系列存储系统执行消息处理操作所需的管理和通知消息的专用队列。
### 通信
1. 在MQ中，主要有三大类通道类型
   1. 消息通道：用于在MQ的服务器和服务器之间传输消息，单向的。分为发送，接收，请求这，服务者四类。
   2. MQI通道：是MQClient和MQServer之间通讯和传递消息的，双向的。
   3. Cluster通道：集群通道是位于同一个MQ群集内部的队列管理器之间通讯使用的。
2. MQ的通讯模式
   1. 点对点通信：一对一，一对多，多对多。支持树状，网状等多种拓扑。
   2. 多点广播：可以使用一条MQ指令将单一消息发送到多个目标站点。并确保为每一站点可靠地提供信息。其中MQ还包括智能消息分发功能。
   3. 发布/订阅(Publish/Subscribe)模式：包含三个角色：主题，发布者，订阅者。能使消息的分发可以突破目的队列地理指向的限制，使消息按照特定的主题甚至内容进行分发。用户或应用程序可以根据主题或内容接收到所需要的消息。
   4. 群集：类似于域。群集内部通信，采用群集通道。从而大大简化了系统配置。且群集中的队列管理器之间能够自动进行负载均衡。
## 消息队列的作用
1. 消息队列中间件： 主要解决应用解耦，异步消息，流量削峰。日志收集。事务最终一致性等问题。实现高性能，高可用，可伸缩和最终一致性架构。。概括起来就是：异步解耦，削峰填谷。
2. 目前常用消息队列有：
   1. ActiveMQ：Apache出品。多语言和协议编写客户端。
   2. RabbitMQ：用erlang语言开发。
   3. ZeroMQ：类似socket的一系列接口。区别在于socket是端到端。ZMQ可以是N:M。
   4. Kafka：一种高吞吐量的分布式发布订阅消息系统。
   5. MetaMQ & RocketMQ ：metaMQ3.0版本后的改名。基于高可用分布式集群技术。提供低延时、高可靠的消息发布和订阅服务。和kafka类似，不同的是使用Java开发。
## 使用消息队列的理由
1. 解耦：各系统之间通过消息系统这个统一的接口交换数据，无须了解彼此的存在
2. 异步通信 ：在不需要立即处理请求的场景下，可以将请求放入消息系统，合适的时候再处理
3. 削峰/限流： 如果某商场每个月要搞一次大促。大促期间并发很高。服务器不能一下处理这么多并发，那么就可能导致服务器崩溃。这时候服务器就可以把多的并发都写入到MQ中，让底下客户端根据自己能处理的请求并发去消息列队中拿数据。保证系统稳定性。
4. 消息通讯： 消息队列一般内置高效的通信机制。可以实现点对点通讯，聊天室通讯。
5. 日志处理：如kafka应用。解决大量日志传输的问题。一般有：
   1. 日志采集客户端：日志采集，写入kafka队列。
   2. kafka消息队列：日志接收，存储和转发
   3. 日志处理应用： 订阅并消费kafka队列中的日志数据。
## 消息队列带来的问题
1. 高可用：消息队列不能是单机。否则消息队列一卦，整个系统几乎不可用。所以一般都要集群/分布式模式。
2. 系统复杂度提高：MQ的加入大大增加分布式系统复杂度。如何保证消息没有被重复消费，怎么处理消息丢失情况？怎么保证消息传递的顺序性？
3. 一致性问题： A系统通过MQ给B.C.D发消息数据，如果B,D成功，C处理失败。如何保证消息数据的一致性。
