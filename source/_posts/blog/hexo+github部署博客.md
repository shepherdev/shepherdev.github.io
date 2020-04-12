---
title: Hexo+GitHub搭建博客
date: 2019-12-20 19:37:26
author: shepherd
img: 
top: false
cover: false
coverImg: 
password:
toc: true
mathjax: false
summary: 
categories: 博客
tags:
  - hexo
  - github
---

*本教程只适用`windows`系统*

##  GitHub创建个人仓库 

- 注册一个[GitHub]( https://github.com/ )账户，创建一个仓库，仓库名字固定写法为

`YouName(你的用户名).github.io`

##  安装git

-  [Git]( https://git-scm.com/download/win )是开源的分布式版本控制系统，用于敏捷高效地管理项目 
-  我们网站在本地搭建好了，需要使用`Git`同步到`GitHub`上 
- 安装完成后在命令行输入`git`会显示版本信息
  - 如果没有，则说明安装失败
- 安装完成后，在桌面右键打开`Git Bash`或者去菜单里找到`Git Bash`并打开它

 qnimg 1.png 

- 输入下面的配置信息

`git config --global user.name "你的GitHub用户名"`
`git config --global user.email "你的GitHub注册邮箱"`

-  生成ssh密钥文件 

`ssh-keygen -t rsa -C "你的GitHub注册邮箱"`

输入这条指令后连按回车（编辑中...）



参考知乎文章：[GitHub+Hexo 搭建个人网站详细教程]( https://zhuanlan.zhihu.com/p/26625249?utm_source=com.tencent.tim&utm_medium=social&utm_oi=881261714933415936)
