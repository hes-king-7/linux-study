# ansible
## 什么是ansible
1. ansible是新出现的自动化运维工具，基于python开发，集合了众多运维工具的优点。实现了批量系统配置、批量程序部署、批量运行命令等功能。
2. ansible是基于模块工作的，本身没有批量部署能力。真正实现的是ansible运行的模块。ansible只是提供一种框架。主要包括：
3. 连接插件connection plugins：负责和被监控端实现通信；
4. host inventory：指定操作的主机，是一个配置文件里面定义监控的主机；
5. 各种模块核心模块、command模块、自定义模块；
6. 借助于插件完成记录日志邮件等功能；
7. playbook：剧本执行多个任务时，非必需可以让节点一次性运行多个任务。
### ansible主要部分
1. Control Node：控制机器
2. Inventory：主机清单，配置管理主机列表
3. Playbooks：剧本、任务编排。根据规则定义多个任务，模块组织结构清晰，由ansible自动执行。
4. Modules（Core | Custom）：模块，用于执行某个具体的任务
5. connection plugin（连接插件）：Ansible通过不同的协议连接到远程主机上，执行指定的命令。默认采用ssh协议连接远程主机。
### ansible特性
1. no agents：不需要在被管控主机上安装任何客户端；
2. no server：无服务器端，使用时直接运行命令即可；
3. modules in any languages：基于模块工作，可使用任意语言开发模块；
4. yaml，not code：使用yaml语言定制剧本playbook；
5. ssh by default：基于SSH工作；
6. strong multi-tier solution：可实现多级指挥。
### ansible优点
1. 轻量级，无需在客户端安装agent，更新时，只需在操作机上进行一次更新即可；
2. 批量任务执行可以写成脚本，而且不用分发到远程就可以执行；
3. 使用python编写，维护更简单，ruby语法过于复杂；
4. 支持sudo。
### ansible任务执行流程
1. Ansible在运行时，首先读取ansible.cfg中的配置，根据规则获取Inventory中的管理主机列表，并行的在这些主机中执行配置的任务，最后等待执行返回的结果。
### ansible安装方法
ansible安装常用两种方式，yum安装和pip程序安装。一般使用yum安装。
### ansible默认安装目录
配置文件目录：/etc/ansible/    配置文件：/etc/ansible/ansible.cfg
执行文件目录：/usr/bin/
Lib库依赖目录：/usr/lib/pythonX.X/site-packages/ansible/
Help文档目录：/usr/share/doc/ansible-X.X.X/
Man文档目录：/usr/share/man/man1/
### ansible命令的具体格式如下
ansible <host-pattern> [-f forks] [-m module_name] [-a args]
### ansible常用模块
1. 主机连通性测试： 使用ansible web -m ping命令来进行主机连通性测试
2. command模块：可以直接在远程主机上执行命令，并将结果返回本主机如：ansible web -m command -a 'ss -ntl'
3. shell模块：可以在远程主机上调用shell解释器运行命令。例如管道：ansible web -m shell -a 'cat /etc/passwd |grep "keer"'
4. copy模块：用于将文件复制到远程主机，同时支持给定内容生成文件和修改权限等。如：ansible web -m copy -a 'src=~/hello dest=/data/hello' 
5. file模块：该模块主要用于设置文件的属性，比如创建文件、创建链接文件、删除文件等。如创建链接文件：ansible web -m file -a 'path=/data/bbb.jpg src=aaa.jpg state=link'
6. fetch模块：该模块用于从远程某主机获取（复制）文件到本地。
   1. dest：用来存放文件的目录。
   2. src：在远程拉取的文件，并且必须是一个file，不能是目录
7. cron模块：该模块适用于管理cron计划任务的。其使用的语法跟我们的crontab文件中的语法一致。
8. yum模块：顾名思义，该模块主要用于软件的安装。
9. service模块：该模块用于服务程序的管理。
10. user模块：该模块主要是用来管理用户账号。
11. group模块：该模块主要用于添加或删除组。
12. script模块：该模块用于将本机的脚本在被管理端的机器上运行。该模块直接指定脚本的路径即可。
13. setup模块：该模块主要用于收集信息，是通过调用facts组件来实现的。facts组件是Ansible用于采集被管机器设备信息的一个功能。facts就是内建变量。用来存储主机信息等。
### Ansible playbook简介
1. playbook是ansible用于配置，部署，和管理被控节点的剧本。
2. 通过playbook的详细描述，执行其中的一系列tasks，可以让远端主机达到预期的状态。playbook就像Ansible控制器给被控节点列出的一系列to-do-list。而被控节点必须要完成。
3. Ansible playbook使用场景：执行一些简单的任务。使用ab-hoc命令可以方便的解决问题。但是需要大量的操作执行的时候。这时最好的方法就是使用playbook。
4. playbook就行执行shell命令与写shell脚本一样。也可以理解为批处理任务。不过Playbook有自己的语法格式。方便重用代码，方便移植到不同的及其上面，最大化利用代码。
### playbook书写格式。
1. playbook由YMAL语言编写。yaml由Clark Evans在2001年5月在首次发表了这种语言。另外Ingy döt Net与OrenBen-Kiki也是这语言的共同设计者。
2. YMAL格式是类似于JSON的文件格式。以下为playbook常用到的ymal格式：
   1. 在文件的第一行应用以"---"(三个连字符)开始，表明ymal文件的开始。
   2. 在同一行中，#之后的内容表示注释，类似于shell。
   3. YMAL中的列表元素以“ -”开头然后紧跟一个空格，后面为元素内容。
   4. 同一个列表中的元素应该保持相同的缩进，否则会被当做错误处理。
   5. play中的hosts，variables，roles。tasks等对象的表示方法都是键值中间以":"分隔表示，后面还要增加一个空格。
   6. 文件名称应该以.yml或.yaml结尾。
   7. 使用ansible-playbook运行playbook文件，输出内容为JSON格式。并且由不同颜色组成。一般而言
      1. 绿色代表执行成功，系统保持原样
      2. 黄色代表系统状态发生改变
      3. 红色代表执行失败，显示错误输出。
   8. 执行有三个步骤：1.收集facts、2执行tasks、3报告结果。
### playbook核心元素
1. Hosts：主机组
2. Tasks：任务列表
3. Variables：变量、设置方式有四种
4. Templates：包含了模板语法的文本文件
5. Handlers： 处理器。由特定条件触发的任务
### 基本组件
playbook配置文件的基础组件：
1. Hosts：运行指定任务的目标主机
2. remoute_user：在远程主机上执行任务的用户
3. sudo_user：
4. tasks：任务列表
5. 模块，模块参数：
6. handlers：任务，在特定条件下触发；接收到其它任务的通知时被触发。
   1. 某任务的状态在运行后为changed时，可通过"notify"通知给相应的handlers；
   2. 任务可以通过"tags"打标签，而后可在ansible-playbook命令上使用-t指定进行调用。
7. 一般步骤：
   1. 定义playbook  ：vim xx.yaml
   2. 测试运行结果  ： ansible-playbook xx.yaml
   3. 测试标签： 测试标签需要先把服务关闭后，再运行剧本(playbook)并引用标签：  ansible-playbook xx.yaml -t tag名。
   4. 测试notify： 如果有的话。重新跑剧本测试。
### variables部分
variables是变量，有四种定义方法
1. facts：可直接调用。具体的facters我们可以使用setup模块来获取。然后直接放入剧本中调用即可。
2. 用户自定义变量： 
   1. 通过命令行传入变量。ansible-playbook命令的命令行中的-e VARS, --extra-vars=VARS，这样就可以直接把自定义的变量传入
   2. 在playbook中定义变量。 格式如下。
      1. vars： 
         1. -var1：value1  
            1.  --var2：value2
3. 通过roles传递变量
4. Host Inventory：我们也可以直接在主机清单中定义。方法如下
   1. 向不同的主机传递不同的变量：IP/HOSTNAME varaiable=value var2=value2
   2. 向组中的主机传递相同的变量：[groupname:vars]  下一级：variable=value
### 模板 templates
模板是一个文本文件，嵌套有脚本(使用模板编程语言编写)
    jinja2：是python的一种模板语言，以Django的模板语言为原本。
模板支持：
1. 字符串：使用单引号或双引号；
2. 数字：整数，浮点数；
3. 列表：[item1, item2, ...]
4. 元组：(item1, item2, ...)
5. 字典：{key1:value1, key2:value2, ...}
6. 布尔型：true/false
7. 算术运算：+, -, *, /, //, %, **
8. 比较操作：==, !=, >, >=, <, <=
9. 逻辑运算：and, or, not
**通常来说，模板都是通过引用变量来运用的**
### 条件测试
1. when语句：在task中使用，jinja2的语法格式
2. 循环：迭代，需要重复执行的任务：对迭代项的引用，固定变量名为item。要在task中使用with_items给定要迭代的元素列表。
### 字典
1. 一个字典是由一个简单的键：值的形式组成。字典也可以使用缩进形式来表示。
### 角色订制：roles
1. roles用于层次性，结构化地组织playbook。roles能够根据层次型结构自动装载变量文件、tasks以及handlers等。相当于角色集合。
2. 要使用roles只需要在playbook中使用include指令即可。
3. 角色集合： roles/
4. mysql/
5. httpd/
6. nginx/
7. files/   存储由copy或script等模块调用的文件；
8. tasks/   此目录中至少应该有一个名为main.yml的文件，用于定义各task；其它的文件需要由main.yml进行“包含”调用；
9. handlers/    此目录中至少应该有一个名为main.yml的文件，用于定义各handler；其它的文件需要由main.yml进行“包含”调用；
10. vars/   此目录中至少应该有一个名为main.yml的文件，用于定义各variable；其它的文件需要由main.yml进行“包含”调用；
11. templates/  存储由template模块调用的模板文本；
12. meta/   此目录中至少应该有一个名为main.yml的文件，定义当前角色的特殊设定及其依赖关系；其它的文件需要由main.yml进行“包含”调用；  
13. default/    此目录中至少应该有一个名为main.yml的文件，用于设定默认变量；
14. 