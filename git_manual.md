# git 使用

## 1. 认识 git

### 1.1 git 介绍

Git是目前世界上最先进的分布式版本控制系统

Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

### 1.2 git 对比 svn

* **1、Git 是分布式的，SVN 不是**：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS 等，最核心的区别。

* **2、Git 把内容按元数据方式存储，而 SVN 是按文件：**所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。

* **3、Git 分支和 SVN 的分支不同：**分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。

* **4、Git 没有一个全局的版本号，而 SVN 有：**目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。

* **5、Git 的内容完整性要优于 SVN：**Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

  ![git&svn](E:\share\workspace\git\notes\git&svn.png)

## 2 git 使用

### 2.1 git 流程

![git_workflow](E:\share\workspace\git\notes\git_workflow.png)

### 2.2 git 概念详解

* **工作区worksapace：**就是你在电脑里能看到的目录
* **暂存区stage：**英文叫 stage 或 index(or staging area)。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）
* **版本库local reopsitory：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库![workspaceToStage](E:\share\workspace\git\notes\workspaceToStage.png)
* 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个指针。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
* 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容
* 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID(哈希值)被记录在暂存区的文件索引中。
* 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树
* 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响
* 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
* 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
* 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

### 2.3 git 命令

初始化本地仓库(将已有项目纳入git管理)，如已进入目录 repo，则使用如下命令

`git ini`

如初始化仓库 repo(项目repo还不存在)，则使用如下命令

`git init repo`

修改当前目录的 git 配置文件

`git config -e`

修改git全局配置(系统--system,本仓库--local)

`git config -e --global`

设置用户信息

`git config --global user.name "runoob"`

`git config --global user.email test@runoob.com`

在工作区 repo 中添加文件 file0

`git add file0`

在工作区 repo 中添加其他所有文件

`git add .`

同样删除工作区文件

`git rm .`

对比工作区和暂存区文件差别

`git diff`

回退版本

`git reset --hard`

移动或者重命令工作区文件

`git mv readme readme.md`

文件修改过之后，将文件提交到本地仓库 

`git commit -m "adding files"`

如果不想每个文件都add，由git自动来提交本地修改，可以使用 -a，-a选项可将所有**被修改或者已删除的且已经被git管理的文档**提交到仓库中，-a不会造成新文件被提交，只能修改

`git commit -ma "change some files"`

查看仓库状态

`git status`

查看提交日志

`git log`

单行查看

`git log --oneline`

查看最新两次

`git log -n2`

图形界面查看

`gitk`

查看拓扑图

`git log --all --graph`

`git log --oneline --decorate --graph`

git默认的是最新的在最上面,可以使用--reverse指定方向

`git log --reverse --oneline`

查找指定用户提交日志,显示5条记录

`git log --author=Annie --oneline -5`

你要指定日期，可以执行几个选项：--since 和 --before，但是你也可以用 --until 和 --after,--no-merges 选项隐藏合并提交

`git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges`

以列表方式查看指定文件的历史修改记录

`git blame file0`

从远程服务器端克隆一个库到本地

`git clone ssh://example.com/~/www/project.git`

修改好之后同步更新到远程库

`git push ssh://example.com/~/www/project.git`

将当前分支自动与唯一一个追踪分支进行合并

`git pull`

从非默认位置更新到指定的远程git仓位

`git pull http://git.example.com/project.git`

从资源库中删除文件 file0

`git rm file0`

查看分支

`git branch`

从当前 master 创建分支 test

`git branch test`

切换到 test 分支

`git checkout test`

以上两步可以合并为

`git checkout -b test`

切回主分支 master

`git checkout  master`

将 test 分支的更改提交到主分支, 则在上一步之后

`git merge test`

如果要删除分支

`git branch -d test`

使用-D强制删除

`git branch -D test`

查看所有的标签

`git tag`

并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。用 git tag -a v1.0 命令给最新一次提交打上（HEAD）"v1.0"的标签

`git tag -a v1.0`

给前一个版本打标签

`git tag -a v0.9 85fc7e7`

指定标签信息命令

`git tag -a <tagname> -m "runoob.com标签"`

PGP签名标签命令

`git tag -s <tagname> -m "runoob.com标签"`

git连接到远程仓库,使用rsa生成秘钥

`ssh-keygen -t rsa -C "youremail@example.com"`

成功的话会在~/下生成.ssh文件夹,进去之后复制id.rsa.pub,复制里面的内容粘贴到git账户配置添加key信息的地方

指定算法

`ssh-keygen -t ed25519 -C "your_email@example.com"`

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

进入.git目录查看git对象HEAD

`cat HEAD`

进入HEAD指向的目录下

`cd refs/heads`

`cat master`

查看文件属性

`git cat-file -t 40e100d231fbfe7934afb22fd91e9647831a4565 `

查看文件内容

`git cat-file -p 40e100d231fbfe7934afb22fd91e9647831a4565 `

.git目录下核心的目录refs/,refs下存储的是引用, objects/objects下存储的是对象

commit /tree/blob三个对象之间的关系

`一个commit对应一个tree(所有文件的快照),tree里面包含项目的视图,blob和文件名无关,只要内容一样在git里面都是同一个blob `

查看objects下文件

`find .git/objects/ -type f`

切换到上一个commit(上两个HEAD~2)

`git checkout HEAD~`

通常情况下checkout与分支名称交互,如果指定了commit的哈希值XYZ,就会进入detached HEAD状态(也就是HEAD指针指向XYZ,但没有指向具体的分支),如果在detached HEAD状态状态下做的变更不重要,那么git会自动清理掉,如果很重要,则需要新建一个分支与之关联上

`detached HEAD`会在哪些情况出现

有一些情况下，`detached HEAD`状态很常见：

- 子模块确实在特定提交而不是分支中检出
- `Rebase`通过在运行时创建临时`detached HEAD`状态来工作

`detached HEAD`不应出现的地方

另一种情况：如果要及时回顾一下您项目的旧版本呢？例如，在bug的上下文中，您希望了解旧版本中的工作原理,这种情况下可以创建一个临时分支,并在完成之后将其删除

HEAD指向分支最新一个commit

`git diff HEAD HEAD^1 `等价于`git diff HEAD HEAD^`等价于`git diff HEAD HEAD~`等价于`git diff HEAD HEAD~1`

`git diff HEAD HEAD^2 `等价于`git diff HEAD HEAD^^`等价于`git diff HEAD HEAD~2`

## 3 git 使用特殊场景

修改最新一次commit的message

`git commit --amend`

修改老旧的commit的message ,比如需要修改commit [xxx]的message,则需要基于其上一个commit [yyy]进行message修改, 策略为r(reword修改message)

`git rebase -i yyy`

同样适用上述的rebase可以合并多个commit的message, 需要设置策略是 s(squash 合并commit),对于不连续的commit, 则需要把合并的commit的行放入到一起

暂存区和head内容比较

`git diff --cached`

比较工作区与暂存区的差别

`git diff`

对比具体的文件的差别

`git diff -- readme.md`

如何将暂存区的所有内容和HEAD中变为一样

`git reset HEAD`

只将暂存区的部分文件恢复和HEAD中一样

`git reset HEAD -- style/style.css`

`git reset HEAD -- style/style.css readme.md`

强制将暂存区和工作区的头指针都指向某个commit [xxx], xxx之后的commit会被舍弃

`git reset --hard xxx` 

比较两个分支之间的差异,默认是全部文件

`git diff temp master`

比较指定的文件的差异

`git diff temp master -- index.html`

如果temp好master的哈希值分别为yyy/zzz,则上述命令同样可以写成以下命令

`git diff yyy zzz -- index.html`

临时遇到紧急任务加塞,可以把当前的内容暂时放到堆栈

`git stash`

查看堆栈内容

`git stash list`

解决紧急任务之后取出

`git stash apply`把之前存到堆栈里面的内容取出来到工作区,同时堆栈里面的内容也继续存在

而使用pop之后堆栈里面的内容会丢弃

`git stash pop`

通过.gitignore添加不需要添加到git控制的文件列表



## 4 git备份



### 4.1 使用的传输协议一览

![git常用传输协议](E:\share\workspace\git\notes\git常用传输协议.png)

### 4.2 哑协议和智能协议对比

* 直观区别：哑协议传输进度不可见，智能协议传输可见
* 传输速度：智能协议比哑协议传输速度快

### 4.3 本地备份

哑协议备份

`git clone --bare ../notes/.git ya.git`



