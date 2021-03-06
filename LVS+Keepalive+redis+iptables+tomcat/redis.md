# 什么是NOSQL
## 什么是NOSQL
1. Nosql(Not Only SQL),泛指非关系型数据库。
## 为什么需要NoSQL
1. 传统关系型数据库在超大规模和高并发的SNS类的web2.0纯动态网站暴漏了很多问题。
   1. high preformance-                             对数据库高并发读写的需求
   2. huge storage-                                 对海量数据的高效率存储和访问的需求
   3. high scalability && high availability-        对数据库的高可扩展性和高可用性的需求
## nosql数据库的四大分类
1. 键值(Key-Value)存储数据库：
   1. 相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
   2. 典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 缓存、分布式集群
   3. 特点：一系列键值对
   4. 优势：快速查询(由存储结构决定)
   5. 缺点：存储的数据缺少结构化
2. 列存储数据库：
   1. 相关产品：Cassandra, HBase, Riak (大数据)
   2. 典型应用：分布式的文件系统
   3. 数据模型：以列簇式存储，将同一列数据存在一起
   4. 优势：查找速度快，可扩展性强，更容易进行分布式扩展
   5. 劣势：功能相对局限
3. 文档型数据库
   1. 相关产品：CouchDB、MongoDB 类似html
   2. 典型应用：Web应用（与Key-Value类似，Value是结构化的）
   3. 数据模型： 一系列键值对
   4. 优势：数据结构要求不严格
   5. 劣势： 查询性能不高，而且缺乏统一的查询语法
4. 图形(Graph)数据库(腾讯) A B C D E
   1. 相关数据库：Neo4J、InfoGrid、Infinite Graph
   2. 典型应用：社交网络
   3. 数据模型：图结构
   4. 优势：利用图结构相关算法。
   5. 劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案
5. NoSQL的特点：
   1. 易扩展
   2. 大数据量，高性能
   3. 灵活的数据模型
   4. 高可用 
## redis由来
1. redis：2008年，意大利创业公司Merzia的创始人Salvatore Sanfilippo为了避免Mysql的低性能，亲自做一个数据库，于09年开发完成了redis的最初版本。
## redis是什么
1. redis是一个开源的使用ANSI C语言编写的。支持网络、可基于内存亦可持久化的日志型、高性能键值对(Key-Value)数据库。
## redis的特性
1. 速度快，性能高：redis数据存储在内存里。配合高效的网络IO模型。
2. 丰富的数据结构：redis支持string(字符串)、list(双向链表)、dict/(hash表)、set(集合)和zset(排序set)等不同的数据类型。
3. 多语言支持：redis客户端提供一个简单的通信协议，包括java、c++、php等。
4. 持久化：redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启后可以再次加载进行使用。
5. 高可用，分布式
6. 为应用海量数据存储的要求，redis提供了对高可用与分布式应用的支持。包括复制，cluster等。
## redis存储
1. redis使用了两种文件格式：全量数据和增量请求
   1. 全量数据：把内存中数据写入磁盘，便于下次读取文件进行加载
   2. 增量请求：把内存中数据序列化为操作请求，用于读取文件进行replay得到数据。
2. redis的存储分为内存存储、磁盘存储和log文件三部分。
### redis典型应用场景
1. 缓存系统：redis作为一个中间缓存服务器使用。
2. 数据共享分布式：string类型，redis分布式独立服务。多应用共享。
3. 分布式锁、全局ID
4. 计数器：利用redis的原子自增指令incr可以实现计数器的功能。
5. 消息队列系统：redis提供了基于内存的消息队列。当对消息可靠性要求不高时，可以使用redis来作为消息队列。
6. 排行榜：利用redis的sortedset有序的特点，以点赞数来作为排序的参考实现。
7. 实时系统：使用redis构建一个BloomFilter可以轻松实现这样的系统。
### redis版本发布
2012年8月2日 redis2.4.16发布->2015年2月 redis3.0.0发布->16年12月2日4.0.0-rc1发布->18年10月5.0.0发布。现在最新稳定版本6.2.6.
1. redis的默认配置目录：一般位于redis的安装目录下的redis.conf 可以使用find / -name redis查找。
2. redis的默认监听端口：6379
3. redis发布订阅(pub/sub)是一种消息通信模式》pub是发送者。sub是订阅者。
### redis 事务
1. redis事务可以一次执行多个命令。可以裂解为一个打包的批量执行脚本。
2. 一个事务从开始到执行经历三个阶段
3. 开始事务-->命令入队-->执行事务
### redis脚本
1. redis脚本使用lua解释器来执行脚本。redis2.6内嵌支持lua环境。执行脚本常用命令为EVAL。
### redis GEO
1. redis GEO主要用于存储地理位置信息，并对存储信息进行操作。在redis3.2版本新增。
### redis stream
1. redis stream是redis5.0版本新增的数据结构，主要用于消息队列。提供了消息的持久化和主备复制功能。可以让任何客户端访问任何时刻数据，且记住每一个客户端的访问位置，还能保证消息不丢失。每个消息都有一个唯一的ID和对应的内容。
### redis 管道技术
1. redis管道技术可以在服务端未相应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。
2. redis管道技术最显著的优势是提高了redis服务的性能。
### redis 分区
1. redis分区是分割数据到多个redis实例的处理过程，每个实例只保存key的一个子集。
2. 分区的优势：构造更大的数据库，扩展计算能力，扩展网络带宽。
3. 分区的缺点：涉及多个key的redis事务不能使用。增删容量复杂。
4. redis分区类型
   1. 范围分区：映射一定范围的对象到特定的redis实例
   2. 哈希分区：用一个hash函数将key转换为一个数字。将整数映射到实例中的一个。