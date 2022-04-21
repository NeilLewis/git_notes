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

初始化本地仓库，如已进入目录 repo，则使用如下命令

`git ini`

如初始化仓库 repo，则使用如下命令

`git init repo`

修改当前目录的 git 配置文件

`git config -e`

修改git全局配置

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

`git reset`

移动或者重命令工作区文件

`git mv`

文件修改过之后，将文件提交到本地仓库 

`git commit -m "adding files"`

如果不想每个文件都add，由git自动来提交本地修改，可以使用 -a，-a选项可将所有**被修改或者已删除的且已经被git管理的文档**提交到仓库中，-a不会造成新文件被提交，只能修改

`git commit -ma "change some files"`

查看仓库状态

`git status`

查看提交日志

`git log`

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

切回主分支 master

`git checkout  master`

将 test 分支的更改提交到主分支, 则在上一步之后

`git merge test`

如果要删除分支

`git branch -d test`





