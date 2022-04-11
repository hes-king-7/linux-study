# ELK && EFK
## 什么是ELK
1. ELK是Elasticsearch、Logstash、Kibana三大开源框架首字母大写简称。
2. ELK最底层是Beats和Logstash，中间层是Elasticsearch，最上层是kibana
   1. 最底层负责接收数据和预处理。
   2. 中间层负责数据的持久化与分析，是ELK的核心。
   3. 最上层主要负责对中间层数据进行可视化展示。
3. Elasticsearch是强大的数据搜索引擎，是分布式、通过restful方式进行交互的近实时搜索平台框架。
4. Logstash是免费且开放的服务器端数据处理通道，能够从多个来源收集数据，转换数据后将数据发送到数据存储库中。能够动态地采集、转换和传输数据，不受格式和复杂度的影响。
5. Kibana是针对Elasticsearch的开元分析及可视化平台，用于搜索、查看交互存储在Elasticsearch索引中的数据。
## 当前主流的集中日志管理系统
1. 简单的：Rsyslog
2. 商业化：Splunk
3. 开源的： Scribe(FaceBook),Chukwa(Apache)
4. ELK最广泛的：(Elastic Stack。Java语言编写)
## ELK的作用
1. ELK组和就是一套完整的日志系统。系统运维和开发人员需要通过日志去排查问题、分析系统允许情况、了解服务器的负荷等。以便及时采取应对措施。
2. 在分布式中，构建一套集中式的日志系统可以提高我们定位问题的效率。
3. 完整的系统日志特征：
   1. 收集：能够采集多种来源的日志数
   2. 传输：能够稳定把数据解析过来并传输至存储系统
   3. 存储：存储日志数据
   4. 分析：支持UI分享
   5. 警告：能够提供错误报告，有监控机制。
4. Filebeat：用于转发和汇总日志与文件。
   1. 当将数据发送到 Logstash 或 Elasticsearch 时，Filebeat 使用背压敏感协议，以考虑更多的数据量。如果 Logstash 正在忙于处理数据，则可以让 Filebeat 知道减慢读取速度。一旦拥堵得到解决，Filebeat 就会恢复到原来的步伐并继续运行。
   2. 无论在任何环境中，随时都潜伏着应用程序中断的风险。Filebeat 能够读取并转发日志行，如果出现中断，还会在一切恢复正常后，从中断前停止的位置继续开始。
5. Logstash：由两个必要组件INPUT和OUTPUT和一个可选组件FILTER组成。in主要负责数据读取。fi主要负责数据的预处理。out负责数据输出。
6. 索引生命周期管理：
7. 基于时间可将索引数据分为四个阶段：
   1. Hot：索引数据被大量更新与新增
   2. Warm：索引数据不再被更新，单用户对处于该阶段的索引任务由查询需求
   3. Cold：索引数据不再被更新，用户对此阶段索引查询需求底且可以容忍较大延迟
   4. Delete：数据不再需要，索引可被删除。
## 什么是EFK
1. EFK是一套解决方案。是Elasticsearch，FileBeat/fluentd，Kibana的缩写，其中EL负责日志保存和搜索。fi负责收集日志，ki负责界面。
## elk和efk的区别
1. efk就是把elk的logstash替换成了filebeat或/fluentd。因为fi比lo有两个好处。
   1. 侵入低。无需修改程序目前任何代码和配置。
   2. lo性能高，但是相对于IO占用大。
2. lo相对于fi来说，对日志的格式化要好于fi。
3. filebeat隶属于beats。目前beats包含六种工具：
   1. Packetbeat（搜集网络流量数据）
   2. Metricbeat（搜集系统、进程和文件系统级别的 CPU 和内存使用情况等数据）
   3. Filebeat（搜集文件数据）
   4. Winlogbeat（搜集 Windows 事件日志数据）
   5. Auditbeat（ 轻量型审计日志采集器）
   6. Heartbeat（轻量级服务器健康采集器）
4. 什么是fluentd：Fluentd 是一个开源的数据收集器，专为处理数据流设计，使用 JSON 作为数据格式。它采用了插件式的架构，具有高可扩展性高可用性，同时还实现了高可靠的信息转发。Fluentd 也是云原生基金会（CNCF）的成员项目之一，遵循Apache 2 License 协议。Fluentd 与 Logstash 相比，比占用内存更少、社区更活跃。
