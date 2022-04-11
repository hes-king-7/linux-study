## tomcat的由来
1. tomcat最初由Sun的软件架构师詹姆斯·邓肯·戴维森开发。后来变成开源项目。并贡献给了apache软件基金会。
## tomcat是什么
1. tomcat是一个开源的轻量级web应用服务器。在并发量小的场合下被普遍使用，是开发和调试Servlet、JSP程序的首选。
2. tomcat主要组件有：服务器server，服务service，连接器connector，容器container。其中连接器和容器是tomcat的核心。
3. 一个容器和一个或多个连接器组合在一起，加上其他一些支持组件共同组成一个Service服务。service对外提供能力。Server组件为Service服务的正常使用提供生存环境，Server组件可以同时管理一个或多个Service服务。
4. Connector连接器：用来在某个指定的端口上侦听客户请。connector最重要的功能就是接收连接请求然后分配线程让container容器来处理。所以这必然是多线程的。
5. tomcat有两个经典的connector。http/1.1在端口8080出侦听来自客户browser的http请求。AJP/1.3在端口8009处侦听其他web server的servlet/JSP请求。
6. container是容器的父接口，由四个子容器组件构成
   1. Engine容器：最上层容器，只定义一些基本关联关系。
   2. host容器：Engine的子容器。一个host容器相当于一个Engine里面的一个虚拟机。主要作用运行多个应用。负责安装和展开以及标识这个应用。还有关联子容器和保存一个主机应有的信息。
   3. context容器：代表servlet，具备了servlet运行的基本环境。简单的tomcat可以没有engine和host。主要用于管理servlet实例。tomcat5以后通过request来实现对servlet实例的执行。
   4. wrapper容器：wrapper代表一个servlet，负责管理一个servlet的装载、初始化、执行以及资源回收。wrapper是最底层的容器。
## tomcat其它组件
1. 安全组件、日志组件、session、mbeans、naming等其它组件。这些组件共同为Connector和container提供必要的服务。
## tomcat重要目录
. /bin -Tomcat 脚本存放目录(如启动、关闭脚本)。*.sh文件用于unix系统；*.bat文件用于windows系统。
. /conf -Tomcat配置文件目录。
. /logs -Tomcat默认日志文件。
. /webapps -webapp运行的目录。一般web项目路径结构都在webapp目录下。
重点：安装tomcat之前一定要先安装相应的系统环境配置变量和相关依赖插件。

### tomcat的默认端口号

```css
tomcat的默认端口号是:8080.weblogic的默认端口号是:7001.
```



## tomcat启动方式
1. 嵌入式： API方式。在pom.xml中添加依赖。
2. 使用maven插件启动(不推荐)。启动简单，但是插件久不更新。在pom.xml中引入插件。
3. IDE插件：使用常见Jave IDE插件启动。
