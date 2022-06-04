---
title: Git学习
date: 2021-07-31 12:19:35
tags: Git
---


## 1. Git基础

### 1.1 版本管理

#### 1.1.1 什么是版本管理

版本管理是一种记录文件变化的方式，以便将来查阅特定版本的文件内容。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731151949.png)

#### 1.1.2 人为维护文档版本的问题

1. 文档数量多且命名不清晰导致文档版本混乱

2. 每次编辑文档需要复制，不方便

3. 多人同时编辑同一个文档，容易产生覆盖




   ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731151953.png)

   

   ### 1.2 Git 是什么

   Git是一个版本管理控制系统（缩写VCS），它可以在任何时间点，将文档的状态作为更新记录保存起来，也可以在任何时间点，将更新记录恢复回来。

   

   ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731152128.png)

   ### 1.3 Git 安装

   [下载地址](https://git-scm.com/downloads) 

   在安装的过程中，所有选项使用默认值即可。

   

   ### 1.4 Git 基本工作流程

   | git仓库          | 暂存区             | 工作目录            |
   | ---------------- | ------------------ | ------------------- |
   | 用于存放提交记录 | 临时存放被修改文件 | 被Git管理的项目目录 |

   ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731151946.png)

   ### 1.5 Git 的使用

   #### 1.5.1 Git 使用前配置

   在使用 git 前，需要告诉 git 你是谁，在向 git 仓库中提交时需要用到。

   1. 配置提交人姓名：`git config --global user.name 提交人姓名`
   2. 配置提交人姓名：`git config --global user.email 提交人邮箱` 
   3. 查看git配置信息：`git config --list`   

   **注意**

   1. 如果要对配置信息进行修改，重复上述命令即可。

   2. 配置只需要执行一次。

   #### 1.5.2 提交步骤

   1. `git init` 初始化git仓库
   2. `git status` 查看文件状态
   3. `git add 文件列表` 追踪文件
   4. `git commit -m 提交信息`  向仓库中提交代码
   5. `git log` 查看提交记录

   #### 1.5.3 撤销

   - 用暂存区中的文件覆盖工作目录中的文件： `git checkout 文件`

   - 将文件从暂存区中删除： `git rm --cached 文件`
   - 将 git 仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录：`git rest --hard commitID` 

   ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731151943.png)

   ## 2. Git进阶

   ### 2.1 分支

   为了便于理解，大家暂时可以认为分支就是当前工作目录中代码的一份副本。

   使用分支，可以让我们从开发主线上分离出来，以免影响开发主线。

   ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731151941.png)

   

   #### 2.1.1 分支细分

   1. 主分支（master）：第一次向 git 仓库中提交更新记录时自动产生的一个分支。

      

      ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731152137.png)

      

   2. 、开发分支（develop）：作为开发的分支，基于 master 分支创建。

      

      ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731152139.png)

   3. 功能分支（feature）：作为开发具体功能的分支，基于开发分支创建

      

      ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731152144.png)

   **功能分支 -> 开发分支 -> 主分支**

   #### 2.1.2 分支命令

   - `git branch` 查看分支
   - `git branch 分支名称` 创建分支
   - `git checkout 分支名称` 切换分支 (切换分支之前需要提交)
   - `git merge 来源分支` 合并分支（如果存在两个分支，dev和master，如果想要将dev分支的内容合并到master分支上，此时首先要回到master分支，然后再使用命令 git merge dev，而且dev分支依旧存在） PS：之后还需要 `git push` 将代码推送到云端。
   - `git push -u origin 分支名称` 首先要在本地切换到分支，然后执行该命令，在远程新建分支并推送分支的代码。
   - `git branch -d 分支名称` 删除分支（分支被合并后才允许删除）（-D 强制删除）


   ### 2.2 暂时保存更改

   在git中，可以暂时提取分支上所有的改动并存储，让开发人员得到一个干净的工作副本，临时转向其他工作。

   使用场景：分支临时切换

   - 存储临时改动：`git stash` 

     使用git add后将文件添加到暂存区后，还没有commit提交。此时使用git stash，将当前分支所有改动剪切到git的剪切板中，文件会回到之前的状态。如果使用git status，会发现显示的是noting to commit，因此便可以切换分支。

     ![image-20210731224217165](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731224217.png)

   - 恢复改动：`git stash pop`

     ![image-20210731224359238](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731224421.png)

