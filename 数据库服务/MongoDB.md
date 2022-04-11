# MongoDB是什么
1. MongoDB是一个基于分布式文件存储的数据库，由C++语言编写。旨在为web应用提供可扩展的高性能数据
存储解决方案。
2. MongoDB是一个介于关系数据库和非关系数据库之间的产品。
3. MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。
## MongoDB特点
1. 面向文档存储。操作简单
2. 可设置任何属性的索引，来实现更快排序
3. 可通过本地或网络创建数据镜像。有更强的扩展性。
4. 可以分布在计算机网络中的其他节点上这就是所谓的分片。
5. 支持丰富的查询表达式。可轻易查询文档中内嵌的对象及数组等。
6. MongoDB支持：C.c++.php.python.go等多种语言结构。
### MongoDB
1. 2007年10月。由10gen团队发展，2009年2月首度推出。2012年5月23日。2.1开发分支发布。
2. 提供了网络和系统监控工具Munin，它作为一个插件应用于Mongo中
3. Gangila是Mongo的高性能系统监视工具，作为一个插件应用于Mongo中。
4. 基于图形界面的开源工具Cacti，用于查看CPU负载，网络带宽利用率等，也提供了一个监控Mongo插件。
### GUI
Fang of Mongo – 网页式,由Django和jQuery所构成。
Futon4Mongo – 一个CouchDB Futon web的mongodb山寨版。
Mongo3 – Ruby写成。
MongoHub – 适用于OSX的应用程序。
Opricot – 一个基于浏览器的MongoDB控制台, 由PHP撰写而成。
Database Master — Windows的mongodb管理工具
RockMongo — 最好的PHP语言的MongoDB管理工具，轻量级, 支持多国语言.
### MongoDB的使用
1. MongoDB 预编译二进制包下载地址：https://www.mongodb.com/download-center/community
2. win安装自查。linux安装步骤如下
3. 先安装相应依赖包 yum install libcurl openssl
4. 下载对应tar.gz包。然后为其添加到PATH路径。默认可执行文件位于bin目录下。
5. 创建数据库目录 /var/lib/mongodb 数据存储目录     /var/lib/mongodb   日志文件目录。
6. 然后为当前用户设置此目录有读写权限。后安装完成后启动服务。默认去到/usr/local/mongodb4/bin下找到mongo文件。./mongoshell方式执行。启动后即可执行各种操作。
### 数据库信息
一个mongodb中可以建立多个数据库。
MongoDB的默认数据库为"db"，该数据库存储在data目录中。
MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。
1. 默认数据库名： admin；相当于root数据库。local：不会被复制，存储限于本地单台服务器的任意集合。 config： 分片设置时，config在内部使用，保存分片的相关信息。
2. 文档(Document)：文档是一组键值(key-value)对(即 BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型。
3. 集合：集合就是 MongoDB 文档组，类似于 RDBMS中的表格。集合存在于数据库中，集合没有固定的结构
4. 元数据： 数据库的信息时存储在集合中。它们使用了系统的命名空间。
### MongoDB数据类型
1. string 字符串；integer 整型数值；Boolean 布尔值；Double 存储浮点值；Min/Max keys 将一个值于BSON元素的最低值和最高值相对比； Array：将数组，列表存储为一个键。 Timestamp：时间戳。Object：内嵌文档。 Null：创建空值。Symbol：符号。date：日期时间。Objectld：类似唯一主键。
### MongoDB链接；
1. 标准URL链接 mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
2. 默认端口链接： mongodb://localhost
3. shell链接：./mongo
4. 命令格式链接： mongodb://admin:123456@localhost/test 使用admin用户直接连接到test库。
### MongoDB增删查改命令
1. use DATABASE_NAME 新建数据库。默认数据库test  使用show dbs可查看所有数据库。
2. 先进入数据库 执行db.dropDatabase(name) 删除集合：db.collection.drop(name)
3. db.createCollection(name，options可选参数)
4. 插入文档 db.COLLECTION_NAME.insert(document)或db.COLLECTION_NAME.save(document)
5. 更新文档 db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true}) true表示修改多条相同文档。
6. MongoDB条件操作符  
   1. (>) 大于 - $gt
   2. (<) 小于 - $lt
   3. (>=) 大于等于 - $gte
   4. (<= ) 小于等于 - $lte
   5. $type 如db.col.find({"title" : {$type : 'string'}})
7. limit语法 db.COLLECTION_NAME.find().limit(NUMBER) 读取指定数量的数据记录。
8. skip()跳过指定数量的数据。语法：db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
9. sort()对数据进行排序。1升序,-1降序。db.COLLECTION_NAME.find().sort({KEY:1})
### 索引
1. 索引可极大提高查询效率，如果没有索引，数据库在读取数据时必须扫描集合中的每个文件并选取哪些符合查询条件的记录。索引存储在一个易于遍历读取的数据集合中，是对数据库表中一列或多列的值进行排序的一种结构。
2. MongoDB使用createindex()方法来创建索引。语法： db.collection.createIndex(keys, options)
3. 也可实现后台创建索引：db.values.createIndex({open: 1, close: 1}, {background: true})
4. 索引关键字段必须时Date类型。只支持单字段索引，不支持混合索引。
5. MongoDB 中聚合(aggregate)主要用于处理数据(诸如统计平均值，求和等)，并返回计算后的数据结果。语法：db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
### 管道概念
1. 管道在unix和inux中一般用于将当前命令的输出结果作为下一个命令的参数。
2. MongoDB的聚合管道将MongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。管道操作是可以重复的。
3. 表达式：处理输入文档并输出。表达式是无状态的，只能用于计算当前聚合管道的文档，不能处理其它的文档。
### MongoDB复制(副本集)
1. MongoDB复制是将数据同步在多个服务器的过程。复制提供了数据的冗余备份，并在多个服务器上存储数据副本，提高了数据的可用性， 并可以保证数据的安全性。
2. mongodb的复制至少需要两个节点。其中一个是主节点，负责处理客户端请求，其余的都是从节点，负责复制主节点上的数据。mongodb各个节点常见的搭配方式为：一主一从、一主多从。
3. 主节点记录在其上的所有操作oplog，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，从而保证从节点的数据与主节点一致。
### MongoDB分片
1. 在Mongodb里面存在另一种集群，就是分片技术,可以满足MongoDB数据量大量增长的需求。
2. 当MongoDB存储海量的数据时，一台机器可能不足以存储数据，也可能不足以提供可接受的读写吞吐量。这时，我们就可以通过在多台机器上分割数据，使得数据库系统能存储和处理更多的数据。
3. 分片的三个主要组件： 
   1. shard：存储实际的数据块
   2. Config Server：Mongodb实例。存储整个ClusterMetadata。包括chunk信息
   3. Query Routers： 前端路由，客户端接入入口。让集群看上去像单一数据库。前端应用可透明使用。
### MongoDB数据备份(dump)与恢复(mongorestore)
1. 在MongoDB中我们使用mongodump命令来备份MongoDB数据。该命令可以导出所有数据到指定目录。
2. 语法：mongodump -h dbhost -d dbname -o dbdirectory
3. 使用mongorestore来恢复备份数据
4. 语法：mongorestore -h <hostname><:port> -d dbname <path>
### MongoDB监控
1. 在你已经安装部署并允许MongoDB服务后，你必须要了解MongoDB的运行情况，并查看MongoDB的性能。这样在大流量得情况下可以很好的应对并保证MongoDB正常运作。
2. MongoDB中提供了mongostat 和 mongotop 两个命令来监控MongoDB的运行情况。
3. mongostat是mongodb自带的状态检测工具，在命令行下使用。它会间隔固定时间获取mongodb的当前运行状态，并输出。
4. \set up\mongodb\bin>mongostat
5. mongotop也是mongodb下的一个内置工具，mongotop提供了一个方法，用来跟踪一个MongoDB的实例，查看哪些大量的时间花费在读取和写入数据。 mongotop提供每个集合的水平的统计数据
6. \set up\mongodb\bin>mongotop