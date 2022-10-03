# Git概述

- 什么是Git？

  Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的**项目版本**管理。也是Linus Torvalds为了帮助管理Linux内核开发而开发的一个开放源码的版本控制软件。

- Git和GitHub的关系：

  Git只是一个版本控制的工具，而GitHub存放开源代码的一个平台。

### Git对象模型

![](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/git对象模型.png)

Git中有四类对象，分别为：

- blob： 用来存储文件数据，通常是一个文件。
- tree： 有点像一个目录，它管理一些“tree”或是 “blob”（就像文件和子目录）。
- commit： commit对象指向一个"tree"对象，他用来表示某次提交时的状态，它包括一些关于时间点的元数据，如时间戳、最近一次提交的作者、指向上次提交（commits）的指针等等。
- tag： 用来表示某个重要的commit。标记tag对象后就可以直观的通过tag名称来找到这个commit。



一个commit对象包含了必要的提交者信息、一个指向父提交的parent指针（除了首次提交）和一个指向tree对象的tree指针。一个tree对象中保存了目录结构信息和若干指向blob对象的指针。Blob对象中保存着实际的数据内容。

![](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/commit对象列表.png)

# Git 应用

Git在实际场景中，用到的命令并不是特别繁琐，最常见的有：clone、add、commit、push、pull、merge、checkout等等，下面会按照开发顺序对Git的命令进行讲解。本文对命令覆盖教全面，所以为了突出重点，在比较重要的地方会做特别说明。

## 配置Git和ssh

### Git

```
git config --global user.name "***"
git config --global user.email "***@gmail.com"
git config --global color.ui true
```

### SSH

在用户目录下` ssh-keygen -t rsa -C "***@gmail.com"`所有选项回车默认

然后在用户目录下会生成.ssh文件夹,里面会有id_rsa and id_rsa.pub

其中id_rsa.pub中就是公钥,另外一个文件中是私钥

然后将公钥添加到GitHub

![image-20220506094725740](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506094725740.png)

## 创建仓库

项目开始会创建一个存放当前项目各个版本项目代码的仓库，有两种方式去创建它，我们依次了解：

### 初始化本地文件夹为仓库

```bash
mkdir <repository name>    ## 建立本地仓库
cd <repository name>     ## 必须先进入，然后初始化
git init    ## 初始化该仓库
```

建立并初始化目录后，在当前目录下会生成一个`.git`的文件夹，在Windows端会显示，Linux端点开头文件为隐藏文件。然后需要在GitHub上新建一个和本地仓库同名的仓库（远程仓库），然后将远程仓库和本地仓库建立连接:

```
git remote add origin https://github.com/<github username>/<repository name>.git
```

- origin： 表示远程仓库再本地的别名，这个可以随便写，但是开发中默认都用origin。

上面的方式，就连接一下就可以了，实际上我们并不会自己去初始化本地仓库的。所以我们需要掌握下面的方法：

### 直接克隆远程仓库

可以直接在GitHub新建远程仓库之后，复制仓库链接，将远程仓库克隆到本地，这样子建立的仓库也不用再去大费周折的建立两个仓库的连接了。实际中我们也更多的采用这种方法。

# Git 工作流程

新建立仓库之后就可以进行开发了！我们先来了解以下我们新建的仓库吧！

### Git 工作区和暂存区

![img](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/1352126739_7909.jpg)

- 工作区： 工作区就是本地文件夹。

  ![image-20220505195753996](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220505195753996.png)

- 版本库

  版本库就是本地文件夹中的`.git`目录。版本库中包含了暂存区和主分支。

  ![image-20220505202505475](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220505202505475.png)

  - 暂存区(index)：顾名思义就是暂时存储被修改的文件。

  

  

  ![工作区暂存区版本库](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/bVbjImS)

当我们开始在工作区开发过程中，可以通过git将本地的代码上传到github进行保存，上图体现了一个完整的上传过程。比如在完成今日的工作之后需要同步代码：

```
git add <file name>    ## 向缓冲区添加一个文件
git commit -m "当次上传说明（建议填写且有意义）"    ## 提交到仓库
```

根据上图的描述，此时代码只是保存在了版本库里面。为什么要单独把前面两个写出来呢？因为add和commit是在本地进行的，并不需要进行联网。然后通过push推送到远程仓库：

```
git push origin <branch name>
```

当我们不知道我们当前的仓库有没有修改的时候可以使用下面的命令查看仓库的状态：

- `git status`  返回当前仓库的状态

  这里表示有一个修改没有add到分支

  ![image-20220506082959603](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506082959603.png)
- `git diff`  查看本地文件和仓库中的文件的不同

  ![image-20220506083140004](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506083140004.png)

在开发中经常会遇到新写的东西不满意，但是当前修改已经add或者commit了，这个时候就需要使用版本回退来恢复到之前的版本了。

## 版本回退

### 管理修改

Git管理的是每一次的修改，所以在开发中，完成一个小节点的时候，就建议进行一次add。那么要是add之后后悔了该怎么办呢？

- 撤销工作区修改

  ```
  git restore <file name>      # git-2.23版本引入，使用时请注意git版本
  git checkout -- <file name>
  # 二者效果相同
  ```

  但是这里要注意的是，对于新建的文件，不能使用以上命令对其进行修改撤回。

- 撤销暂存区的修改（已add但未commit）

  ```
  git restore --staged<file name>      # git-2.23版本引入，使用时请注意git版本
  git reset HEAD<file name>
  ```

  可以看到上面的撤销都是针对文件的，但是如果我们一次性修改了很多文件之后后悔了怎么办？还是建议先commit然后，看下面：

### 版本回退

`git log`  可以查看从最近到最远的提交日志，可以添加参数`--pretty=oneline` 使每条信息只显示一行

![image-20220506091949299](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506091949299.png)

版本回退首先要明白回退到哪个版本，在git中用HEAD表示当前的版本，所以上一个版本就是HEAD^，上上一个版本就是HEAD^^，往后就不会这么写了，写成HEAD~100

`git reset --hard HEAD^`  表示回退到上一个版本

那么回退到上一个版本之后后悔了怎么办呢？

`git reset --hard <之前的版本的commit号（输入前几位能区分就行）>`  就可以后悔了。

对于reset命令可以使用不同的参数来限制不同的回退力度，如果不指定回退方式，默认为mixed

| command           | Description                                          |
| ----------------- | ---------------------------------------------------- |
| git reset --mixed | 分支指针退回先前版本、还原暂存区、不更改工作区的代码 |
| git reset --soft  | 分支指针退回先前版本、不还原暂存区和工作区代码       |
| git reset --hard  | 分支指针退回**先前版本、并还原暂存区和工作区的代码** |

### 删除文件

先删除本地文件`rm <file name>` 此时`git status` 晓得你在工作区已经删除了此文件,他会给出两个选择:

1. 从版本库彻底删除

   `git rm <file name>`

2. 误删,需要恢复到本地工作区

   `git checkout --<file name>`  (过时)

   `git restore <file name>` 

## 远程仓库

在上面已经新建了一个远程仓库，并且克隆到了本地，那么在开发过程中，就应当合理对代码进行推送到远程仓库的操作。在此之前先看看对远程仓库的管理吧

### 管理远程仓库

- 与本地仓库建立连接

  ```
  git remote add origin https://gitee.com/klelee/learngit.git
  ```

- 与本地仓库取消连接

  ```
  # 首先查看与本地仓库相关联的远程仓库
  git remote -v
  # 然后删除需要删除的远程仓库
  git remote remove <remote name>    # 删除了本地库和远程库之间的联系,本地库和远程库本身都还是存在的,要删除真正的远程库,需要登陆到远程库进行删除.
  ```

### 推送

在推送的时候是遇到问题最多的，常见的问题有：

- 远程仓库与本地仓库存在冲突，远程仓库有的，本地没有
- 忘记add和commit（真的很容易忘）
- 网络问题
- ......

```
git push origin main
```



## 分支管理

Git中引入分支功能，旨在不同的分支做不同的事情，比如main分支就是用来发版本的，dev分支就是开发分支，还会有bug分支，feature分支等等。

### 创建分支

创建新的分支并切换到新分支

` git checkout -b dev ` 

![image-20220506102847181](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506102847181.png)

`git branch`它会列出所有的分支，并在当前分支前面用星号标示

![image-20220506102921395](https://klelee-image.oss-cn-qingdao.aliyuncs.com/image/image-20220506102921395.png)

这样，之后的所有操作就是在dev分支上进行了

当在新分支上的工作结束时，要将新的分支合并到main分支上，切换回到master分支`git checkout master`合并两个分支`git merge dev`

合并之后删除新的分支`git branch -d dev`

#### git switch

git在新版本中提供了新的切换分支的方式

`git switch -c dev` 可以创建并切换到dev分支

分支之间的切换`git switch dev`

### 分支操作

- 删除分支：`git branch -d <branch name>`
- 重命名分支： `git branch -m <oldname> <newname>`

### 解决冲突

假设在新的分支`feature1`上做了修改并添加提交，然后回到master分支，此时git还会提示我们当前的master比远程的master分支要超前一个提交，此时在master上修改。然后，master和新的分支feature1都有了新的提交，这种情况下git无法快速合并，若试图把各自的修改合并起来就会产生冲突。

所以，在遇到这种情况的时候就需要在文件中手动修改了。

### 分支管理策略

#### 强制禁用 Fast forword模式

在禁用fast froword的情况下，git在merge的时候就会生成一个新的commit，这样，在分支历史上就可以看出分支信息。

```
git switch -c dev
## 操作
git add <file>
git commit -m "reason"
git switch master
git merge --no-ff -m "merge with no-ff" dev ## 禁用fast forword的合并
```

在上面合并中，使用-m参数做上传说明使用为no-ff上传本身就带有一次commit

### bug分支

假设你在工作中突然接到命令需要修改一个其他的bug，此时？？？

首先，需要将现在手头的工作放下：`git stash` 可以将当前的进度保存

然后你就可以去你需要修改bug的分支，创建一个新的分支去修改bug了，加入你的bug在master分支上：

```
git switch master
git switch -c bug
## 修改bug
## bug修改完成
git switch master
git merge --no-ff -m "fixed bug" dev
git branch -d bug
```

然后返回到你之前工作的分支，用`git stash list`可以查看你保存的工作进度，

使用：

```

git stash apply ## 可以恢复你的工作进度，但是不会删除内存上存储的你的进度，需要使用
git stash drop  ## 释放存在内存上你的进度

git stash pop  ## 可以直接从内存中提取出你的工作进度
```

反思：既然master分支上出现了bug，那么早期的dev分支的代码应该也有bug才对，那么早期dev分支的bug怎么办呢？

这里只需要把刚刚在master修改bug的commit复制到dev，只复制他的修改。这里git提供了一个cherry-pick命令，可以复制一个特定的提交到当前分支

### feature分支

开发新功能需要开辟的新的分支

在新分支中，对于已经添加并提交的修改，要删除需要强制删除使用参数-D

### 多人协作

#### 推送分支

`git push origin master`  主分支，时刻与远程同步

`git push origin dev`  开发分支，团队需要在dev分支上面工作，因此需要远程同步

bug分支，只用于修复bug不用同步

feature分支要不要推送取决于新功能你一个人能都开发的了

#### 抓取分支

`git pull` 多人协作时，当你和你的伙伴同时对一个文件进行修改，你的伙伴先push，那么你就得先将最新的修改pull下来，在本地进行合并，如果合并有冲突，先解决冲突，然后进行提交

## 标签管理

### 创建标签

`git tag <tag name>` 创建一个新标签，在当前分支的最近一次提交上

`git tag <tag name> <commit code>`  创建一个新标签，在当前分支的指定的commit上

`git tag` 查看所有标签

`git show <tag name>` 查看标签信息

### 操作标签

如果一个标签打错了，可以删除`git tag -d <tag name>`

推送某个标签到远程`git push origin <tag name>`

或者一次性将所有标签都推送到远程`git push origin --tags`

但是如果标签已经推送到远程再想要删除，就得先删除本地标签，然后再删除远程标签`git push origin :refs/tags/<tag name>`







