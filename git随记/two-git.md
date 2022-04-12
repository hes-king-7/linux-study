<div align='center' ><font size='70'>第二章 
gitlab介绍</font></div>
## github & GitLab简介
1. Github 是一项公开的面向开源以及私有软件项目的托管平台，相当于一个公有云仓库。只支持git作为唯一的版本库格式进行托管
2. gitlab 是一个基于git实现的在线代码仓库软件，可以使用gitlab自己搭建一个类似于github一样的私有云仓库。
3. github和gitlab都是基于文本的git远程仓库，它们都提供了分享开源项目的平台。gitlab更倾向与私有库，github为公有云仓库。
4. gitlab登录方式
   1. ssh(通过gitlab shell方式)以22端口远程登陆
   2. http/https(通过nginxweb代理服务器)以80/443端口web页面登录。
5. gitlab默认用户权限分配
   1. guest用户。可以创建issue。相当于普通用户
   2. reporter用户。可以克隆代码，默认不能提交，需要授权
   3. developer用户。可以克隆代码、开发、提交push等操作
   4. master用户。可以创建项目、添加tag、保护分支、添加项目成员
   5. owner用户。可以设置项目权限，相当于项目组老大
6. gitlab中组和项目的三种访问权限
   1. private:私立专属权限。只有此项目中的组成员可以看到和修改
   2. internal:开源的权限。只要登录的用户就能看到
   3. Public:公开权限。所有人都能看到。
7. gitlab升级/降级
   1. gitlab允许小版本直接升级，大版本需要通过相近的小版本阶段升级。
   2. 版本回退的操作和升级操作相同。
   3. 升降级过程中
   4. 特别注意事项：无论做版本升级或者回退步骤之前，先对现版本进行备份操作。以防止升级或回退失败带来不可逆操作。
## gitlab CI/CD
1. gitlab内置持续集成功能
2. Continuous Integration(CI) 持续集成 通过脚本运行来构建、测试和验证代码更改，将代码块合并到主分支master。
3. Continuous Delivery(CD) 持续交付
4. Continuous Deployment(CD)持续部署 
   1. 持续交付和部署相当于更进一步的CI，可以在开发周期的早期发现bugs和errors。提早修改
5. Gitlab是一个web应用程序。一个gitlab程序至少包含三分部
   1. 一个gitlab实例 
   2. 一个gitlab Runner  用来执行gitlab-ci.yml配置文件。
   3. 一个gitlab-ci文件 gitlab-ci.yml文件用来配置Gitlab CI/CD，在项目根目录创建。
6. Gitlab Runner
   1. Gitlab Runner是配合Gitlab-ci进行使用的。一个执行集成脚本的东西
   2. Runner可以分布在不同的主机或容器内，同一个主机上也可以有多个Runner
   3. Runner的类型
      1. 共享Runner(Shared Runner)，所有项目可以使用
      2. 群组Runner(Group Runner)，特定群组里的所有项目和子群组可以使用
      3. 特定Runner(Specific Runner)，用于独立的项目。
   4. 向Gitlab-CI注册一个Runner需要两样东西：Gitlab-CI的url和注册token
   5. toker是为了确定你这个Runner是所有工程都能够使用的Shared Runner还是具体某一个工程才能使用的Specific Runner
   6. 使用Runner需要使用系统增加的gitlab-runner账户，并为其配置用户权限和需要操作目录的权限。
## pipline
1. 什么是pipline，是一套插件，支持实现和集成连续交付的管道构建软件。
2. Job ：作业定义
3. stages：定义作业可以使用的阶段。全局模式定义。一般有三个阶段
   1. build-test-deploy
4. variables：定义变量
   1. pipeline变量、job变量(优先级最大)、RUnner变量
## Git LFS
1. Git大文件存储。简称LFS。目的是更好的把大型二进制文件集成到git的工作流中。处理方式用文本指针替换。
## 分支管理
1. git有很多中工作流程。包括Centralized Workflow、Feature Branch Workflow、Gitflow Workflow、Forking Workflow等
2. Git Flow工作流程围绕项目发布定义了严格的分支模型。为不同的分支分配了非常明确的角色，并且定义了使用场景和用法。
3. 分支类型
   1. master分支：存放所有正式发布的版本。不直接提交代码
   2. develop分支：主开发分支，一般不直接提交代码
   3. feature分支：新功能分支，基于develop分支创建，开发完成后合并到develop分支上，可以同时存在多个。
   4. release分支：基于最新develop分支创建，测试新功能分支，所有的测试bug在这个分支该，测试完成后合并到master并打上版本号，同时也合并到develop分支，更新最新开发分支。同一时间只有1个，生命周期很短，只是为了发布。
   5. hotfix分支，基于master分支创建，对线上版本的bug进行修复，完成后直接合并到master分支和develop分支，如果当前还有release分支，也同步到release分支。同一时间只有一个，生命周期较短。
## git提交内容规范
1. git每次提交代码，都要写Commit message(提交说明)。
2. 规范设置为："type （scope）:subject"
   1. type用于说明commit的类别标识
   2. subject是commit类型的简短描述。
3. 设置分支保护让只有授权的用户才可以向受保护的分支推送代码。(master分支一般设置为分支保护)
## PlantUML
1. UML(统一建模语言)用于对软件进行描述、可视化处理等。
2. UML三种模型
   1. 功能模型：从用户的角度展示系统的功能，包括：用例图
   2. 对象模型：采用对象，属性，操作，关联等概念展示系统的结构和基础。包括类图、对象图。
   3. 动态模型：展现系统的内部行为。包括：序列图、活动图、状态图。
3. UML常用工具
   1. IBM Rational Rose、StarUML、EA、precesson(在线协作绘图平台)、Plant UML、planttext(在线绘图)、gravizo。
4. 类图：是软件工程的统一建模语言一种静态结构图。
5. 类图是UML的灵魂。要学会给类图分组，分包。分组规则跟三层架构一一对应。纯三层：UI、BLL、DAL层这三层再加一个Entity实体层。
   1. UI层：用户界面对应的类
   2. Entity层：实体层里面的类跟数据库里面的表是一一对应的。
   3. DAL层：用来根数据库打交道。对数据简单的增删改查。
   4. BLL层：上面三个层之间的一个桥梁，负责它们之间的数据交换。
6. 在UML类图中，类使用包含类名，属性，方法名及其参数并用分割线分割的长方形表示。
7. 类之间的关系有以下几种
   1. 泛化(Generalization)又叫继承，表示一个对象是另外一个对象的意思
   2. 实现(Realization)在UML中用与类的表示法类似的方式表示接口
   3. 依赖(Dependency)最弱的关系，一个类使用另一个类
   4. 关联(Association)描述不同类的对象之间的结构关系
   5. 聚合(Aggregation)关联关系的一种特例，表示整体与部分拥有的关系
   6. 组合(Composition)关联关系的一种特列，一种强烈的包含关系。
8. 其他类型还有
   1. 时序图
   2. 用例图
   3. 活动图
   4. 组件图
   5. 状态图
9.  PlantUML是一个开源项目。支持快速绘图等功能。是一个快随创建UML图形的组件。
10. PlantUML支持的图形有：
sequence diagram,
use case diagram,
class diagram,
activity diagram (here is the new syntax),
component diagram,
state diagram,
object diagram,
wireframe graphical interface