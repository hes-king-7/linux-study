# Puppet
## 什么是puppet
1. puppet是一个IT基础设施自动化管理工具。能管理基础设施的整个生命周期：供应(provisioning)、配置(configuration)、联动(orchestration)及报告(reporting)。
2. puppet可实现自动化重复任务、快速部署关键性应用以及在本地或云端完成主动管理变更和快速扩展架构规模等。
3. 遵循GPL协议(2.7.0-)，基于ruby语言开发。2.7.0后使用apache 2.0 license。对于系统管理员是抽象的。只依赖于ruby和facter。能管理多达40多种资源：file、user、host、service、yum repo等。
## puppet工作机制
1. 工作模型：
    puppet 通过声明性、基于模型的方法进行IT自动化管理。
　　定义：通过puppet 的声明性配置语言定义基础设置配置的目标状态；
　　模拟：强制应用改变的配置之前先进行模拟性应用；
　　强制：自动、强制部署达成目标状态，纠正任何偏离的配置；
　　报告：报告当下状态及目标状态的不同，以及达成目标状态所进行的任何强制性改变；
2. puppet三层模型：
   1. 最上层：Configuration Language  中间层：transactional Layer  最底层：Resource Abstraction Layer
3. 工作流程：
   1. Define：定义各种资源清单，使用puppet语言
   2. 模拟：根据资源站点清单模拟部署(测试阶段)
   3. enforce：比对agent端和资源站点清单目标状态是否一致，强制执行(APPLY阶段)
   4. report：agent端通过API接口，将日志发送到Master端。然后回复到define。
4. puppet使用单机模型和master/agent模型。
5. 单机使用模型：实现定义多个manifests-->complier--->catalog--->apply
6. master/agent模型是集中式管理。即agent端周期性向master端发起请求。请求自己需要的数据，然后在自己的机器上运行，并将结果返回给master端。
### puppet名词解释
1. 资源：是puppet的核心，通过资源申报，定义在资源清单中。相当于ansible中的模块
2. 类：一组资源清单
3. 模块：包含多个类。相当于ansible中的角色
4. 站点清单：以主机为核心，应用哪些模块。
### puppet安装和配置文件
1. 使用yum install -y puppet安装
2. 主配置文件为 /etc/puppet/puppet.conf
3. 使用的主程序为/usr/bin/puppet
### puppet资源简介
1. 资源抽象
   1. 相似的资源被抽象成同一种资源类型。如：程序包资源、用户资源、服务资源等。
   2. 将资源属性或状态的描述与其实现方式剥离开来。
   3. 仅描述资源的目标状态。
   4. 这三个也被称为puppet的资源抽象层(RAL) RAL由type(类型)和provider(提供者，即不同OS上的特定实现)组成。
2. 资源定义
   1. 资源定义通过向资源类型的属性赋值来实现。可称为资源类型实例化。定义了资源实例的文件即清单，manifest。
   2. 注意：type必须使用小写字符；title是一个字符串，在同一类型中必须惟一；每一个属性之间需要用“,”隔开，最后一个“,”可省略。
3. 资源属性中的三个特殊属性
   1. Namevar：可简称为name；
   2. ensure：资源的目标状态；
   3. provider：指明资源的管理接口；
### 常用资源总结
1. 查看资源： puppet describe
2. group：管理系统上的用户组
3. user：管理系统上的用户
4. package：puppet的管理软件包
5. service：定义服务的状态
6. file： 管理文件、目录、软链接。
7. exec： 执行命令，通常用来执行外部命令。
8. cron： 定义周期性任务。
9. notify：调试输出
10. 资源的特殊性：puppet中也提供了before、require、notify和subscribe四个参数来定义资源之间的依赖关系和通知关系。
    1. before：表示需要依赖于某个资源
    2. require：表示应该先执行本资源，在执行别的资源
    3. notify：A notify B：B依赖于A，且A发生改变后会通知B；
    4. subscribe：B subscribe A：B依赖于A，且B监控A资源的变化产生的事件；
    5. 依赖关系还可以用->表示后资源需要依赖前资源。~>表示前资源变动通知后资源调用。
### tag标签
1.  如同 anssible 一样，puppet 也可以定义“标签”——tag，打了标签以后，我们在运行资源的时候就可以只运行某个打过标签的部分，而非全部。这样就更方便于我们的操作。一个资源中，可以有一个tag也可以有多个。
2.  tag具体语法：type{'title':
    ...
    tag => ['TAG1','TAG2',...],
}
3. 调用时语法：puppet apply --tags TAG1,TAG2,... FILE.PP
### puppet变量
1. puppet变量以"$" 开头，赋值操作符为"="，语法为 $variable_name=value
2. 数据类型
   1. 字符型：引号可有可无；但单引号为强引用，双引号为弱引用；支持转义符；
   2. 数值型：默认均识别为字符串，仅在数值上下文才以数值对待；
   3. 数组：[]中以逗号分隔元素列表；
   4. 布尔型值：true, false；不能加引号；
   5. hash：{}中以逗号分隔k/v数据列表； 键为字符型，值为任意puppet支持的类型；{ ‘mon’ => ‘Monday’, ‘tue’ => ‘Tuesday’, }；
   6. undef：从未被声明的变量的值类型；
3. 正则表达式：(?<ENABLED OPTION>:<PATTERN>)&(?-<DISABLED OPTION>:<PATTERN>)
   1. options：i：忽略字符大小写；m：把.当换行符；x：忽略<PATTERN>中的空白字符；(?i-mx:PATTERN)
   2. 注意：不能赋值给变量，仅能用在接受=~ 或 ！~操作符的位置。
4. puooet有三种变量：为facts，内建变量和用户自定义变量。
   1. facts：由facter提供；top scope；
   2. 内建变量：
      1. master端变量$servername, $serverip, $serverversion。 
      2. agent端变量$clientcert, $clientversion, $environment。
      3. parser变量$module_name
   3. 用户自定义变量：
5. 变量的作用域：不同变量也有其不同的作用域。称之为Scope。作用域有三种，top scope，node scope，class scope
6. 生效范围：top > node > class.     优先级排序为：top < node < class
### puppet流程控制语句
1. puppet支持if语句，case语句和selector语句。
2. if语句：支持单分支if，双分支if else和多分支if elsif else。其中condition可以是：变量，比较表达式，有返回值的函数
3. case语句：类似if语句，case语句会接受一个控制表达式和一组case代码块，并执行第一个匹配到控制表达式的块。各case的给定方式有五种：直接字串；变量；有返回值的函数；正则表达式的模式；default
4. selector语句：只能用于期望出现直接值(plain value)的地方。包括变量赋值、资源属性、函数参数、资源标题、其它selector。selector不能用于一个已经嵌套于selector的case中。也不能用于一个已经嵌套于case的case语句中。各case的给定方式有如下五种：直接子串；变量；有返回值的函数；正则表达式模式；default。
## class类
### 什么是类
1. 类是puppet中命名的代码模块，常用于定义一组通用目标的资源，可在puppet全局调用。类可以被继承。也可以包含子类。类的名称只能以小写字母开头。
2. 每个类都会引入一个新的变量scope，这意味着在任何收访问类中的变量时，都得使用其完全限定名称。在本地scope可以重新为top scope中的变量赋予一个新值。
3. 类只有被调用才会执行。include后可以跟多个类，直接用","隔开即可。
### 带有参数的类
1. 我们定义的类也可以进行参数设置，可以进行参数的传递。**单个主机上不能被直接声明两次**
### 类的继承
1. 类似于其它编程语言中的类的功能，puppet的Class可以被继承，也可以包含子类。
2. 新增属性：我们在继承父类的时候，可以定义一些父类原本没有的属性：
3. 新增原有值：在继承的类中，我们可以在属性原有值的基础上，使用+>进行新增修改：
4. 修改原有值：在继承的类中，我们可以直接把原有的值进行覆盖修改，使用=>进行覆盖即可。
5. 整体调用父类，并重写部分值：在继承的类中，我们还可以在整体调用的基础上，根据不同的需求，把父类中的部分值进行重写修改
## 模板
1. 模板通常以erb结尾。模板均使用erb语法。puppet兼容的erb语法，可以去https://docs.puppet.com/puppet/latest/reference/lang_template_erb.html官方文档地址查看。
## 模块
### 什么是模块
1. 实践中，一般需要把mainfest文件分解成易于理解的结构，并且通过某种机制在必要时将它们整合起来。这种机制即模块。有助于以结构化、层次化的方式使用puppet。而puppet则基于模块自动装载器。
### 模块的命名规范
模块的目录格式如下：
1. MODULE NAME: 模块名称，模块名只能以小写字母开头，可以包含小写字母、数字和下划线；但不能使用main和settings
2. manifests/: 必须要有
   1. init.pp：必须一个类定义，类名称必须与模块名称相同；
3. files/：静态文件
   1. 每个文件的访问路径遵循：puppet:///modules/MODULE_NAME/FILE_NAME;
4. templates/:
   1. 每个文件的访问路径遵循：tempate('MOD_NAME/TEMPLATE_FILE_NAME');
5. lib/:插件目录，常用于存储自定义的facts以及自定义类型；
6. spec/：类似于tests目录，存储lib/目录下插件的使用帮助和范例
7. tests/：当前模块的使用帮助或使用范例文件。
### master/agent模型
1. 实现步骤：
2. 实现前准备：
   1. 下载包：master端：puppet.noarch, puppet-server.noarch.     agent端；puppet.noarch
   2. 主机名解析： 在/etc/hosts文件实现主机名的解析。
   3. 时间同步
3. 开启master端的puppet服务
   1. 整个过程都是自动完成的，其中，每一步的意思如下：
　　① 创建key 给CA
　　② 创建一个请求给CA
　　③ 自签名证书
　　④ CA 创建完成
　　⑤ 创建证书吊销列表
　　⑥ 为当前的master 主机签署证书
　　⑦ master 的证书签署完成
4. 直接在systemctl开启服务，监听8140端口。
5. 在agent端开启服务
6. 在配置文件中指明server端的主机名： vim /etc/puppet/puppet.conf添加 server = master.keer.com
7. 开启agent服务： puppet agent -v --noop --no-daemonize
8. 在master端签署证书
   1. 注意：某agent证书手工吊销后重新生成一次；
　　On master host：
　　　　puppet cert revoke NODE_NAME
　　　　puppet cert clean NODE_NAME
　　On agent host：
　　　　重新生成的主机系统，直接启动agent；
　　　　变换私钥，建议先清理/var/lib/puppet/ssl/目录下的文件
9. 配置站点清单，且测试agent端是否实现。
10. 给puppet用户授权
11. 直接开启服务，agent会自动去master端获取配置。
