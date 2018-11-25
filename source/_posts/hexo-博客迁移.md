---
title: Hexo 博客迁移
date: 2018-11-25 10:29:10
categories:
- hexo
tags:
- hexo
---

## Hexo 博客迁移管理
在公司电脑上部署后，博客只能在公司编辑，总有一点不方便。查找了一些资料，如何把博客迁移到自己的笔记本中。主要有两种方法，一个是新建仓库来放环境文件，另一个是通过分支进行管理。本文采用的是通过分支进行管理。现在记录下该过程，方便以后有需要的时候即时查看。**本文只记录迁移的过程**

<!--more-->

### 准备工作
1. github上已经搭建好相关仓库。

### 原电脑的操作步骤
1. 新建分支dev，该分支用来存储静待文件。
```
git checkout -b  dev
```
2. 将github 上的仓库下载到本地,删除这个文件夹所有的除了.git文件外的所有文件。
3. 将工作区的变化提交（包括删除文件）。
```
git add -A
git commit -m "message"
git push origin dev
```
4.复制.git文件到项目的根目录下。删除clone下来的文件夹。

### 新电脑上环境搭建
1. 安装node.js和git
2. 安装hexo
```
npm install -g hexo-cli
```
3. 下载仓库到本地
4. 安装依赖
```
npm install
```
5. 进入到themes下，下载主题
```
git clone https://github.com/iissnan/hexo-theme-next
```
### 同步
完成以上工作后，两台电脑就可以通过git命令同步。
目前流程可能有不完善的地方，以后碰到会对文章做出修改。

**参考**：[参考链接](https://www.jianshu.com/p/fceaf373d797)
