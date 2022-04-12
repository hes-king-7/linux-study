<div align='center' ><font size='70'>第一章 
git介绍</font></div>
## git原理
git是一个开源的分布式版本控制系统。
1. git组成
   1. 软件
   2. 版本控制
   3. 分布式
2. git工作步骤
   1. 进入文件夹        git init--创建.git文件。执行初始化命令
   2. 给与git授权       git status 查看目录下文件的工作状态
   3. 管理文件夹内容    git add+文件名/. 管理指定文件/所有文件。
   4. git              git commit -m '版本信息描述'
   5. git              git log 查看版本信息日志
3. git三大区域
   1. 工作区            自动生成的文件
   2. 暂存区            add过程
   3. 版本库            commit过程
4. git回滚
   1. git reset --hard +版本ID值 回滚到ID值对应版本。
   2. git reflog 查看回滚前的版本信息
5. 分支 ————工作流
   1. 创建新的分支，对bug进行紧急修复，修复后合并到原主干线
   2. git branch    查看当前分支
      1. git branch +分支名     创建新分支
      2. git checkout +分支名   切换到新分支
      3. git merge +分支名      要合并的分支
         1. 注意：切换分支再合并(可能产生冲突，需手动解决)
      4. git branch -d +分支名称    删除分支
## github(开源的代码托管的仓库。相当于云)/gitlab(开源的工具，可以依靠此工具自己搭建一个代码仓库)
1. 注册账号---2.创建仓库---本地代码上传
2. 给远程仓库起别名
   1. git remote add origin +远程仓库地址
3. 向远程推送代码
   1. git push -u origin 分支名称
4. 在本地拉取远程代码
   1. git pull origin 分支名称
5. 克隆远程仓库代码
   1. git clone 远程仓库地址（内部已实现起别名）
## git rebase(变基) ：使git记录简洁
1. git rebase应用的三种情况
## git 快速解决冲突
1. 安装beyond compare
2. 在git中配置
   1. git config --local merge.tool bc3
   2. git config --local mergetool.path '/usr/local/bin/bcomp'
   3. git config --local mergetool.keepBackup false
3. 应用beyond compare解决冲突
   1. git mergetool
4. 记录图形展示
   1. git log --graph --pretty=format:"%h %s"
5. tag标签
   1. git tag -a 标签信息 -m 版本信息
6. git实现免密码登录的方式
   1. URL中体现：https://用户名:密码@url链接
   2. SSH实现："最常用"
      1. 生成公钥和私钥(默认放在~/.ssh目录下)
      2. 拷贝公钥的内容，并设置到github中
      3. 在git本地中配置SSH地址
   3. git自动管理凭证
7. git忽略文件
   1. 让git不再管理当前目录下的某些文件。 gitignore文件下编辑。
   2. 更多参考https://github.com/github/gitignore
8. git任务管理相关
   1. issues，文档以及任务管理，问题反馈汇总
   2. wiki，项目文档描述。
## git配置文件
git配置文件分三个配置文件
1. 项目配置文件： 项目目录下的/.git/config      --local
2. 全局配置文件： 在~/.gitconfig                --global
3. 系统配置文件： 在etc/.gitconfig              --system 需要有root权限
## 给开源软件贡献代码
1. fork源代码
   1. 将别人的源代码拷贝到我自己的远程仓库
2. 在自己的仓库进行修改代码，并提交到自己的远程仓库
3. 给源代码的作者提交修复bug的申请，提交命令(pull request)

