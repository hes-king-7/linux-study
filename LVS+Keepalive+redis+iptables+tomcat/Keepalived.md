## Keepalived：起初主要服务于LVS
1. keepalived:是集群管理中保证集群高可用的一个服务软件，功能类似于heartbeat。用来防止单点故障。是一个类似于layer3，4 & 5交换机制的软件。
2. keepalived三个核心模块
   1. core核心模块：负责主进程的启动、维护、以及全局配置文件的加载和解析。
   2. chech健康监测：负责健康检查，包括常见的各种检查方式。
   3. vrrp虚拟路由冗余协议：实现VRRP协议。
3. keepalived服务三个重要功能：
   1. 管理LVS
   2. 对LVS集群节点检查
   3. 作为系统网络服务的高可用功能。
4. Keepalived工作原理
   1. keep是以VRRP协议为实现基础。VRRP全程Virtual Router Redundancy Protocol，即虚拟路由冗余协议。
   2. 虚拟路由冗余协议，将N台相同功能路由器组成一个组，组里有一个master和多个backup。master上有一个对外提供服务的VIP。master发送组播，当backup收不到vrrp包时任务master宕机。然后根据VRRP优先级来选举一个backup当master。保证路由的高可用性。
5. keepalived配置模式
   1. 抢占式配置：master和backup主从之分
   2. 非抢占式配置：没有主从之分，全部都为backup模式。配置文件中添加nopreempt。用来标识为非抢占式。
6. keepalived脑裂现象：由于某些原因，导致两台keepalived高可用服务器在指定时间无法检测到对方存活心跳信息，从而导致相互抢占对方资源和服务所有权。
7. 可能出现的原因：
   1. 服务器网线松动等网络故障
   2. 服务器硬件故障发生损坏现象而崩溃；
   3. 主备都开启了firewalld防火墙
   4. nginx宕机，导致用户请求失败，但keepalived不会进行切换。（解决方法：编写一个检测nginx的存活状态脚本。如果nginx不存活，则kill掉宕掉的nginx主机上年的keepalived）所有的keepalived都要配置。
8. keepalived服务器状态检测
   1. 在layer3上：keepalived定期向服务器集群的服务器发送一个ICMP(ping)包。如果发现某台服务IP未激活。keepalived便报告这台服务器失效，并将它从服务器中剔除。如某台服务器被非法关机。
   2. 在layer4上：主要以TCP端口状态来决定服务器工作正常与否。如果未检测到服务端口，则keepalived将把这台服务器从服务器群中剔除。
   3. 在layer5上：体现在应用层，keepalived将根据用户的设定检查服务器程序的运行是否正常，如果与用户的设定不相符，则将服务器从集群中剔除。
9. keep默认配置文件存放位置：/etc/keepalived/keepalived.conf
10. keep默认使用端口112进行通讯。