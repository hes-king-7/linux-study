# Zabbix
## 为什么要用监控
1. 需要的时候，提前提醒服务器出问题了
2. 出问题之后，可以找到问题的根源
3. 网站/服务器的可用性
## 监控范围
1. 硬件监控、系统监控、服务监控、性能监控、日志监控、安全监控、网络监控。
## 常用监控工具
1. mrtg         流量监控出图
2. magios       监控
3. cacti        流量监控出图
4. zabbix       监控+出图
## Zabbix是什么
1. zabbix是由Alexei Vladishev开发的一个基于WEB界面的提供 分布式系统监控以及网络监视功能的企业级的开源解决方案。依赖与lamp/lnmp环境。
2. zabbix主要由2部分构成： zabbix server端和可选组件zabbix agent客户端。zabbix proxy 是用来管理其他的agent，作为代理。zabbix database：用于存储所有zabbix的配置信息，监控数据的数据库。zabbix web：zabbix的web界面，通过web界面管理zabbix配置和查看zabbix相关监控信息。
## zabbix监控范围
1. 硬件监控：zabbix IPMI interface
2. 系统监控：zabbix agent interface
3. java监控：zabbix JMX(Java Management Extensions Java管理扩展) interface
4. 网络设备监控：zabbix SNMP interface  
5. 应用服务监控：zabbix agent userparameter
6. mysql数据库监控：percona-monitoring-pldlgins
7. URL监控：zabbix web监控
## zabbix安装方法
1. 编译安装
2. yum安装
## zabbix的工程流程
1. agent安装在被监控客户端，然后获取被监控端数据，发送给server
2. server记录所接收到的数据，存储在database中并按照策略进行相应操作
3. 如果是分布式，server会将数据传送一份到上级server中。
4. web interface将收集到的数据和操作信息显示给用户。
## zabbix的进程
1. 一般情况下，zabbix包含5个进程
2. zabbix_agentd 客户端守护进程。收集客户端数据。
3. zabbix_get zabbix工具，单独使用的命令，通常在server或proxy端执行获取远程客户端信息的命令，通常用于排错。
4. zabbix_sender zabbix工具，用于发送数据给server或proxy。通常用于耗时较长的检测。可以使用sender主动提交数据。
5. zabbix_server zabbix服务端守护进程。 所有数据都主动或被动的提交到server。
6. zabbix_proxy zabbix代理守护进程。功能类似server。一个中转站，把收集到的数据提交/被提交到server里。
7. zabbix_java_gateway zabbix2.0之后引入的功能。Java网关。类似agentd。只用于Java方面。只能主动获取数据，最终传给server或者proxy。
## zabbix监控环境相关术语
1. 主机host：被监控的网络设备。可由IP或DNS名称指定
2. 主机组host group：主机的逻辑容器，包含主机和模板。同一组织内的主机和模板不能互相连接。通常给用户或用户组指派监控权限时使用。
3. 监控项item：特定的监控指标数据。数据来自于被监控对象。是zabbix进行数据收集的核心。每个item都由key（键）标识。key参数如果在<>里面的为可省略参数。否则为必要参数。
4. 触发器trigger：表达式，用于评估某监控对象的特定item内接收到的数据是否在合理范围内。也就是阙值。数据量大于阙值时，触发器状态将从OK转变为Problem。数据恢复到合理范围，又转变为OK。
5. 事件event：触发一个值得关注的事情，比如触发器状态转变。新的agent或重新上线的agent的自动注册等。
6. 动作action：指对于特定事件事先定义的处理方法，如发送通知，何时执行操作。动作可以是分步骤进行的。
7. 报警升级escalation：发送警报或者执行远程命令的自定义方案。如每隔5分钟发送一次警报，共发送5次等。
8. 媒介media：发送通知的手段或者通道，如Email、Jabber、或者SMS等。
9. 通知notification：通过选定的媒介向用户发送的有关某事件的信息。
10. 远程命令remote command：预定义的命令，可在被监控主机处于某特定条件下时自动执行。
11. 模板template：用于快速定义被监控主机的预设条目集合。通常包含了item、trigger、graph、screen、application以及low-level discovery rule；模板可以直接链接至某个主机。
12. 应用application：同一类型一组item的集合
13. web场景web scennario：用于检测web站点可用性的一个或多个HTTP请求。
14. 前端frontend：Zabbix的web接口。
##### 对触发器事件动作监控项和key的理解如下，当一个主机设定一个item监控项根据key值来监控。然后给与其设置相对应的触发条件。当被监控项达到触发条件后，会相应的生成一个事件通过报警媒介通知给给用户，然后用户通过执行动作来解决事件。
## zabbix常用的监控架构模式
1. server-agentd模式：用于监控主机较少的情况下
2. server-proxy-agentd模式：用于较多的机器，使用proxy进行分布式监控，有效的减轻server端的压力。
3. zabbix server端基于C语言，web管理端frontend基于php制作。
## 自动发现与自动注册
1. 自动发现：server主动发现所有客户端。然后将客户端登记在自己的上。缺点：server压力山大(网段大，客户端多)。耗时多。
2. 自动注册：agent主动到server上报道，登记。缺点：配置出错时agent可能找不到server。
3. 两种模式： 两种模式都是在agent上进行配置。
4. 被动模式：默认模式，agent被server抓取数据(都是在agent的立场上说)
5. 主动模式： agent主动将数据发到server端。
## 自动发现流程
1. 完成server端的安装 
2. 完成agent的安装，配置server端IP/或域名信息。保证可互通
3. 在web界面上进行配置 web界面:配置-自动发现-local network 
4. 创建发现动作：配置-动作-Auto discovery. linux server.
5. 配置动作-在条件中添加条件，让添加更准确-在操作中添加。
6. 添加主机与启动主机。等待客户端自动上门就好。
## 自动注册流程
1. 完成server安装
2. 完成agent安装。在agent进行额外配置。
3. 在web页面进行配置 配置-动作-事件源-创建动作-在动作中添加动作。
## 分布式监控与SNMP监控
### 分布式监控
1. 作用：分担压力，减轻负载；多机房监控
2. 需要配置zabbix proxy 和安装数据库。
### SNMP监控
1. 作用：当无法安装agent时，通过snmp监控。
2. SNMP(simple network manager protocol)简单网络管理协议。由一组网络管理的标准组成，包含一个应用层协议，数据库模型，和一组资源对象。
## zabbix的使用
1. zabbix server默认配置文件 /etc/zabbix/zabbix_server.conf
2. zabbix server默认安装目录 /etc/zabbix
3. zabbix agent默认配置文件 /etc/zabbix/zabbix_agent.conf(配置主动发现和自动注册模式)
4. zabbixserver默认端口10051.agent默认端口10050.
5. zabbix默认的报警脚本目录位置 /etc/zabbix/zabbix_server.conf


