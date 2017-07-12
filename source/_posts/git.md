---
title: Git命令整理
tags:
  - Git
date: 2017-07-12 21:29:36
---

## 前言碎碎念
自从使用Git作为版本控制工具以来，通过教程学习、手册查阅方式了解了Git原理和Git命令，能够顺利使用。但由于还不熟练，实践经验也还不够丰富，每次遇到问题都需要重新搜索，多次下来十分麻烦。另一方面，查阅手册往往是不够的，因为手册只会告诉你什么命令做什么用，不会根据不同场景告诉你应该用什么命令。

所以在这篇文章中，将常用的Git命令根据不同的使用场景做一个整理，加深印象的同时也方便自己日后查阅。
***

## 四个概念
借用阮一峰老师文章[《常用Git命令清单》](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)中的图。
![四个概念和常用命令](/images/git_basicConcept.png)

- Workspace: 工作区，也就是正在编辑的文件目录
- Index / Stage: 暂存区
- Repository: 本地仓库，.git文件夹管理的版本库
- Remote: 远程仓库，例如github.com上的仓库

例如，最常用的命令串中：
```bash
$ git add <file>
#添加工作区指定文件的改动到暂存区，"<file>"为"."时添加全部改动

$ git commit -m "XXXX"
#提交暂存区的所有内容到本地仓库的当前分支

$ git push
#上传本地仓库到已关联的远程仓库
```
***
## 建立工程
在工作目录中建立与远程仓库关联的Git工程主要有两种情况：第一是由本地上传到远程仓库；第二是从远程仓库克隆到本地。

### 本地上传
这种情况下，远程应该是没有工程的。在本地工程文件夹下：
```bash
$ git init
#此时会增加.git文件夹，当前文件夹受到Git管理，并默认创建master分支

$ git remote add origin <url>
#为当前项目添加远程主机。其中origin为自定义的远程主机名，url为远程主机地址（推荐采用ssh协议）
```
此时已经建立了本地仓库与远程仓库的关联，可以通过`git push`推送上传。
第一次推送采用：
```bash
$ git push -u origin master
#将本地master分支推送到远程同名分支（若不存在则新建），同时-u指定origin为默认主机名，之后若要上传到origin可省略
```

### 远程克隆
这种情况下，远程仓库已经有工程，只需要在本地工程文件夹下用`git clone`命令克隆：
```bash
$ git clone <url>
```
此时本地仓库已经与对应远程仓库建立关联，<url>为主机名origin的地址。

#### 克隆其他分支
`git clone`命令默认克隆远程项目的master分支及其历史，若还需克隆别的分支，可通过以下方式进行（以克隆dev分支为例）：
```bash
$ git checkout -b dev origin/dev
#检出origin下的dev分支到本地新建的dev分支，并建立本地分支与远程分支的追踪关系
```
或
```bash
$ git checkout -b dev
#新建并切换到本地分支dev

$ git branch --set-upstream-to=origin/dev dev
#建立origin/dev远程分支和dev本地分支的追踪关系

$ git pull
#拉取本地分支dev对应的远程分支的最新状态
```

#### 托管到新的远程仓库
在克隆需要的内容后，有时会希望托管到新的远程仓库。
此时可以增加新的远程主机名:
```bash
$ git remote add <new_remote_name> <url>
```
或者干脆更改原来origin的地址：
```bash
$ git remote origin set-url <url>
```
***

## 分支管理

### 查看分支
```bash
$ git branch
#查看本地分支

$ git branch -r
#查看远程分支

$ git branch -a
#查看所有本地分支和远程分支
```

### 新建本地分支
新建分支（不切换）：
```bash
$ git branch <new_branch>
```

新建分支并切换到新分支：
```bash
$ git checkout -b <new_branch>
#相当于：
$ git branch <new_branch>
$ git checkout <new_branch>
```

### 删除本地分支
```bash
$ git branch -d <branch>
#删除分支前检查该分支是否有未提交或者未合并的内容

$ git branch -D <branch>
#强制删除该分支
```

### 新建远程分支
相当于把远程未添加的本地分支push到远程：
```bash
$ git push origin <local_branch>:<remote_branch>
#建议远程与本地分支同名，同名时可省略远程分支名
```

### 删除远程分支
相当于push一个本地的空分支：
```bash
$ git push origin :<remote_branch>
#本地分支为空
```
或者使用`--delete`：
```bash
$ git push origin --delete <remote_branch>
```

### 合并分支
```bash
$ git merge <branch>
#快进合并，合并<branch>到当前分支
```
***
## 撤销与版本回退
待续...
***
## 多人协作
待续...
***
## 其他
待续...
