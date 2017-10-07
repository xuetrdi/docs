# Git 学习笔记

+ 版本控制系统的分类和历史：
  - 本地版本控制系统(文件补丁记录变更rcs)
  - 集中化版本控制系统(中央服务器Subversion)
  - 分布式版本控制系统(Git，Mercurial)

[注意]Subversion记录文件内容的具体差异;Git不记录内容，只记录文件快照,变更记录保存在本地。

+ 本地文件的三种状态
  - 工作区
  - 暂存区
  - 本地仓库
+ 工作区的文件两种状态
  - 已追踪文件
  - 未追踪文件
+ 暂存区的文件两种状态
  - 已暂存文件
  - 未暂存文件
+ 获取项目的Git仓库
  - 在工作目录中初始化新仓库(git init)
  - 克隆仓库(git clone)
+ Git支持许多数据传输协议
  - Git协议:git://
  - HTTP协议:https://
  - SSH协议:user@server:/path.git
+ 检查文件状态
  - git status
+ 追踪新文件
  - 工作区未追踪文件git add，则文件处于追踪状态，文件从工作区添加到暂存区
  - git commit把暂存区文件提交到本地仓库
+ 忽略某些文件
  - 在工作目录编辑.bitnore文件
+ 查看暂存区文件
  - git diff显示还没有暂存区的改动
  - git diff --cached已暂存与上次提交差异
+ 提交更新
  - git commit 条件已暂存文件提交
  - git commit -a会把所有已追踪文件更新和提交
+ 移除文件
  - 已追踪文件清单移除，从暂存区移除，然后提交
  - git rm -f,从暂存区移除，从工作区删除
  - git rm --cached,只移除，不删除文件
  - 文件移动重命名,git mv file_from file_to
+ 查看提交历史
  - git log -p,显示详细差异
  - git log --stat,显示简略差异
  - git log --pretty,美化格式显示
+ 撤销操作
  - 修改上一次提交,git commit --amend
  - 提交漏掉文件,git add,git commit --amend
+ 取消已暂存文件
  - git reset HEAD filename
+ 取消对文件的修改
  - git checkout -- filename,危险命令,把所有操作恢复到上一个版本。
+ 远程仓库
  - 查看远程仓库,git remote -v
  - 要添加远程仓库,git remote add [别名] url
  - 从远程仓库抓取数据,git fetch 别名,只拉取,不合并
  - 拉取并合并到当前分支,git pull
+ 推送数据到远程仓库
  - git push remote-name branch-name
  - git push origin master,本地master,推送远程库origin
+ 查看远程仓库信息
  - git remote show remote-name
+ 远程仓库的删除和重命名
  - 重命名,git remote rename oldname newname
  - 删除,git remote rm remote-name
+ 打标签
  - 查看所有标签,git tag
  - 查看某个版本标签,git tag -l "v.1.*"
  - 新建标签,git tag -a tagname -m "comments"
  - 分享标签,git push origin --tags
+ 设置别名简写
  - git config --global alias.co checkout
  - git config --global alias.ci commit
  - 等等,都可以设置
+ 分支
  - 分支是指向commit对象的指针
  - 创建新的分支指针,git branch branch-name,会在commit对象上新建一个分支指针，既有多个指针指向commit对象。
  - HEAD指针是指向分支的指针
  - 切换分支git checkout branch-name,即是HEAD指向的改变
  - 新建并切换到分支,git checkout -b branch-name
+ 合并分支
  - 回到被合并的分支,git checkout master
  - 合并分支到master,git merge branch-name
  - 删除已被合并过的分支,git branch -d branch-name
+ 解决冲突
  - 在不同分支中修改同一文件的同一内容
  - merge后如果遇到冲突,只合并,未提交,等待解决冲突
  - 查看冲突,git status
  - 修改冲突文件
  - 修改后的文件添加到暂存区,git add filename
  - 合并提交,git commit filename
+ 分支管理
  - 查看分支,git branch -v,带*的为当前所处的分支
  - 查看合并入当前分支的分支,git branch --merged
  - 查看未合并入当前分支的分支,git branch --no-merged
  - 删除分支,git branch -d branch-name,如果未合并会报错
  - 强制删除分支,git branch -D branch-name
+ 利用分支进行开发工作流程
  - 远程分支clone到本地会新建一个指向本地仓库的指针
  - git fetch origin更新本地数据,则在本地产生了指针移动
  - 推送本地分支,git push 远程库名 分支名,取出本地某个分支推送到服务器
  - 远程指针指向的分支不可编辑,但可以合并到当前分支,跟踪远程分支
  - 删除远程分支,git push 远程名 :分支名
+ 衍合rabase

---------------
## 在常用编辑器上集成Git环境

+ 在Emacs/Spacemacs上集成Git
+ 在Sublime上集成Git

1. 在Emacs/Spacemacs上集成Git以及使用
2. 在Sublime上集成Git以及使用


---------------
## 使用场景举例
+ 推送本地仓库到github服务器上失败
+ 如何合并github上的两个分支

1. 推送本地仓库到github服务器上失败

+ Permission denied (publickey).fatal the remote hang up unexpectly解决办法
  1. 本地生成公钥私钥对()
  2. 复制公钥内容，创建github公钥，粘贴公钥内容
  3. 设置全局环境变量user.name,user.email(git config --global user.name "xx"),这里的设置会被保存在～/.gitconfig文件里面，也可以 直接在这个文件里面编辑
+ ssh: Could not resolve hostname github.com: Name or service not known.fatal: Could not read from remote repository解决方法
  - 说明:这种情况就是git remote add成功，但是就是push不成功
  1. ping github.com获取一个github的服务器ip地址,或者创建秘钥后测试ssh -T git@github.com里面返回的ip地址，或者网上搜一下，一般就那么几个
  2. 如果是Linux在/etc/hosts增加你找到的ip然后配置github.com,如果是Windows找到C盘中存放hosts文件的方法同样的编辑方法，保存
  3. 重新推送(git push -u origin master)，第一次推送带-u参数，以后不需要

2. 如何合并github上的两个分支？

- 首先，把github远程仓库克隆到本地，并查看分支(git branch -a)，看是否有各个分支
- 其次，如果没有切换到被合并的分支查看(git checkout branch-name)，切换后会创建远程分支对应的本地分支
- 再次，回到本地主分支(git checkout master)
- 再次，合并本地分支到本地主分支上，如果有冲突，修改冲突文件，再次提交，直至成功
- 最后，推送合并后的分支到github远程仓库(git push -u origin master),首次推送加-u，其余不用加