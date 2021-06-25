# `Git`简书

> 个人`Git`学习笔记。跟廖雪峰的`Git`教程走了一遍（`https://www.liaoxuefeng.com/wiki/896043488029600`）。
>
> ```bash
> # ----------------------设置、查询设置----------------------------------
> $ git config --global user.name "Your Name"           # 设置user.name
> $ git config --global user.email "email@example.com"  # 设置user.email
> $ git config user.name                                # 查询user.name
> $ git config --list                                   # 查询此处所有设置
> # --------------------初始化仓库（repository）--------------------------
> $ git init                                            # 将当前目录初始化为仓库
> # --------------------添加文件至暂存区（stage）--------------------------
> $ git add <file>                                      # 将文件添加到仓库暂存区
> # --------------------------提交修改-----------------------------------
> $ git commit -m <msg>                                 # 提交修改，并标注信息
> # -------------------------查看git状态---------------------------------
> $ git status
> # ----查看提交过的版本，不包括被删除的commit记录和reset的操作----------
> $ git log                                             
> $ git log --pretty=oneline                            # 每个提交信息在一行显示
> $ git log --graph                                     # 在输出的左侧绘制提交历史的基于文本的图形表示
> $ git log --abbrev-commit                             # 不显示完整的40字节十六进制commit_id,只显示唯一的前缀
> # --------------------------版本回退----------------------------------- 
> # --hard：stage区和工作区里的内容会被完全重置为和HEAD的新位置相同的内容。换句话说，没有commit的修改会被全部擦掉。
> $ git reset --hard  HEAD^                             # 回退到上个版本
> $ git reset --hard  HEAD^^                            # 回退到上上个版本
> $ git reset --hard  HEAD^^                            # 回退到上上个版本
> $ git reset --hard  HEAD@{No.}                        # 回退到{No.}版本
> $ git reset --hard  commit_id                         # 回退到commit_id的那个版本
> # ------显示所有的版本记录，包括被删除的commit记录和reset的操作------------
> $ git reflog
> # ----------------------撤销修改----------------------------------------
> $ git restore <file>                                  # 撤销未添加到暂存区的修改
> $ git restore --staged <file>                         # 撤销添加修改到暂存区
> # ----------------------删除文件----------------------------------------
> $ git rm <file>                                       # rm本地已提交到版本库的文件后，                                                                               之后git rm、git commit -m
> # ------------查看工作区和版本库里面最新版本的区别------------------------
> $ git diff HEAD -- <file>
> # ----------------------添加远程库--------------------------------------
> $ git remote add origin https://github.com/<GitAccount>/RepositoryName                                  												 # 添加远程库，起名origin
> $ git push -u origin master                           # 将本地master分支推送到远程库origin的master分支，并将本                                                         地master分支与远程库master分支关联起来
> # --------------------查看远程库信息------------------------------------
> $ git remote
> $ git remote -v                                       # 远程库详细信息
> # ----------------------删除远程库--------------------------------------
> $ git remote rm origin                                # 解除本地和远程库origin的绑定关系，并不是物理上删除了远                                                         程库，远程库本身并没有任何改动
> # --------------------- 克隆远程库--------------------------------------
> $ git clone https://github.com/dk-zen/gitskills.git   # 克隆远程库gitskills到本地
> # ------------查看、创建、切换、合并、删除分支----------------------------
> $ git branch                                          # 查看分支、目前在哪个分支
> $ git branch <name>                                   # 创建name分支
> $ git switch <name>                                   # 切换到name分支
> $ git switch -c <name>                                # 创建当前分支的name分支
> $ git merge <name>                                    # 合并name分支到当前分支,Fast forward模式
> $ git merge --no-ff -m <msg> <name>                   # 强制禁用Fast forward模式，Git就会在merge时生成一个新的                                                         commit，这样，从分支历史上就可以看出分支信息
> $ git branch -d <name>                                # 删除name分支
> $ git branch -D <name>                                # 强制删除分支
> # --------------------暂存及恢复工作------------------------------------
> $ git stash                                           # 把工作区、暂存区所有未提交的修改（包括暂存的和非暂存的）                                                         都保存起来，用于后续恢复当前工作目录。
> $ git stash list                                      # 查看暂存的工作
> $ git stash apply stash@{No.}                         # 恢复暂存的工作，但不删除stash@{No.}
> $ git stash drop stash@{No.}                          # 删除stash@{No.}队列
> $ git stash pop                                       # 恢复最新的一个stash点，并将其从stash列表中删除
> # --------------------复制修改到当前分支----------------------------------
> $ git cherry-pick <commit_id>                         # 复制提交的修改commit_id到当前分支
> # --------------------远程库拉取、推送、建立追踪关系-----------------------
> $ git checkout -b dev origin/dev                      # 创建远程库origin的dev分支到本地dev分支
> $ git push <remote> <branch>                          # 向远程库推送分支
> $ git pull <remote> <branch>                          # 取回远程库某个分支的更新，再与当前分支合并
> $ git branch --set-upstream-to=origin/<branch> dev    # 建立本地dev分支与远程库origin的某个分支的追踪关系
> # ----------------------------rebase-------------------------------------
> $ git rebase                                          # 合并多个commit为一个完整commit
> # ----------------------------标签管理------------------------------------
> $ git tag <tagname>                                   # 新建一个标签，默认为HEAD，也可以指定一个commit id
> $ git tag a <tagname> -m <msg>                        # 指定标签信息
> $ git tag                                             # 查看所有标签
> $ git tag -d <tagname>                                # 删除本地标签
> $ git push origin :refs/tags/<tagname>                # 删除远程库的一个标签
> ```

[toc]



# 1、简介

1991年，`Linus`开发了开源的`Linux`。在2002年以前，世界各地的志愿者把源代码文件通过`diff`的方式发给`Linus`，然后由`Linus`本人通过手工方式合并代码。但此时`Linux`的代码已经非常庞大，`Linus`很难继续通过手工方式管理，因此`Linus`选择了商业版本控制系统`BitKeeper`。`BitKeeper`的东家`BitMover`公司对此非常支持，授权`Linux`社区免费使用这个版本控制系统。

2005年，开发`Samba`的`Andrew`试图破解`BitKeeper`的协议，导致`BitMover`公司要收回Linux社区的免费使用权。于是，`Linus`花了两周时间自己用`C`写了一个分布式版本控制系统，这就是`Git`！一个月之内，`Linux`系统的源码已经由`Git`管理了！膜拜大神~

`Git`迅速成为最流行的分布式版本控制系统。尤其是2008年，`GitHub`网站上线了，它为开源项目提供`Git`免费存储，无数开源项目开始迁移至`GitHub`，包括`jQuery`、`PHP`、`Ruby`等等。

`Git`是一种分布式的版本控制系统，它不像`CVS`或者`SVN`把历史记录放在一个中心的服务器上，而是分布在各处，包括最终用户的本地机器上，每一处都有完整的历史纪录，这使得它不依赖中心服务器就可以工作；它具有轻便的“分支 - 合并”功能，并且它鼓励积极地使用这一功能来进行开发，每一个新的功能或者`bug`修复都可以 / 应该在一个新的分支上进行，完成之后再`merge`回主干上。


# 2、`Git`安装

在`Windows`上使用`Git`，可以从`Git`官网直接下载安装程序，然后按默认选项安装即可。

安装`Git`后，设置`user.name`,`user.email`。因为`Git`是分布式版本控制系统，所以，每个机器都必须自报家门。

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

`git config`命令用了`--global`参数，表示本地机器上所有的`Git`仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和`Email`地址。

如果想检查你的设置，可以使用 `git config --list` 命令来列出`Git`可以在该处找到的所有的设置，也可以指定查询某些字段。 

```bash
git config user.name
git config --list
```

# 3、创建版本库

`step 1:` 创建目录。

```bash
Administrator@DELL-7390 MINGW64 ~/Desktop (master)
$ cd /d
Administrator@DELL-7390 MINGW64 /d
$ mkdir mygit
Administrator@DELL-7390 MINGW64 /d
$ cd mygit
Administrator@DELL-7390 MINGW64 /d/mygit
$ pwd
/d/mygit
```

`step 2:` `git init`将该目录变成仓库`Repository`。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit
$ git init
Initialized empty Git repository in D:/mygit/.git/
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ ll -ah
total 12K
drwxr-xr-x 1 Administrator 197121 0 Jun 23 21:28 ./
drwxr-xr-x 1 Administrator 197121 0 Jun 23 21:12 ../
drwxr-xr-x 1 Administrator 197121 0 Jun 23 21:28 .git/
```

当前目录下多了一个`.git`的目录，这个目录是`Git`来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了就把`Git`仓库给破坏了。

也不一定必须在空目录下创建`Git`仓库，选择一个已经有文件的目录也是可以的。

>注意：
>
>所有的版本控制系统，其只能跟踪文本文件的改动，比如`TXT`文件，网页，所有的程序代码等等，`Git`也不例外。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从`100KB`改成了`120KB`，但到底改了啥，版本控制系统不知道，也没法知道。
>
>不幸的是，`Microsoft`的`Word`格式是二进制格式，因此，版本控制系统是没法跟踪`Word`文件的改动的，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
>
>因为文本是有编码的，比如中文有常用的`GBK`编码，日文有`Shift_JIS`编码，如果没有历史遗留问题，强烈建议使用标准的`UTF-8`编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。
>
>千万不要使用`Windows`自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存`UTF-8`编码的文件，他们自作聪明地在每个文件开头添加了`0xefbbbf`（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个`“?”`，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议下载`Notepad++`替记事本。记得把`Notepad++`的默认编码设置为`UTF-8 without BOM`。

`step3:` 创建文件并放入仓库。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git add mygit.md
warning: LF will be replaced by CRLF in mygit.md.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "add mygit.md"
[master (root-commit) d0b7869] add mygit.md
 1 file changed, 2 insertions(+)
 create mode 100644 mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
nothing to commit, working tree clean

```

> `LF`是`linux`和`Unix`系统的换行符，`CRLF`是`windows` 系统的换行符。这就给跨平台的协作的项目带来了问题，保存文件到底是使用哪个标准呢？ `Git`为了解决这个问题，提供了一个”换行符自动转换“的功能，并且这个功能是默认处于”自动模式“即开启状态的。

小结：

初始化一个`Git`仓库，使用`git init`命令。

添加文件到`Git`仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

# 4、时光穿梭机

## 4.1 版本回退

修改文件`mygit.md`，并多次提交。

版本1：

```bash
Git is a version control system.
Git is free software.
```

版本2：

```bash
Git is a distributed version control system.
Git is free software.
```

版本3：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

`git log`查看历史记录：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log
commit 87895244058aed768e26a31e549764235e776b4c (HEAD -> master)
Author: dk-zen <2005dingkai@sina.com>
Date:   Wed Jun 23 22:12:08 2021 +0800

    distributed under the GPL

commit 25912897008c90c9c731e2e5387c4c04d418900b
Author: dk-zen <2005dingkai@sina.com>
Date:   Wed Jun 23 22:05:15 2021 +0800

    add distributed

commit d0b7869922da73260e1a4a4bc18bd21efff1b695
Author: dk-zen <2005dingkai@sina.com>
Date:   Wed Jun 23 21:52:44 2021 +0800

    add mygit.md
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`distributed under the GPL`，上一次是`add distributed`，最早的一次是`add mygit.md`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --pretty=oneline
87895244058aed768e26a31e549764235e776b4c (HEAD -> master) distributed under the GPL
25912897008c90c9c731e2e5387c4c04d418900b add distributed
d0b7869922da73260e1a4a4bc18bd21efff1b695 add mygit.
```

`87895244058aed768e26a31e549764235e776b4c`是`commit id`（版本号），由`SHA1`计算得到。

每提交一个新版本，实际上`Git`就会把它们自动串成一条时间线。如果使用可视化工具查看`Git`历史，就可以更清楚地看到提交历史的时间线。

好了，现在我们启动时光穿梭机，准备把`mygit.md`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，`Git`必须知道当前版本是哪个版本，在`Git`中，用`HEAD`表示当前版本，也就是最新的提交`87895244058aed768e26a31e549764235e776b4c`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`distributed under the GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --pretty=oneline
87895244058aed768e26a31e549764235e776b4c (HEAD -> master) distributed under the GPL
25912897008c90c9c731e2e5387c4c04d418900b add distributed
d0b7869922da73260e1a4a4bc18bd21efff1b695 add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git reset --hard HEAD^
HEAD is now at 2591289 add distributed

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --pretty=oneline
25912897008c90c9c731e2e5387c4c04d418900b (HEAD -> master) add distributed
d0b7869922da73260e1a4a4bc18bd21efff1b695 add mygit.md

```

最新的那个版本`distributed under the GPL`已经看不到了！

想再返回到原先的最新版本，怎么办？

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git reset --hard 878952
HEAD is now at 8789524 distributed under the GPL

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --pretty=oneline
87895244058aed768e26a31e549764235e776b4c (HEAD -> master) distributed under the GPL
25912897008c90c9c731e2e5387c4c04d418900b add distributed
d0b7869922da73260e1a4a4bc18bd21efff1b695 add mygit.md

```

`898752`是`distributed under the GPL`版本的`ID`。

`Git`的版本回退速度非常快，因为`Git`在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，`Git`仅仅是把`HEAD`从指向`distributed under the GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ distributed under the GPL
        │
        ○ add distributed
        │
        ○ add mygit.md
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ distributed under the GPL
   │    │
   └──> ○ add distributed
        │
        ○ add mygit.md
```

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`distributed under the GPL`，就必须找到`distributed under the GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git reflog
8789524 (HEAD -> master) HEAD@{0}: reset: moving to 878952
2591289 HEAD@{1}: reset: moving to HEAD^
8789524 (HEAD -> master) HEAD@{2}: commit: distributed under the GPL
2591289 HEAD@{3}: reset: moving to HEAD^
4aa20e8 HEAD@{4}: commit: add distributed
2591289 HEAD@{5}: commit: add distributed
d0b7869 HEAD@{6}: commit (initial): add mygit.md
```

小结：

- `HEAD`指向的版本就是当前版本，因此，`Git`允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 4.2 工作区与暂存区

工作区`（Working Directory）`是`/d/mygit`,版本库`（Repository）`是工作区的隐藏目录`/d/mygit/.git`。

`Git`的版本库里存了很多东西，其中最重要的就是称为`stage`（或者叫`index`）的暂存区，还有`Git`为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![git-repo](git简要手册.assets/0)

前面讲了我们把文件往`Git`版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建`Git`版本库时，`Git`自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

俗话说，实践出真知。现在，我们再练习一遍，先对`mygit.md`做个修改，比如加上一行内容：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后，在工作区新增一个`license`文本文件。

先用`git status`查看一下状态：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi license

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        license

no changes added to commit (use "git add" and/or "git commit -a")
```

`Git`非常清楚地告诉我们，`mygit.md`被修改了，而`license`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`mygit.md`和`LICENSE`都添加后，用`git status`再查看一下：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   license
        modified:   mygit.md
```

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区`（Stage）`，然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。一旦提交后，如果你又没有对工作区、暂存区做任何操作，那么工作区、暂存区就是“干净”的。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "modify mygit.md, add license"
[master 5e8fdd2] modify mygit.md, add license
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 license

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
nothing to commit, working tree clean
```

## 4.3 管理修改

现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么`Git`比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

为什么说`Git`管理的是修改，而不是文件呢？我们还是做实验。第一步，对`mygit.md`做一个修改，比如加一行内容：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
```

然后，添加：

```
$ git add mygit.md
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   mygit.md
```

然后，再修改`mygit.md`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
```

提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "track 1st change"
[master 5484309] track 1st change
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")
```

咦，怎么第二次的修改没有被提交？

别激动，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，`Git`管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- mygit.md`命令可以查看工作区和版本库里面最新版本的区别：

```BASH
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git diff HEAD -- mygit.md
diff --git a/mygit.md b/mygit.md
index 4b58cbb..0a70ff3 100644
--- a/mygit.md
+++ b/mygit.md
@@ -2,3 +2,4 @@ Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
 Git tracks 1st change.
+Git tracks 2nd change.
```

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

好，现在，把第二次修改提交了。

小结：

现在，你又理解了`Git`是如何跟踪修改的，每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

## 4.4 撤销修改

自然，你是不会犯错的。不过现在是凌晨两点，你正在赶一份工作报告，你在`mygit.md`中添加了一行：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
My boss likes stupid SVN.
```

在你准备提交前，一杯咖啡起了作用，你猛然发现了这可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")
```

你可以发现`Git`会告诉你`git restore <file>`可以丢弃工作区的修改：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git restore mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
```

命令`git restore <file>`意思就是，把`mygit.md`文件在工作区的修改全部撤销，这里有两种情况：

一种是`mygit.md`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`mygit.md`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。现在，看看`mygit.md`的文件内容已经复原。

对于第二种情况，经过`git restore --staged`和`git restore`后文件复原：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   mygit.md


Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
My boss likes stupid SVN.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git restore --staged mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
My boss likes stupid SVN.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git restore mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
```

现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得版本回退一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得`Git`是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，你就真的惨了……

小结：

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git restore <file>`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git restore --staged <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考*版本回退*一节，不过前提是没有推送到远程库。

## 4.5 删除文件

在`Git`中，删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git add test.txt
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "test.txt"
[master b660644] test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了。这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ rm test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ ll
total 2
-rw-r--r-- 1 Administrator 197121   8 Jun 24 09:03 license
-rw-r--r-- 1 Administrator 197121 182 Jun 24 10:20 mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git rm test.txt
rm 'test.txt'

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    test.txt


Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "remove test.txt"
[master 90e4aac] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
nothing to commit, working tree clean
```

现在，文件就从版本库中被删除了。

 小提示：先手动删除文件，然后使用`git rm <file>`和`git add <file>`效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ rm test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ ll
total 2
-rw-r--r-- 1 Administrator 197121   8 Jun 24 09:03 license
-rw-r--r-- 1 Administrator 197121 182 Jun 24 10:20 mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git restore test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
nothing to commit, working tree clean

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ ll
total 3
-rw-r--r-- 1 Administrator 197121   8 Jun 24 09:03 license
-rw-r--r-- 1 Administrator 197121 182 Jun 24 10:20 mygit.md
-rw-r--r-- 1 Administrator 197121  18 Jun 24 10:39 test.txt
```

`git restore`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt

nothing added to commit but untracked files present (use "git add" to track)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ rm test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git status
On branch master
nothing to commit, working tree clean
```

小结：

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

# 5、远程仓库

## 5.1 添加远程库

现在的情景是，你已经在本地创建了一个`Git`仓库后，又想在`GitHub`创建一个`Git`仓库，并且让这两个仓库进行远程同步，这样，`GitHub`上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，登陆`GitHub`，然后在右上角找到`“Create a new repo”`按钮，创建一个新的仓库。

![image-20210624104138747](Git简书.assets/image-20210624104138747.png)

目前，在`GitHub`上的这个`mygit`仓库还是空的，`GitHub`告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到`GitHub`仓库。

![image-20210624104300884](Git简书.assets/image-20210624104300884.png)

现在，我们根据`GitHub`的提示，在本地的`mygit`仓库下运行命令：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote add origin https://github.com/dk-zen/mygit.git

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git push -u origin master
Enumerating objects: 26, done.
Counting objects: 100% (26/26), done.
Delta compression using up to 8 threads
Compressing objects: 100% (20/20), done.
Writing objects: 100% (26/26), 2.08 KiB | 354.00 KiB/s, done.
Total 26 (delta 7), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (7/7), done.
To https://github.com/dk-zen/mygit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

添加后，远程库的名字就是`origin`，这是`Git`默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。下一步，就可以把本地库的所有内容推送到远程库上。

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，`Git`不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在`GitHub`页面中看到远程库的内容已经和本地一模一样：

![image-20210624104743707](Git简书.assets/image-20210624104743707.png)

## 5.2 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote -v
origin  https://github.com/dk-zen/mygit.git (fetch)
origin  https://github.com/dk-zen/mygit.git (push)
```

然后，根据名字删除，比如删除`origin`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote rm origin

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote -v

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到`GitHub`，在后台页面找到删除按钮再删除。

恢复绑定关系：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote add origin https://github.com/dk-zen/mygit.git

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote -v
origin  https://github.com/dk-zen/mygit.git (fetch)
origin  https://github.com/dk-zen/mygit.git (push)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git push -u origin master
Everything up-to-date
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

小结：

要关联一个远程库，使用命令`git remote add origin https://github.com/path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送`master`分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而`SVN`在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

## 5.3 从远程库克隆

从远程库`gitskills`克隆一个本地库。

```bash
Administrator@DELL-7390 MINGW64 /d
$ git clone https://github.com/dk-zen/gitskills.git
Cloning into 'gitskills'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

Administrator@DELL-7390 MINGW64 /d
$ cd gitskills/

Administrator@DELL-7390 MINGW64 /d/gitskills (main)
$ ll -ah
total 13K
drwxr-xr-x 1 Administrator 197121  0 Jun 24 11:09 ./
drwxr-xr-x 1 Administrator 197121  0 Jun 24 11:09 ../
drwxr-xr-x 1 Administrator 197121  0 Jun 24 11:09 .git/
-rw-r--r-- 1 Administrator 197121 11 Jun 24 11:09 README.md

Administrator@DELL-7390 MINGW64 /d/gitskills (main)
$ git remote -v
origin  https://github.com/dk-zen/gitskills.git (fetch)
origin  https://github.com/dk-zen/gitskills.git (push)

```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，`GitHub`给出的地址不止一个。`Git`支持多种协议，默认的`git://`使用`ssh`，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`。

小结：

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

`Git`支持多种协议，包括`https`，但`ssh`协议速度最快。

# 6、分支管理

## 6.1 创建与合并分支

在*版本回退*一节里，你已经知道，每次提交，`Git`都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在`Git`里，这个分支叫主分支，即`master`分支。

`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](Git简书.assets/0)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，`Git`新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](Git简书.assets/l)

你看，`Git`创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![image-20210624130157139](Git简书.assets/image-20210624130157139.png)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。`Git`怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![image-20210624130221546](Git简书.assets/image-20210624130221546.png)

所以`Git`合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![image-20210624130238120](Git简书.assets/image-20210624130238120.png)

真是太神奇了，你看得出来有些提交是通过分支完成的吗？

首先，我们创建`dev`分支，然后切换到`dev`分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git branch dev

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git branch
  dev
* master

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch dev
Switched to branch 'dev'

```

也可以用`git switch -c dev`代替`git branch dev`和`git switch dev`。`git branch`查看当前分支，`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`mygit.md`做个修改，加上一行，然后提交。现在，`dev`分支的工作完成，我们就可以切换回`master`分支。切换回`master`分支后，再查看一个`mygit.md`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git commit -m "branch test"
[dev dddd2d8] branch test
 1 file changed, 1 insertion(+)

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote -v
origin  https://github.com/dk-zen/mygit.git (fetch)
origin  https://github.com/dk-zen/mygit.git (push)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$
```

![image-20210624130308705](Git简书.assets/image-20210624130308705.png)

现在，我们把`dev`分支的工作成果合并到`master`分支上。`git merge`命令用于合并指定分支到当前分支。

合并后，再查看`mygit.md`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge dev
Updating 65cf7ff..dddd2d8
Fast-forward
 mygit.md | 1 +
 1 file changed, 1 insertion(+)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ cat mygit.md
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
Creating a new branch is quick.

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$
```

注意到上面的`Fast-forward`信息，`Git`告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

小结：

`Git`鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git switch <name>`

创建+切换分支：`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

## 6.2 解决冲突

人生不如意之事十之八九，合并分支往往也不是一帆风顺的。

准备新的`feature1`分支，继续我们的新分支开发.

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch -c feature1
Switched to a new branch 'feature1'
```

修改`mygit.md`最后一行，改为：

```
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (feature1)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (feature1)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (feature1)
$ git commit -m "and simple"
[feature1 4e62b87] and simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支，Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。

在`master`分支上把`mygit.md`文件的最后一行改为`Creating a new branch is quick & simple.`并提交。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (feature1)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git commit -m "& simple"
[master 1a874d9] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)

```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![image-20210624130359463](Git简书.assets/image-20210624130359463.png)

这种情况下，`Git`无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge feature1
Auto-merging mygit.md
CONFLICT (content): Merge conflict in mygit.md
Automatic merge failed; fix conflicts and then commit the result.

Administrator@DELL-7390 MINGW64 /d/mygit (master|MERGING)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")
```

果然冲突了！`Git`告诉我们，`mygit.md`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件。我们可以直接查看`mygit.md`的内容：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

`Git`用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Creating a new branch is quick and simple.
```

再提交：

```
Administrator@DELL-7390 MINGW64 /d/mygit (master|MERGING)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master|MERGING)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (master|MERGING)
$ git commit -m "conficts fixed"
[master 54950c0] conficts fixed

```

现在，`master`分支和`feature1`分支变成了下图所示：

![image-20210624130417436](Git简书.assets/image-20210624130417436.png)

用带参数的`git log`也可以看到分支的合并情况：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
*   commit 54950c0d08e16eae5e79f265cd41e4d788549678 (HEAD -> master)
|\  Merge: 1a874d9 4e62b87
| | Author: dk-zen <2005dingkai@sina.com>
| | Date:   Thu Jun 24 12:15:20 2021 +0800
| |
| |     conficts fixed
| |
| * commit 4e62b8706df8b51b7b0a572e5ed08c001f4805d5 (feature1)
| | Author: dk-zen <2005dingkai@sina.com>
| | Date:   Thu Jun 24 12:11:36 2021 +0800
| |
| |     and simple
| |
* | commit 1a874d9ac93e4a8c51aa09f1fcf1b2e313a3eb69
|/  Author: dk-zen <2005dingkai@sina.com>
|   Date:   Thu Jun 24 12:12:41 2021 +0800
|
|       & simple
|
* commit dddd2d80cbd2a919b870121827d1567bfbafa138 (dev)
| Author: dk-zen <2005dingkai@sina.com>
| Date:   Thu Jun 24 11:29:13 2021 +0800
|
git log --graph --pretty=oneline
*   54950c0d08e16eae5e79f265cd41e4d788549678 (HEAD -> master) conficts fixed
|\
| * 4e62b8706df8b51b7b0a572e5ed08c001f4805d5 (feature1) and simple
* | 1a874d9ac93e4a8c51aa09f1fcf1b2e313a3eb69 & simple
|/
* dddd2d80cbd2a919b870121827d1567bfbafa138 (dev) branch test
* 65cf7ff4a9e9beaf27f645ca9c729b43bc782ac2 (origin/master) add test.txt
* 90e4aacaf7efad8d760c3163377c92f319bdb4f1 remove test.txt
* b66064443dee1c8f0e1ac63fa1b5890d4dc9ee01 test.txt
* ed2ba06a19e614b5485a71cbb7eae3adca4a80d5 track 2nd change
* 548430975387d994595a673ac60db002691fd2bc track 1st change
* 5e8fdd25fab86b77b82c89ac277b14918a25f108 modify mygit.md, add license
* 87895244058aed768e26a31e549764235e776b4c distributed under the GPL
* 25912897008c90c9c731e2e5387c4c04d418900b add distributed
* d0b7869922da73260e1a4a4bc18bd21efff1b695 add mygit.md
```

最后，删除`feature1`分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git branch -d feature1
Deleted branch feature1 (was 4e62b87).
```

工作完成。

小结：

当`Git`无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把`Git`合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`或`git log --graph --pretty=oneline`命令可以看到分支合并图。

## 6.3 分支管理策略

通常，合并分支时，如果可能，`Git`会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，`Git`就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch dev
Switched to branch 'dev'
```

修改`mygit.md`文件，并提交一个新的`commit`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git commit -m "add --no-ff merge"
[dev 359fe72] add --no-ff merge
 1 file changed, 1 insertion(+)

```

现在，我们切换回`master`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 4 commits.
  (use "git push" to publish your local commits)

```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge --no-ff -m "merge --no-ff!" dev
Merge made by the 'recursive' strategy.
 mygit.md | 3 +--
 1 file changed, 1 insertion(+), 2 deletions(-)
```

因为本次合并要创建一个新的`commit`，所以加上`-m`参数，把`commit`描述写进去。

合并后，我们用`git log`看看分支历史：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
git log --graph --pretty=oneline --abbrev-commit
*   1b983d3 (HEAD -> master) merge --no-ff!
|\
| * a34e76a (dev) add git add mygit.md !
|/
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
| * 359fe72 add --no-ff merge
* |   54950c0 conficts fixed
|\ \
| * | 4e62b87 and simple
| |/
* / 1a874d9 & simple
|/
* dddd2d8 branch test
* 65cf7ff (origin/master) add test.txt
* 90e4aac remove test.txt
* b660644 test.txt
* ed2ba06 track 2nd change
* 5484309 track 1st change
* 5e8fdd2 modify mygit.md, add license
* 8789524 distributed under the GPL
```

可以看到，不使用`Fast forward`模式，`merge`后就像这样：

![image-20210624130449820](Git简书.assets/image-20210624130449820.png)

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![image-20210624130749071](Git简书.assets/image-20210624130749071.png)

小结：

`Git`分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

## 6.4 `bug`分支

软件开发中，bug就像家常便饭一样。有了`bug`就需要修复，在`Git`中，由于分支是如此的强大，所以，每个`bug`都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的`bug`的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
git switch dev
Switched to branch 'dev'

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git status
On branch dev
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   mygit.md

```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，`Git`还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被`Git`管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复`bug`，假定需要在`master`分支上修复，就从`master`创建临时分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 10 commits.
  (use "git push" to publish your local commits)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch -c issue-101
Switched to a new branch 'issue-101'
```

现在修复`bug`，需要把“`Git is free software ...`”改为“`Git is a free software ...`”，然后提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git commit -m "fix bug 101"
[issue-101 3962172] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 10 commits.
  (use "git push" to publish your local commits)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge --no-ff -m "merge bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 mygit.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git branch -d issue-101
Deleted branch issue-101 (was 3962172).
```

太棒了，原计划两个小时的`bug`修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch dev
Switched to branch 'dev'

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git status
On branch dev
nothing to commit, working tree clean

```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash list
stash@{0}: WIP on dev: a34e76a add git add mygit.md !

```

工作现场还在，`Git`把`stash`内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把`stash`内容也删了：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash pop
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (6e8bf8a2ea29bf85723eeecbba0032d0e042f055)

```

再用`git stash list`查看，就看不到任何`stash`内容了：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash list

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$

```

你可以多次`stash`，恢复的时候，先用`git stash list`查看，然后恢复指定的`stash`，用命令：

```
$ git stash apply stash@{0}
```

软件开发中，`bug`就像家常便饭一样。有了`bug`就需要修复。在`Git`中，由于分支是如此的强大，所以，每个`bug`都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的`bug`的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git status
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git add mygit.md

```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该`bug`，怎么办？

幸好，`Git`还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash
Saved working directory and index state WIP on dev: a34e76a add git add mygit.md !

```

现在，用`git status`查看工作区，就是干净的（除非有没有被`Git`管理的文件），因此可以放心地创建分支来修复`bug`。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支.现在修复`bug`，需要把“`Git is free software ...`”改为“`Git is a free software ...`”，然后提交：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch -c issue-101
Switched to a new branch 'issue-101'

Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ vi mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git add mygit.md

Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git commit -m "fix bug 101"
[issue-101 3962172] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)

```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (issue-101)
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 10 commits.
  (use "git push" to publish your local commits)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge --no-ff -m "merge bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 mygit.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```

太棒了，原计划两个小时的`bug`修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git switch dev
Switched to branch 'dev'

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git status
On branch dev
nothing to commit, working tree clean

```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash list
stash@{0}: WIP on dev: a34e76a add git add mygit.md !
```

工作现场还在，`Git`把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把`stash`内容也删了：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash pop
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   mygit.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (6e8bf8a2ea29bf85723eeecbba0032d0e042f055)
```

再用`git stash list`查看，就看不到任何`stash`内容了：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git stash list
```

你可以多次`stash`，恢复的时候，先用`git stash list`查看，然后恢复指定的`stash`，用命令：

```
$ git stash apply stash@{0}
```

在`master`分支上修复了`bug`后，我们要想一想，`dev`分支是早期从`master`分支分出来的，所以，这个`bug`其实在当前`dev`分支上也存在。

那怎么在`dev`分支上修复同样的`bug`？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的`bug`，要在dev上修复，我们只需要把`39621723e fix bug 101`这个提交所做的修改“复制”到`dev`分支。注意：我们只想复制`39621723e fix bug 101`这个提交所做的修改，并不是把整个`master`分支`merge`过来。

为了方便操作，`Git`专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```bash
$ git log --graph --pretty=oneline
*   97275b1f964abc95dbd7d39c2a732995417e209f (HEAD -> master) merge bug fix 101
|\
| * 39621723e03b3aca7c6aa7063f0cff14e51203c8 fix bug 101
|/
*   1b983d3915c1c6490e96f450abd79a320fbafb74 merge --no-ff!
|\
| * a34e76ab68f218112a19a211ddd190fd0f31c312 (dev) add git add mygit.md !
|/
* ae4badce6a2804958b962508dc62a50fd08c4f1e git add mygit.md
* e23af41e1a74ef5b37d69d5bff68635b956a8fcd another !
*   a38cc073920cf2e51135b96764be2978c7ef5d8c conflicts fixed
|\
| * 359fe7266e46a8c9af2fdc82240bfab1a05e7c69 add --no-ff merge
* |   54950c0d08e16eae5e79f265cd41e4d788549678 conficts fixed
|\ \
| * | 4e62b8706df8b51b7b0a572e5ed08c001f4805d5 and simple
| |/
* / 1a874d9ac93e4a8c51aa09f1fcf1b2e313a3eb69 & simple
|/
* dddd2d80cbd2a919b870121827d1567bfbafa138 branch test
* 65cf7ff4a9e9beaf27f645ca9c729b43bc782ac2 (origin/master) add test.txt
* 90e4aacaf7efad8d760c3163377c92f319bdb4f1 remove test.txt
* b66064443dee1c8f0e1ac63fa1b5890d4dc9ee01 test.txt

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git cherry-pick 39621723e0
Auto-merging mygit.md
[dev c91ab81] fix bug 101
 Date: Thu Jun 24 13:30:34 2021 +0800
 1 file changed, 1 insertion(+), 1 deletion(-)

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ cat mygit.md
Git is a distributed version control system.
Git is a free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks 1st change.
Git tracks 2nd change.
Creating a new branch is quick and simple.
add --no-ff merge!!!!!
test stash!

```

`Git`自动给`dev`分支做了一次提交，注意这次提交的`commit`是`c91ab81`，它并不同于`master`的`39621723e`，因为这两个`commit`只是改动相同，但确实是两个不同的`commit`。用`git cherry-pick`，我们就不需要在`dev`分支上手动再把修`bug`的过程重复一遍。

有些聪明的童鞋会想了，既然可以在`master`分支上修复`bug`后，在`dev`分支上可以“重放”这个修复过程，那么直接在`dev`分支上修复`bug`，然后在`master`分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从`dev`分支切换到`master`分支。

小结：

修复`bug`时，我们会通过创建新的`bug`分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复`bug`，修复后，再`git stash pop`，回到工作现场；

在`master`分支上修复的`bug`，想要合并到当前`dev`分支，可以用`git cherry-pick <commit_id>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

## 6.5 `feature`分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个`feature`分支，在上面开发，完成后，合并，最后，删除该`feature`分支。

现在，你终于接到了一个新任务：开发代号为`Vulcan`的新功能，该功能计划用于下一代星际飞船。

于是准备开发：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch -c featureVulan
Switched to a new branch 'featureVulan'

```

5分钟后，开发完毕：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ vi vulcan.py

Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ git add vulcan.py
warning: LF will be replaced by CRLF in vulcan.py.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ git commit -m "vulcan.py"
[featureVulan 57daea3] vulcan.py
 1 file changed, 1 insertion(+)
 create mode 100644 vulcan.py

```

切回`dev`，准备合并：

```
Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ git switch dev
Switched to branch 'dev'

```

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是！

就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch featureVulan
Switched to branch 'featureVulan'

Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ ll
total 4
-rw-r--r-- 1 Administrator 197121   8 Jun 24 09:03 license
-rw-r--r-- 1 Administrator 197121 265 Jun 24 13:42 mygit.md
-rw-r--r-- 1 Administrator 197121  18 Jun 24 10:39 test.txt
-rw-r--r-- 1 Administrator 197121   8 Jun 24 13:57 vulcan.py

Administrator@DELL-7390 MINGW64 /d/mygit (featureVulan)
$ git switch dev
Switched to branch 'dev'

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git branch -d featureVulan
error: The branch 'featureVulan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D featureVulan'.

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git branch -D featureVulan
Deleted branch featureVulan (was 744dfb4).

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$

```

销毁失败。`Git`友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

终于删除成功！

小结：

开发一个新`feature`，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

## 6.6 多人协作

当你从远程仓库克隆时，实际上`Git`自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`或者`git remote -v`显示更详细的信息：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote
origin

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git remote -v
origin  https://github.com/dk-zen/mygit.git (fetch)
origin  https://github.com/dk-zen/mygit.git (push)

```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

### 6.6.1 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，`Git`就会把该分支推送到远程库对应的远程分支上：

```
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- `bug`分支只用于在本地修复`bug`，就没必要推到远程了，除非老板要看看你每周到底修复了几个`bug`；
- `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在`Git`中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

### 6.6.2 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑或者同一台电脑的另一个目录下克隆。当你的小伙伴从远程库`clone`时，默认情况下，你的小伙伴只能看到本地的`master`分支。

```bash
Administrator@DELL-7390 MINGW64 /d
$ git clone https://github.com/dk-zen/mygit.git gitgit
Cloning into 'gitgit'...
fatal: unable to access 'https://github.com/dk-zen/mygit.git/': Failed to connect to github.com port 443: Timed out

Administrator@DELL-7390 MINGW64 /d
$ git clone https://github.com/dk-zen/mygit.git gitgit
Cloning into 'gitgit'...
remote: Enumerating objects: 62, done.
remote: Counting objects: 100% (62/62), done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 62 (delta 23), reused 59 (delta 20), pack-reused 0
Receiving objects: 100% (62/62), 5.43 KiB | 926.00 KiB/s, done.
Resolving deltas: 100% (23/23), done.

Administrator@DELL-7390 MINGW64 /d
$ ll
total 224
drwxr-xr-x 1 Administrator 197121 0 Sep 30  2019 '$RECYCLE.BIN'/
drwxr-xr-x 1 Administrator 197121 0 Dec 24 09:37  360Downloads/
drwxr-xr-x 1 Administrator 197121 0 Jun  8 14:33  360Rec/
drwxr-xr-x 1 Administrator 197121 0 Feb 19 15:09  BaiduNetdiskDownload/
drwxr-xr-x 1 Administrator 197121 0 Feb  4 13:11  CloudMusic/
drwxr-xr-x 1 Administrator 197121 0 May 11 11:29  DELLEMC_ISM/
drwxr-xr-x 1 Administrator 197121 0 Oct 16  2020  Download/
drwxr-xr-x 1 Administrator 197121 0 Jun 22 12:38  Fcode/
drwxr-xr-x 1 Administrator 197121 0 May 29  2019  FeigeDownload/
drwxr-xr-x 1 Administrator 197121 0 Sep 19  2018 'System Volume Information'/
drwxr-xr-x 1 Administrator 197121 0 Dec  2  2020  XNF_CRM/
drwxr-xr-x 1 Administrator 197121 0 Jun 23 15:39  anotherlearngit/
drwxr-xr-x 1 Administrator 197121 0 Jun 24 14:29  gitgit/
drwxr-xr-x 1 Administrator 197121 0 Jun 24 11:09  gitskills/
drwxr-xr-x 1 Administrator 197121 0 Apr 13  2020  goprojects/
drwxr-xr-x 1 Administrator 197121 0 Jun 23 15:17  learngit/
drwxr-xr-x 1 Administrator 197121 0 Dec 24 10:07  matlab2020b/
drwxr-xr-x 1 Administrator 197121 0 Jun 24 13:59  mygit/
drwxr-xr-x 1 Administrator 197121 0 Feb 20 12:38  mym/
drwxr-xr-x 1 Administrator 197121 0 Apr 25 09:38  rust/
drwxr-xr-x 1 Administrator 197121 0 Nov  9  2020  zpeng/
drwxr-xr-x 1 Administrator 197121 0 Nov  9  2020  zyxia/
drwxr-xr-x 1 Administrator 197121 0 Jun 23 10:46  国重lenovo部署/
drwxr-xr-x 1 Administrator 197121 0 Nov 11  2020  子午文章/
drwxr-xr-x 1 Administrator 197121 0 Jun  3 15:47  改造项目招标/
drwxr-xr-x 1 Administrator 197121 0 Nov  9  2020  迅雷下载/

Administrator@DELL-7390 MINGW64 /d
$ cd git
gitgit/    gitskills/

Administrator@DELL-7390 MINGW64 /d
$ cd git
gitgit/    gitskills/

Administrator@DELL-7390 MINGW64 /d
$ cd gitgit

Administrator@DELL-7390 MINGW64 /d/gitgit (master)
$ ll -ah
total 15K
drwxr-xr-x 1 Administrator 197121   0 Jun 24 14:29 ./
drwxr-xr-x 1 Administrator 197121   0 Jun 24 14:29 ../
drwxr-xr-x 1 Administrator 197121   0 Jun 24 14:29 .git/
-rw-r--r-- 1 Administrator 197121   9 Jun 24 14:29 license
-rw-r--r-- 1 Administrator 197121 252 Jun 24 14:29 mygit.md
-rw-r--r-- 1 Administrator 197121  18 Jun 24 14:29 test.txt

Administrator@DELL-7390 MINGW64 /d/gitgit (master)
$ git branch
* master

```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```bash
Administrator@DELL-7390 MINGW64 /d/gitgit (master)
$ git checkout -b dev origin/dev
Switched to a new branch 'dev'
Branch 'dev' set up to track remote branch 'dev' from 'origin'.

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ vi env.txt

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git add env.txt
warning: LF will be replaced by CRLF in env.txt.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git branch
* dev
  master

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git commit -m "add dev"
[dev ce012ca] add dev
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git push origin dev
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dk-zen/mygit.git
   c91ab81..ce012ca  dev -> dev

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$

```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ vi env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git add env.txt
warning: LF will be replaced by CRLF in env.txt.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git commit -m "add env"
[dev 7c5c711] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git push origin dev
To https://github.com/dk-zen/mygit.git
 ! [rejected]        dev -> dev (fetch first)
error: failed to push some refs to 'https://github.com/dk-zen/mygit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，`Git`已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git pull
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 313 bytes | 52.00 KiB/s, done.
From https://github.com/dk-zen/mygit
   c91ab81..ce012ca  dev        -> origin/dev
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev

```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.

```

再`pull`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git pull
Merge made by the 'recursive' strategy.

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git push origin dev
Enumerating objects: 2, done.
Counting objects: 100% (2/2), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 336 bytes | 336.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/dk-zen/mygit.git
   ce012ca..fb5d09f  dev -> dev

```

如果`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的*解决冲突*一节完全一样。解决后，提交，再`push`：

```bash
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

小结：

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## 6.7 `rebase`

在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后`push`的童鞋不得不先pull，在本地合并，然后才能push成功。

每次合并再`push`后，分支变成了这样：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git log --graph --pretty=oneline --abbrev-commit
*   fb5d09f (HEAD -> dev, origin/dev) Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
|\
| * ce012ca add dev
* | 7c5c711 add env
|/
* c91ab81 fix bug 101
* 3e1324f git stash
* a34e76a add git add mygit.md !
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
| * 359fe72 add --no-ff merge
* |   54950c0 conficts fixed
|\ \
| * | 4e62b87 and simple
| |/
* / 1a874d9 & simple
|/
* dddd2d8 branch test
* 65cf7ff add test.txt
* 90e4aac remove test.txt

```

总之看上去很乱，有强迫症的童鞋会问：为什么Git的提交历史不能是一条干净的直线？

其实是可以做到的！

Git有一种称为`rebase`的操作，有人把它翻译成“变基”。

我们还是从实际问题出发，看看怎么把分叉的提交变成直线。

在和远程分支同步后，我们对`env.txt`这个文件做了两次提交。用`git log`命令看看：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git log --graph --pretty=oneline --abbrev-commit
* ecc6d72 (HEAD -> dev) test rebase
* 7419572 test rebase
*   fb5d09f (origin/dev) Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
|\
| * ce012ca add dev
* | 7c5c711 add env
|/
* c91ab81 fix bug 101
* 3e1324f git stash
* a34e76a add git add mygit.md !
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
| * 359fe72 add --no-ff merge
* |   54950c0 conficts fixed
|\ \
| * | 4e62b87 and simple
| |/

```

注意到`Git`用`(HEAD -> dev)`和`(origin/dev)`标识出当前分支的HEAD和远程origin的位置分别是`ecc6d72 test rebase`和`fb5d09f iMerge branch 'dev'`，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git push origin dev
To https://github.com/dk-zen/mygit.git
 ! [rejected]        dev -> dev (fetch first)
error: failed to push some refs to 'https://github.com/dk-zen/mygit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

很不幸，失败了。`/d/gitgit`已经提交了新的推送。

```bash

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git pull
remote: Enumerating objects: 2, done.
remote: Counting objects: 100% (2/2), done.
remote: Total 2 (delta 1), reused 2 (delta 1), pack-reused 0
Unpacking objects: 100% (2/2), 316 bytes | 39.00 KiB/s, done.
From https://github.com/dk-zen/mygit
   ce012ca..fb5d09f  dev        -> origin/dev
Updating ce012ca..fb5d09f
Fast-forward

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ cat env.txt
env

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ vi env.txt

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git add env.txt
warning: LF will be replaced by CRLF in env.txt.
The file will have its original line endings in your working directory

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git commit -m "gitgit rebase"
[dev 713296f] gitgit rebase
 1 file changed, 1 insertion(+)

Administrator@DELL-7390 MINGW64 /d/gitgit (dev)
$ git push origin dev
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 347 bytes | 347.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dk-zen/mygit.git
   fb5d09f..713296f  dev -> dev

```

按照经验，先pull一下。这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的*解决冲突*一节完全一样。

再用`git status`看看状态。加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

解决后，提交，再`push`。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 327 bytes | 36.00 KiB/s, done.
From https://github.com/dk-zen/mygit
   fb5d09f..713296f  dev        -> origin/dev
Auto-merging env.txt
CONFLICT (content): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.

Administrator@DELL-7390 MINGW64 /d/mygit (dev|MERGING)
$ cat env.txt
env
<<<<<<< HEAD
rebase
rebase 2
=======
env rebase
>>>>>>> 713296f36d2bef52140289febd812bb8d3b2e3e0

Administrator@DELL-7390 MINGW64 /d/mygit (dev|MERGING)
$ vi env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (dev|MERGING)
$ git add env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (dev|MERGING)
$ git commit -m "fix the conflict during merging"
[dev 21daeb3] fix the conflict during merging

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git status
On branch dev
Your branch is ahead of 'origin/dev' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git push origin dev
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 1016 bytes | 338.00 KiB/s, done.
Total 9 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dk-zen/mygit.git
   713296f..21daeb3  dev -> dev

$ git log --graph --pretty=oneline --abbrev-commit
*   21daeb3 (HEAD -> dev, origin/dev) fix the conflict during merging
|\
| * 713296f gitgit rebase
* | ecc6d72 test rebase
* | 7419572 test rebase
|/
*   fb5d09f Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
|\
| * ce012ca add dev
* | 7c5c711 add env
|/
* c91ab81 fix bug 101
* 3e1324f git stash
* a34e76a add git add mygit.md !
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
| * 359fe72 add --no-ff merge
* |   54950c0 conficts fixed
|\ \
| * | 4e62b87 and simple
| |/

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
git rebase
Current branch dev is up to date.

Administrator@DELL-7390 MINGW64 /d/mygit (dev)
$ git switch master
Switched to branch 'master'
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git merge --no-ff -m "merge dev" dev
Merge made by the 'recursive' strategy.
 env.txt  | 4 ++++
 mygit.md | 3 ++-
 2 files changed, 6 insertions(+), 1 deletion(-)
 create mode 100644 env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --graph --pretty=oneline --abbrev-commit
*   6a52b04 (HEAD -> master) merge dev
|\
| *   21daeb3 (origin/dev, dev) fix the conflict during merging
| |\
| | * 713296f gitgit rebase
| * | ecc6d72 test rebase
| * | 7419572 test rebase
| |/
| *   fb5d09f Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
| |\
| | * ce012ca add dev
| * | 7c5c711 add env
| |/
| * c91ab81 fix bug 101
| * 3e1324f git stash
* | 1b983d3 merge --no-ff!
|\|
| * a34e76a add git add mygit.md !
|/
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
:

Administrator@DELL-7390 MINGW64 /d/mygit (master)
git rebase
dropping ce012cac332ef21a59217a648b69f9272ecdb1d4 add dev -- patch contents already upstream
error: could not apply 713296f... gitgit rebase
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 713296f... gitgit rebase
Auto-merging env.txt
CONFLICT (content): Merge conflict in env.txt

Administrator@DELL-7390 MINGW64 /d/mygit (master|REBASE 6/6)
$ git log --graph --pretty=oneline --abbrev-commit
* 0ebb8f4 (HEAD) test rebase
* f3f42b4 test rebase
* e56c668 add env
* 2381329 git stash
*   97275b1 (origin/master) merge bug fix 101
|\
| * 3962172 fix bug 101
|/
*   1b983d3 merge --no-ff!
|\
| * a34e76a add git add mygit.md !
|/
* ae4badc git add mygit.md
* e23af41 another !
*   a38cc07 conflicts fixed
|\
| * 359fe72 add --no-ff merge
* |   54950c0 conficts fixed
|\ \
| * | 4e62b87 and simple
| |/
* / 1a874d9 & simple
```

对强迫症童鞋来说，现在事情有点不对头，提交历史分叉了。

如果现在把本地分支`push`到远程，有没有问题？有！什么问题？不好看！有没有解决方法？有！

这个时候，`rebase`就派上了用场。我们输入命令`git rebase`试试。再用`git log`看看。

原本分叉的提交现在变成一条直线了！`rebase`操作前后，最终的提交内容是一致的，但是，我们本地的commit修改内容已经变化了。

这就是`rebase`操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

小结：

- `rebase`操作可以把本地未`push`的分叉提交历史整理成直线；
- `rebase`的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# 7、 标签管理

## 7.1 创建标签

在`Git`中打标签非常简单，首先，切换到需要打标签的分支上。然后，敲命令`git tag <name>`就可以打一个新标签。可以用命令`git tag`查看所有标签。默认标签是打在最新提交的`commit`上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的`commit id`，然后打上就可以了。

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
git branch
  dev
* master

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag v1.0

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag
v1.0

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --pretty=oneline --abbrev-commit
a1bfc71 (HEAD -> master, tag: v1.0, origin/master) Merge branch 'master' of https://github.com/dk-zen/mygit
6a52b04 merge dev
21daeb3 (origin/dev, dev) fix the conflict during merging
713296f gitgit rebase
ecc6d72 test rebase
7419572 test rebase
fb5d09f Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
7c5c711 add env
ce012ca add dev
c91ab81 fix bug 101

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag v0.9 7c5c711

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git log --graph --pretty=oneline --abbrev-commit
*   a1bfc71 (HEAD -> master, tag: v1.0, origin/master) Merge branch 'master' of https://github.com/dk-zen/mygit
|\
| *   97275b1 merge bug fix 101
| |\
| | * 3962172 fix bug 101
| |/
* |   6a52b04 merge dev
|\ \
| |/
|/|
| *   21daeb3 (origin/dev, dev) fix the conflict during merging
| |\
| | * 713296f gitgit rebase
| * | ecc6d72 test rebase
| * | 7419572 test rebase
| |/
| *   fb5d09f Merge branch 'dev' of https://github.com/dk-zen/mygit into dev
| |\
| | * ce012ca add dev
| * | 7c5c711 (tag: v0.9) add env
| |/
| * c91ab81 fix bug 101

```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
git show v0.9
commit 7c5c711741f6752df20b5450bf8e51dbf4b6817d (tag: v0.9)
Author: dk-zen <2005dingkai@sina.com>
Date:   Thu Jun 24 14:39:21 2021 +0800

    add env

diff --git a/env.txt b/env.txt
new file mode 100644
index 0000000..0a764a4
--- /dev/null
+++ b/env.txt
@@ -0,0 +1 @@
+env

```

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag -a v0.1 -m "v0.1 version" ce012ca

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git show v0.1
tag v0.1
Tagger: dk-zen <2005dingkai@sina.com>
Date:   Thu Jun 24 16:57:52 2021 +0800

v0.1 version

commit ce012cac332ef21a59217a648b69f9272ecdb1d4 (tag: v0.1)
Author: dk-zen <2005dingkai@sina.com>
Date:   Thu Jun 24 14:35:01 2021 +0800

    add dev

diff --git a/env.txt b/env.txt
new file mode 100644
index 0000000..0a764a4
--- /dev/null
+++ b/env.txt
@@ -0,0 +1 @@
+env

```

小结：

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个`commit id`；
- 命令`git tag -a <tagname> -m <msg>`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

## 7.2 标签管理

如果标签打错了，也可以删除：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag -d v0.1
Deleted tag 'v0.1' (was d7d012d)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git show v0.1
fatal: ambiguous argument 'v0.1': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'

```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dk-zen/mygit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/dk-zen/mygit.git
 * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除，然后从远程删除。删除命令也是`push`，但是格式如下：

```bash
Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git tag -d v0.9
Deleted tag 'v0.9' (was 7c5c711)

Administrator@DELL-7390 MINGW64 /d/mygit (master)
$ git push origin :refs/tags/v0.9
To https://github.com/dk-zen/mygit.git
 - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆`GitHub`查看。

小结：

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

# 8、使用`GitHub`

我们一直用`GitHub`作为免费的远程仓库，如果是个人的开源项目，放到`GitHub`上是完全没有问题的。`GitHub`还是一个开源协作社区，通过`GitHub`，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

在`GitHub`出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难。因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个`bug`，即使能改掉`bug`，也只能把`diff`文件用邮件发过去，很不方便。

但是在`GitHub`上，利用`Git`极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。

如何参与一个开源项目呢？比如人气极高的`bootstrap`项目，这是一个非常强大的`CSS`框架，你可以访问它的项目主页`https://github.com/twbs/bootstrap`，点“`Fork`”就在自己的账号下克隆了一个`bootstrap`仓库，然后，从自己的账号下`clone`：

```
git clone git@github.com:dk-zen/bootstrap.git
```

一定要从自己的账号下`clone`仓库，这样你才能推送修改。如果从`bootstrap`的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

`Bootstrap`的官方仓库`twbs/bootstrap`、你在`GitHub`上克隆的仓库`my/bootstrap`，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

```ascii
┌─ GitHub ────────────────────────────────────┐
│                                             │
│ ┌─────────────────┐     ┌─────────────────┐ │
│ │ twbs/bootstrap  │────>│  my/bootstrap   │ │
│ └─────────────────┘     └─────────────────┘ │
│                                  ▲          │
└──────────────────────────────────┼──────────┘
                                   ▼
                          ┌─────────────────┐
                          │ local/bootstrap │
                          └─────────────────┘
```

如果你想修复`bootstrap`的一个`bug`，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

如果你希望`bootstrap`的官方库能接受你的修改，你就可以在`GitHub`上发起一个`pull request`。当然，对方是否接受你的`pull request`就不一定了。

如果你没能力修改`bootstrap`，但又想要试一把`pull request`，那就`Fork`一下我的仓库：`https://github.com/michaelliao/learngit`，创建一个`your-github-id.txt`的文本文件，写点自己学习`Git`的心得，然后推送一个`pull request`给我，我会视心情而定是否接受。

小结：

- 在`GitHub`上，可以任意`Fork`开源仓库；
- 自己拥有`Fork`后的仓库的读写权限；
- 可以推送`pull request`给官方仓库来贡献代码。

