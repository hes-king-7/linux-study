# gitlab安装使用

## gitlab简介

### 1.  gitlab介绍

#### 1. Gitlab是一种可以搭建类似于github服务平台的管理工具。更倾向于私有云。供企业私人内部管理代码

### 2.  gitlab安装

1. 实验环境准备：

   1. linux centos7.9虚拟机一台
   2. 安装配置好docker 环境
   3. 虚拟机可以正常连接外网(如不能连网需到官网手动下载好gitlab及gitlab-runner相关镜像包)

2. 安装docker-compose(非必须)

   ```css
    curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ### 下载安装docker-compose1.29.2版本
   [root@www ~]# chmod +x /usr/local/bin/docker-compose	##加权限
   [root@www ~]# docker-compose --version					##查看版本信息
   docker-compose version 1.29.2, build 5becea4c
   ```

3. gitlab安装步骤：**在相关环境配置完成的情况下。具体相关环境配置可自行百度**

   1. 使用docker search gitlab 命令，查找docker仓库源中包含的gitlab相关镜像包。

   2. 使用docker pull +镜像包名称:tags 下载镜像包。不指定版本默认下载latest(最新版本)

   3. 使用docker image ls查看下载好的镜像包。

   4. mkdir -p /home/gitlab/config   创建config挂载目录
      mkdir -p /home/gitlab/logs    创建logs挂载目录
      mkdir -p /home/gitlab/data    创建data挂载目录

   5. 使用 docker run --detach \               #指定容器运行于前台还是后台。detach是后台

      --hostname IP或域名 \               		#指定IP/域名(主机名)

      --publish 7001:443 --publish 7002:80 --publish 7003:22 \    

      #指定容器暴露的端口,左边的端口代表宿主机的端口，右边的是代表容器的端口

      --name gitlab \									#给容器起一个名字叫gitlab

      --restart always \								#总是重启

      --volume /home/gitlab/config/:/etc/gitlab \		#数据卷挂载。左边宿主机。右边容器内

      --volume /home/gitlab/logs/:/var/log/gitlab \

      --volume /home/gitlab/data/:/var/opt/gitlab 	46cd6954564a(镜像包ID值)

   6. 启动容器后，在宿主机内cd  /home/gitlab/config/      #切换到容器对应挂载宿主机的配置相关目录，修改config目录下的gitlab.rb配置文件。**提醒。修改配置前可先cp gitlab.rb gitlab.rb.bak。为配置文件做个cp备份。放在修改出错不可逆** 。使用vim gitlab.rb命令进入配置文件后，找到以下位置。进行相关配置修改

   7. external_url = 'ip/主机名' **推荐使用IP**。

      gitlab_rails['gitlab_shell_ssh_port'] = 7003	#修改容器ssl端口为映射的宿主机端口。后保存配置。

   8. 然后重新进入gitlab容器                           docker exec -it gitlab /bin/bash

   9. 在容器内重置gitlab客户端                        gitlab-ctl reconfigure

   10. 重置完成后退出容器                                  exit

   11. 然后在容器宿主机上重启gitlab容器         docker restart gitlab   

   12. 重启后可以通过docker ps命令查看容器是否已启动。通过netstat -ntpl命令查看容器相关端口是否正常被监听。配置完成后。既可在真实主机下(推荐使用)google浏览器输入docker ip+映射端口/域名。进行gitlab页面访问选项。

       ![](gitlab实验文档.assets/访问登录页面1.jpg)

   13. 出现此页面即说明gitlab容器启动成功。**(开始登录时页面报错502.经百度查询告知原因可能有两种，一、宿主机内存给的不够(默认建议给2-4G或以上)。二、端口被占用。也就是说你容器端口或映射端口有被其他服务占用。导致端口冲突。这个时候我们就可以使用netstat -ntpl查看是否有端口被其他服务占用，如果被占用，可以kill掉其它服务。或者重新配置容器端口。我这里的错误原因后经查证是因为容器刚起来，还在缓读内存才导致的。这个时候多等一会再从新刷新网页即可。）**

   14. 页面出来后可以输入默认的用户名root和密码(**root默认密码存储位置在/home/gitlab/config/initial_root_password内**。使用cat命令查看即可。此文件将在首次执行reconfigure后24小时内自动删除。所以我们使用此密码首次登录后建议先进web页面给root重新配置密码。)

   15. 拿到密码后在web页面输入用户名和密码登录进入。

       ![](gitlab实验文档.assets/使用root登录3.jpg)

   16. 进入后首页如下。后找到修改密码界面对root用户密码进行修改操作。

       ![](gitlab实验文档.assets/root登陆后首页4.jpg)

       ![](gitlab实验文档.assets/root密码修改5.jpg)

       ![](gitlab实验文档.assets/root密码修改6.jpg)

       ![](gitlab实验文档.assets/root密码修改7.jpg)

   17. 修改完后，点击下面的保存配置即可，其它信息可暂时不改。修改完后会重新跳回登录界面。这时候我们使用root+修改后的密码登录即可。到此。一个简单的gitlab容器实例搭建完成。

### 3.  gitlab使用配置

​			gitlab搭建完成后。我们可以对其进行功能操作实验。

#### 3.1 SSH公钥部署

1. 在本机打开git命令行，输入 ssh-keygen -t rsa -C "bv_test"。生成一对ssh密钥。其中id_rsa是本地保存私钥。id_rsa.pub是在gitlab添加的公钥。复制信息粘贴到gitlab上即可。

   ![](gitlab实验文档.assets/gitssh部署8.jpg)

   ![](gitlab实验文档.assets/gitlabssh公钥部署9.jpg)

   ![](gitlab实验文档.assets/gitlabssh公钥部署10-16500297022481.jpg)

   ![](gitlab实验文档.assets/gitlabssh公钥部署11.jpg)

   ![](gitlab实验文档.assets/gitlabssh密钥部署9.jpg)

   ![](gitlab实验文档.assets/gitlabssh公钥部署10.jpg)

   ![](gitlab实验文档.assets/gitlabssh公钥部署完成11.jpg)

   一个gitlab/github上可以添加多个ssh-key。但是一个key只能对应一个主机。

   ![](gitlab实验文档.assets/github上添加ssh-key15.jpg)

   ![](gitlab实验文档.assets/gitlab上添加ssh-key16.jpg)

   - github账户中的SSH keys，相当于这个账号的最高级key，只要是这个账号有的权限（任何项目），都能进行操作。

   - 仓库的Deploy keys，就是这个仓库专有的key，用这个key，只能操作这个项目，其他项目没有权限。

   - 报错信息：此信息提示SSH KEY已被注册。意即一个key只可用于一个SSH或deploy keys

   - #### The form contains the following error:

     - Fingerprint has already been taken

   到此。ssh-key已配置完成。后面本机就可以免用户密码直接和gitlab/github上传下载代码了

### 4.  功能点测试

#### 4.1 gitlab数据备份测试

** 测试数据备份操作。(docker容器环境下)

1. 首先进入自己的gitlab容器内。 docker exec -it gitlab /bin/bash

2. 查找到gitlab默认存储数据目录。/var/opt/gitlab/git-data/

3. 停止gitlab服务数据

   ```
   root@www:/var/opt/gitlab/git-data# gitlab-ctl status			##查看状态
   root@www:/var/opt/gitlab/git-data# gitlab-ctl stop unicorn		##停止un(新版本已被puma替代。使用status命令可查看是否有puma服务。若有，则说明已使用puma)
   root@www:/var/opt/gitlab/git-data# gitlab-ctl stop sidekiq		##停止队列
   ok: down: sidekiq: 0s, normally up
   root@www:/var/opt/gitlab/git-data# gitlab-ctl stop puma			##停止puma(网页登录)
   ok: down: puma: 1s, normally up
   ```

4. 停止后网页报错502.等后续重启即可。

   ```
   root@www:/var/opt/gitlab/backups# gitlab-rake gitlab:backup:create
   ## 手动备份命令
   root@www:/var/opt/gitlab/backups# ll
   total 344
   drwx------  2 git  root     60 Apr 18 04:49 ./
   drwxr-xr-x 20 root root   4096 Apr 18 03:48 ../
   -rw-------  1 git  git  348160 Apr 18 04:49 1650257368_2022_04_18_14.6.1_gitlab_backup.tar		##备份生成的文件。
   root@www:/var/opt/gitlab/backups# gitlab-ctl start	##启动gitlab服务
   ```

5. 删除数据库

   ![](gitlab实验文档.assets/gitlab删除数据模拟17.jpg)

   ![](gitlab实验文档.assets/删除库18.jpg)

#### 4.2 数据恢复

```css
root@www:/var/opt/gitlab/backups# gitlab-ctl stop puma		
ok: down: puma: 1s, normally up
root@www:/var/opt/gitlab/backups# gitlab-ctl stop sidekiq
ok: down: sidekiq: 0s, normally up
```

![](gitlab实验文档.assets/恢复数据19.jpg)

指定恢复那个备份数据。恢复期间要输入两次yes。

![](gitlab实验文档.assets/恢复数据20.jpg)

![](gitlab实验文档.assets/恢复数据21.jpg)

恢复之后重启gitlab然后进网页查看是否恢复。

```css
root@www:/var/opt/gitlab/backups# gitlab-ctl start sidekip
root@www:/var/opt/gitlab/backups# gitlab-ctl start puma
ok: run: puma: (pid 6536) 0s
```

![](gitlab实验文档.assets/恢复数据22.jpg)

#### 4.3 测试新建用户，组和项目

1. 新建用户

   ![](gitlab实验文档.assets/新建用户和组23.jpg)

   ![](gitlab实验文档.assets/新建用户24.jpg)

2. 新建组

   ![](gitlab实验文档.assets/新建组25.jpg)

   ![](gitlab实验文档.assets/组中添加用户26.jpg)

3. 新建项目

   ![](gitlab实验文档.assets/新建项目27.jpg)

   ![](gitlab实验文档.assets/新建项目给jsb29.jpg)

   ![](gitlab实验文档.assets/新建项目成功30.jpg)

### 5. Gitlab CI/CD

1. Gitlab CI/CD是一个内置在gitlab中的工具。通过持续方法进行软件开发：
   1. Continuous Integration ：持续集成
   2. Continuous delivery ：      持续交付
   3. Continuous Deployment：持续部署

2. 持续集成原理是将小代码块推送到git仓库中托管，并且每次推送，都要运行一系列脚本构建、测试和验证等。然后再合并到主分支。
3. 持续交付和部署相当于进一步的持续集成。可以把代码推送到仓库默认分支的同时将应用程序部署到生产环境。
4. CI/CD由gitlab-ci.yml文件进行配置。位于仓库的根目录下。指定脚本由gitlab runner执行。
5. gitlab至少包含三部分：一个gitlab实例、一个gitlab runner、一个gitlab-ci文件。
6. 一条流水线(pipeline)包含多个阶段(stage)。一个阶段又包含多个任务(job)任务是流水线最小单元。.gitlab-ci.yml是执行一个job的具体内容。

### 6. Gitlab runner

1. Gitlab-Runner是配合Gitlab-CI进行使用的一个用来执行软件集成脚本的东西。用于运行任务作业并将结果发送回gitlab。Gitlab-ci就是一套配合Gitlab使用的持续集成系统。
2. gitlab runner主要分三种
   1. shared： 运行整个gitlab平台项目的作业
   2. group：   运行特定group下的所有项目的作业
   3. specific： 运行指定的项目作业
3. Gitlab Runner两种状态
   1. locked：无法运行项目作业
   2. paused：不会运行作业
4. 使用docker起一个gitlab-runner容器。

```
[root@www ~]# docker run -itd  --restart=always --name gitlab-runner \
> -v /data/gitlab-runner/config/:/etc/gitlab-runner \
> -v /var/run/docker.sock:/var/run/docker.sock 77a7b2f30dd5(gitlab-runner镜像ID值。跟镜像名亦可。)
899afa5f374cdf1c96c17ca41ac7ef5d5e89e0fc7c4f381f6434bb07efc9fd22
[root@www ~]# docker exec -it gitlab-runner /bin/bash
root@899afa5f374c:/# gitlab-runner -v
Version:      14.6.0
Git revision: 5316d4ac
Git branch:   14-6-stable
GO version:   go1.13.8
Built:        2021-12-17T17:35:51+0000
OS/Arch:      linux/amd64
```

1. gitlab-runner向gitlab注册。

   **注册前提必须有一个可以使用的gitlab仓库。

2. 点击用户管理。选择项目管理，选择项目进入。在项目里面找到settings-->CI/CD-->Runners。找到仓库路径和token值。

   ![](gitlab实验文档.assets/gitlab-runner注册1.jpg)

   ```css
   root@899afa5f374c:/# gitlab-runner register		##注册命令
   Runtime platform                                    arch=amd64 os=linux pid=32 revision=5316d4ac version=14.6.0
   Running in system-mode.
   
   Enter the GitLab instance URL (for example, https://gitlab.com/):
   http://192.168.200.102:7002/		##填写gitlab仓库url路径
   Enter the registration token:
   TAsNToZzJvUCfmo_XPx6				##填写对应的token值
   Enter a description for the runner:
   [899afa5f374c]: node1.hes1.com^H	##对此runner的描述
   Enter tags for the runner (comma-separated):
   hes1								##给此runner打标签
   Registering runner... succeeded                     runner=TAsNToZz
   Enter an executor: custom, docker, parallels, virtualbox, docker-ssh, shell, ssh, docker+machine, docker-ssh+machine, kubernetes:
   docker								##指定执行方式，这里选docker。如果宿主机直接安装建议选shell。
   Enter the default Docker image (for example, ruby:2.6):
   latest
   Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
   ```

   ```css
   root@899afa5f374c:/# gitlab-runner restart		##重启runner
   Runtime platform                                    arch=amd64 os=linux pid=43 revision=5316d4ac version=14.6.0
   root@899afa5f374c:/# gitlab-runner list			##list列出runner信息。
   Runtime platform                                    arch=amd64 os=linux pid=78 revision=5316d4ac version=14.6.0
   Listing configured runners                          ConfigFile=/etc/gitlab-runner/config.toml
   node1.hes1.co                                     Executor=docker Token=VGurpzrQsCbLMarnQrUs URL=http://192.168.200.102:7002/
   ```

   **runner注册完成后会在 /etc/gitlab-runner目录下生成一个config.toml的文件。这个就是runner的配置文件。因为在安装runner的时候我们已经将配置文件的目录通过挂载的形式映射到了宿主机目录：/data/gitlab-runner/config 下，所以后续如果需要更新runner配置文件可以直接在宿主机上进行修改。并且在宿主机上进行修改runner配置文件不需要重启runner。它会每5分钟检查一次文件自动获取所有更改。包括该[[runners]]部分中定义的任何参数以及全局部分中的大多数参数（除外）listen_address。**

   配置如下：

   ```css
   root@899afa5f374c:/etc/gitlab-runner# cat config.toml
   concurrent = 1
   check_interval = 0
   
   [session_server]
     session_timeout = 1800
   
   [[runners]]
     name = "node1.hes1.co"
     url = "http://192.168.200.102:7002/"	##你的gitlab访问url地址
     token = "VGurpzrQsCbLMarnQrUs"		##在gitlab的ui上看到的token
     executor = "docker"
     [runners.custom_build_dir]
     [runners.cache]
       [runners.cache.s3]
       [runners.cache.gcs]
       [runners.cache.azure]
     [runners.docker]
       tls_verify = false
       image = "latest"
       privileged = false
       disable_entrypoint_overwrite = false
       oom_kill_disable = false
       disable_cache = false
       volumes = ["/cache"]
       shm_size = 0
   root@899afa5f374c:/etc/gitlab-runner#
   ```

3. 注册完成后，返回gitlab的ui查看runner信息。而且一个是shared。一个是group模式。

   ![](gitlab实验文档.assets/runner两种模式4.jpg)

   在点击runner时报错.这个时候我们可以去这台访问主机(win真实主机的hosts文件进行一下gitlab容器启动的虚拟级的IP和域名指定即可。测试方法。正常方法应该是搭建一个DNS服务器。通过DNS服务器进行域名解析)

   ![](gitlab实验文档.assets/runner找不到主机名称报错3.jpg)

   **当此时都可访问域名，但是当我像做gitlab-runner配置时，提示报错。

   ![](gitlab实验文档.assets/gitlab-runner报错连接被拒绝5.jpg)

   然后我们在仓库的根目录添加一个.gitlab-ci.yml文件后。在.gitlab-ci.yml中写入需要执行的任务脚本信息。然后到CI/CD-->pipelines中查看执行的流水线，在jobs中查看具体的任务信息。

4. s

5. s

6. s

7. s

8. 

9. 

### 7. GitLFS

#### 7.1GitLFS简介：

1. GitLFS(git 大文件存储，Large File Storage，简称LFS)目的是更好的把大型二进制文件。视频等资源集成到git工作流中。git存储二进制文件的所有版本，所以效率不高。而lfs用文本指针替换。这些指针包含二进制文件信息的文本文件。指针存储在git中。而大文件本身通过https托管在git lfs服务器上。
2. **手动开启步骤：在gitlab(14.6.1)web页面上未找到gitlfs开启位置？**
3. 文件存储位置：

```css
### Git LFS
gitlab_rails['lfs_enabled'] = true
gitlab_rails['lfs_storage_path'] = "/var/opt/gitlab/gitlab-rails/shared/lfs-objects"(默认配置目录。修改为其它配置目录后重启gitlab服务会导致gitlab容器一直重启。)
```

​	仓库被删除之后。lfs文件并未删除，而是仍然存放在服务器上面。

#### 7.2 测试步骤

##### 	测试环境

```ccs
[root@www ~]# git lfs install
Git LFS initialized.								##git lfs已安装
[root@www ~]# git --version
git version 2.18.0									##git版本信息
root@www:/# cat /opt/gitlab/embedded/service/gitlab-rails/VERSION
14.6.1												##gitlab版本信息
[root@www gitlab]# git lfs --version
git-lfs/3.1.2 (GitHub; linux amd64; go 1.17.6)		##git-lfs版本信息
```

1. 环境搭好后。我们先在gitlab上新创建一个gitlfs_test仓库。

2. 在本地新建一个test1的空文件夹。使用git clone +仓库地址将仓库克隆到本地此文件夹。

   ```css
   git clone http://www.hes3.com.cn/developer_jsb/gitlfs_test.git
   正克隆到 'gitlfs_test'...
   fatal: unable to access 'http://www.hes3.com.cn/developer_jsb/gitlfs_test.git/': Failed connect to www.hes3.com.cn:80; 拒绝连接				##使用http地址显示连接被拒绝。我们就换ssh地址clone。
   [root@www gitlfs_test]# git clone ssh://git@www.hes3.com.cn:7003/developer_jsb/gitlfs_test.git
   正克隆到 'gitlfs_test'...
   remote: Enumerating objects: 3, done.
   remote: Counting objects: 100% (3/3), done.
   remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
   接收对象中: 100% (3/3), 完成.				##显示克隆成功。
   ```

3. 然后我们在此文件夹下上传几个.mp4格式的视频大文件。使用 git lfs track "*.mp4"追踪。追查完成后会默认在根目录生成一个.gitattributes文件。我们可以使用cat 命令查看文件信息.也可以使用git lfs ls-files命令查看被追踪文件。

   ```css
   [root@www gitlfs_test]# cat .gitattributes
   *.mp4 filter=lfs diff=lfs merge=lfs -text
   [root@www gitlfs_test]# git lfs ls-files
   82c0b167e4 * 20220301_235820.mp4
   327e9b6eaa * 20220302_003435.mp4
   6173dd755a * 20220302_210308.mp4
   ```

4. 接下来我们使用git add 文件名/. 来添加修改过的文件。(.代表全部)

5. 使用git commit -m “描述信息”   文件名/.         提交信息。

6. 使用git status 查看状态。

7. 使用git push +远程仓库名 +远程仓库地址/远程分支名称 将新的内容推送到远程仓库。

   ```css
   [root@www gitlfs_test]# git push origin main
   batch response: Post "http://www.hes3.com.cn/developer_jsb/gitlfs_test.git/info/lfs/objects/batch": dial tcp 192.168.200.102:80: connect: connection refused
   error: 无法推送一些引用到 'ssh://git@www.hes3.com.cn:7003/developer_jsb/gitlfs_test.git
   ```

   **提示报错，80端口被拒绝。和一些引用无法推送**。

   ```css
   [root@www gitlfs_test]# git remote set-url origin 				git://git@www.hes3.com.cn:7003/developer_jsb/gitlfs_test.git
   ### 设置默认远程方式为ssh方式
   [root@www gitlfs_test]# git config --get remote.origin.url
   git://git@www.hes3.com.cn:7003/developer_jsb/gitlfs_test.git
   ### 查看默认远程方式
   [root@www gitlfs_test]# git push -u origin main
   fatal: Unable to look up git@www.hes3.com.cn (port 7003) (未知的名称或服务)
   ### 使用ssh远程推送方式推送。 
   ```

   **报错，未知的名称或服务**

8. s

9. s

10. s

11. s

12. 

### 8.

### 9.



