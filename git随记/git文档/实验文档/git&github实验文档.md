# git&github&gitlab实验文档

## 一、git简介

Git--只是一个命令行工具，是一个开源的分布式版本控制系统。可以管理本地代码仓库和对远程仓库的资源更新。

Githua是一个代码托管网站，一个面向开源及私有软件项目的托管平台，给用户提供git服务。

Gitlab是一种可以搭建类似于github服务平台的管理工具。更倾向于私有云。供企业私人内部管理代码。

## 二、git&github&gitlab关系图

![](D:\study\LINUX\git\git学习资料\git.jpg)

其中，repository(本地仓库)供git管理。remote(远程仓库)相当于github/gitlab用于在远端存储管理更新本地仓库代码。或把远程仓库代码下载到本地的作用。

## 三、软件安装&使用

#### git与github基本使用。

1. 去往git官网https://git-scm.com/downloads下载git安装包。win下直接点击.exe安装包安装即可。安装过程可自行百度。

2. 然后去往github官网https://github.com/进行账号注册。使用邮箱进行账号注册。注册步骤：1.官网首页选择sign up输入对应信息后，点击页面底部create an account创建对应账号。然后进入激活邮箱激活即可。注册成功后返回登录即可。

3. 然后在github上进行操作

   1. 新建一个仓库(New repository)

   2. 在仓库界面创建项目以及一系列信息描述。选择公开即所有人可看。选择私人只有自己和有相关权限的用户可看/编辑。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\github新建项目.jpg)

   3. 新建完成后进入新建的存储库界面

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\存储库界面1.jpg)

   4. 创建完成后即可在此界面选择code。找到对应的https存储库链接地址。在本地已安装好的git系统上新建一个文件夹。右键打开git bash here。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\存储库界面2.jpg)

1. 本地系统上

   1. 在新创建文件夹内右键打开git bash here

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\git bash 打开.png)

   2. 然后会弹出一个git命令行界面。在此界面上可以使用git init命令初始化此文件夹使之生成为本地git仓库。也可以使用git clone +远程仓库地址。将远程仓库克隆到本地。(这里为了保持后续本地和远程仓库的一致性。我们直接演示使用git clone 命令将远程仓库克隆到本地)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\本地克隆远程仓库.jpg)

   3. 克隆完成后即可看到本地文件夹下多了一个test文件夹。里面有包含一个.git文件夹。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\文件夹1.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\文件夹2.jpg)

   4. 这时我们本地仓库和远程github仓库都已新建完成。接下来我们就测试在本地仓库新建文件看是否可以正常上传远程仓库。

2. 测试在本地仓库git命令行下新建文件后打包上传文件测试。

   1. 首先进入test1仓库下。在这里我们可以看到进入test1后会显示一个master。这里的master就代表本地test1仓库的master主分支。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\进入test1仓库.jpg)

   2. 使用git branch 命令可查看本地此仓库下所有分支。也可使用git branch+分支名称，新建分支。使用git branch -a 查看所有分支(包括本地和远程分支)。 使用git branch -r查看远程分支。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\新建分支和查看分支.jpg)

   3. 创建完分支后，可以使用git checkout+分支名(hes1)，切换分支。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\切换分支步骤.jpg)

   4. 然后在当前分支下新建文件/文件夹(touch+文件名)/(mkdir+文件夹名) 

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\新建内容.jpg)

   5. 新建完后使用git status 命令查看当前仓库状态，即可看到新建内容为红色标记状态，此状态意为新的变动还未提交状态。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\查看状态信息.jpg)

   6. 使用git add +文件名或.来把新的内容进行提交。/后跟.表示提交当前目录下所有。后跟文件名，表示只提交此文件。使用git add后相当于把新内容从工作区提交到暂存区。然后使用git commit -m"描述信息" +文件名。将暂存区文件提交到版本库中。描述信息相当于此次提交的版本库的tag标签信息。提交完成后再次使用git status查看main/状态已变绿。即说明提交成功，使用git log命令可查看当前所有已提交过的版本信息日志。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\提交流程.jpg)

   7. 当我们使用git remote -v 可以看到本地有两个指向远端代码库的origin。(因为我们是从这个地址clone下来的) **这里的origin就是一个名字，是我们在clone一个github上的代码库时，git为其默认创建的指向这个远程代码库的标签**(开始一直没懂origin的意思。)可以使用git remote add命令修改为其它的标签。“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\别名添加.jpg)

   8. 使用 git push （-u hes1+远程地址）括号内为可选项，因为我们已经设置添加了别名本地已有信息缓存。不指定时默认找本地缓存指定的远程路径。此处-u的意思是。指定一个默认主机，这样后面不加任何参数使用git push使。默认使用第一次-u参数后指定的主机。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程推送.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程查看.jpg)

   9. 这时我们可以在远端github上看到已经推送成功。

   10. 然后我们测试远端修改文件后本地下载同步过程。

       ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程新建文件.jpg)

   11. 然后使用git branch -r查看远程分支名称。默认origin/master

       ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\产看远程分支名称.jpg)

   12. 然后使用git pull origin +远程分支名称。从远程分支拉取代码到本地当前分支。

       ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程下载到本地.jpg)

   1. 到此我们可以看到本地到远端上传代码和远端到本地下载代码都已可以实现。**注意，此时我们只是将其代码下载到本地的master分支仓库。其它仓库并没有**我们可以使用git branch查看所有分支后，使用git checkout +分支名称切换到其它分支进行文件查看。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\切换分支查看.jpg)

   2. 此时我们可以看到新的分支develop里面并无master分支里面刚刚新建和下载的信息。如果需要产生此信息有两种方法。

      1. 切换到此分支后重新操作一遍远程下载操作。

         ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\其它分支下载远程代码.jpg)

      2. 使用分支合并操作git merge+分支名 命令，将其它分支内容合并到当前分支。**注意：分支合并操作只能由低分支找高分支进行合并。同级分支无限制**。但是分支合并时可能会因为不同分支对同一文档进行操作。导致合并时出现分支冲突。此时我们就需要进入冲突文件将其冲突项进行手动修改操作。(或使用第三方软件配合解决。具体软件可自行百度。)

   3. 分支合并有两种方式：

      1.本地分支合并。在本地dev分支进行信息修改后。切换到本地master分支。执行分支合并操作。

      ​	a. 使用vim +文件名新添加代码信息

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\dev端新添加内容.jpg)

      ​	b.切换回本地master分支。后执行git merge +分支名。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\本地分支合并.jpg)

      2.远程分支合并到本地分支。在本地develop分支更新代码信息后，先使用git push命令上传到远程github分支代码上。然后本地再切换回master分支后，在master分支上使用git pull +远程分支名。将远程分支更新后的代码重新下载到本地master，实现远程分支合并操作。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\本地dev上传到远程dev分支.jpg)

   4. 首先在远端github上新建一个develop分支。然后在本地develop分支上上传代码到远端github上的develop分支。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev分支收到更新内容.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作1.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作2.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程合并选项.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作3.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作4.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作5.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作6.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作7.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\远程dev合并到远程master分支操作8.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\查看远程分支合并结果.jpg)

   5. 回到本地分支，切换回master分支后，先查看当前本地master分支信息，后进行git pull命令下载最新代码信息再次查看。

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\查看本地master分支.jpg)

      ![](D:\study\学习软件\VSCode\netpode\linux服务\git随记\git文档\实验文档\实验图片\本地master分支下载后查看结果.jpg)

   6. 此时可以看到本地master分支通过下载远程github上master分支最新代码，也完成信息同步。远程合并操作也完成。

3. 到此。git与github常用操作已基本实现。
