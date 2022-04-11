# netfilter/iptables
## netfilter/iptables简介
1. netfilter是linux内置的一种防火墙机制，也叫数据包过滤机制。netfilter组件也称为内核空间(kernelspace)是内核的一部分。
2. iptables则是一个命令行工具，用来内置netfilter防火墙。iptables组件也成为用户空间(userspace)。
3. netfilter在协议栈中添加一些钩子(hooks)，运行内核模块通过钩子注册回调函数。给数据包打标记或丢弃。
4. netfilter框架负责维护钩子上注册的处理函数或模块和优先级。
5. iptables是运行在和用户态的一个程序。通过netlink和内核的netfilter框架打交道，并负责往钩子上配置回调函数。
## netfilter防火墙原理
1. netfilter机制对进出主机的数据包通过按一定顺序匹配iptables规则进行过滤。匹配到接收accept数据包进入主机获取资源，否则予以丢弃drop。
2. netfilter主要采用连线跟踪（Connection Tracking）、 包过滤 （Packet Filtering）、 地址转换 、包处理（Packet Mangling)4种关键技术
### iptables中的table与chain
1. iptables结构：iptables->tabels->chains->rules。
1. iptables用表table来分类管理它的规则rule。这也是iptables名称的由来。根据rule的作用分成了几个表。
   1. filter表：过滤数据包(默认有三种内建链)
      1. INPUT链 – 处理来自外部的数据。
      2. OUTPUT链 – 处理向外发送的数据。
      3. FORWARD链 – 将数据转发到本机的其他网卡设备上
   2. nat表：处理地址转换(默认三种内建链)
      1. PREROUTING链 – 处理刚到达本机并在路由转发前的数据包。它会转换数据包中的目标IP地址（destination ip address），通常用于DNAT(destination NAT)。
      2. POSTROUTING链 – 处理即将离开本机的数据包。它会转换数据包中的源IP地址（source ip address），通常用于SNAT（source NAT）。
      3. OUTPUT链 – 处理本机产生的数据包。
   3. mangle表：包重构(修改)用于指定如何处理数据包。它能改变TCP头中的QoS位。内有5种内建链
      1. PREROUTING
      2. OUTPUT
      3. FORWARD
      4. INPUT
      5. POSTROUTING
   4. raw表：数据跟踪处理。内具两种内建链
      1. PREROUTING chain
      2. OUTPUT chain
   5. Security表： 里面的rule主要跟selinux有关。只要在数据包上设置一些selinux的标记。便于跟selinux相关的模块来处理该数据包。
2. rule规则表由chain链来决定什么时候触发chain上的这些规则
3. iptables5个内置的chain
   1. prerouting：接收的数据包刚进来，还没有经过路由选择，触发该chain上的规则
   2. input：已经过路由选择，并该数据包的目的IP是本机。进入本地数据包处理流程，触发该chain规则。
   3. forward：已经过路由选择，但该数据包的目的IP标识本机，进入forward流程。触发该chain上的规则
   4. output：本地程序要发出去的数据包刚到IP层，还未进行路由选择，触发该chain规则。
   5. postrouting： 本地程序发出去的数据包，或者抓发(forward)的数据包已经经过了路由选择，将交由下层发送出去。此时会触发该chain上的规则。
4. iptables中的规则rules
5. 规则(rules)存放在特定表的特定 chain 上，每条 rule 包含下面两部分信息：
   1. Matching
    Matching 就是如何匹配一个数据包，匹配条件很多，比如协议类型、源/目的IP、源/目的端口、in/out接口、包头里面的数据以及连接状态等，这些条件可以任意组合从而实现复杂情况下的匹配。
   2. Targets 动作选项
        Targets 就是找到匹配的数据包之后怎么办，常见的有下面几种：
        DROP：直接将数据包丢弃，不再进行后续的处理
        REDIRECT：与DROP基本一样，区别在于它除了阻塞包之外， 还向发送者返回错误信息。
        RETURN： 跳出当前 chain，该 chain 里后续的 rule 不再执行
        QUEUE： 将数据包放入用户空间的队列，供用户空间的程序处理
        ACCEPT： 同意数据包通过，继续执行后续的 rule
        SNAT：源地址转换，即改变数据包的源地址
        DNAT：目标地址转换，即改变数据包的目的地址
        MASQUERADE： IP伪装，即是常说的NAT技术，MASQUERADE只能用于ADSL等拨号上网的IP伪装，也就是主机的IP是由ISP分配动态的；如果主机的IP地址是静态固定的，就要使用SNAT
        LOG：日志功能，将符合规则的数据包的相关信息记录在日志中，以便管理员的分析和排错
6. iptables命令选项输入顺序：
   1. iptables -t 表名 <-A/I/D/R> 规则链名 [规则号] <-i/o 网卡名> -p 协议名 <-s 源IP/源子网> --sport 源端口 <-d 目标IP/目标子网> --dport 目标端口 -j 动作
7. IPtables默认配置文件
8. 通过iptables命令添加的规则不会永久保存，只存在内存，重启失效。如果需要永久保存规则，则需要将规则保存在 /etc/sysconfig/iptables 配置文件中
    通过以下命令将iptables规则写入配置文件
    service iptables save 或者 iptables-save > /etc/sysconfig/iptables
    保存现有的规则到指定文件，主要是配合iptables-restore命令，iptables-restore命令用来还原iptables-save命令所备份的iptables配置。
    iptables-save > iptables.bak
    iptables-restor < iptables.bak