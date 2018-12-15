---
title: git 常用命令操作
date: 2018-12-10 23:33:28
categories:
- git
tags:
---

  公司项目开发中经常使用git进行代码管理，记录常用问题以及遇到问题的解决方法。目前写下只是一部分印象较深的，以后会根据项目中遇到的问题进行解决并更新记录。
<!--more-->
## 基本操作
1. 平时代码上传拉取远程仓库并上传到远程仓库。
```
git add .
git commit -am "message"
git pull
git push origin branchName
```
2. 检查分支状态
```
git status
```
3. 分支切换
```
git checkout branchName
```
4. 撤销commit
```
git log
git reset --hard ^HEAD
```
5. 返回到相应的提交位置
```
git reset --hard commit_id
```
## 一般操作
1. 在新的环境下新建本地仓库并拉取远程。
 进入项目复制该项目的SSH路径,使用git bash或者cmd进入到改项目路径。
 ```
git clone git@...
git add .
git commit -am "first commit"
 ```
