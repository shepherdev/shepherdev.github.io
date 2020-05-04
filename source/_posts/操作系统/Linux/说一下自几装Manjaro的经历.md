---
title: Manjaro
date: 2020-2-19 22:37:26
author: shepherd
categories: [操作系统,Linux]
toc: true
tags:
  - Linux
  - 装系统
---

说一下自几装Manjaro的经历

1.在分区时将/usr独立，导致开机启动提示/sbin/init does not exist，并且啥也不能输入，原因我还没弄懂

<!-- more -->

2.进入系统后，将亮度条拉到最低，最想不到的事出现了，黑屏（纯黑），我的电脑屏幕可不是A屏，我没想到这kde的开发者怎么坑，竟然把亮度最低弄到了黑屏QWQ，废了半天，终于找到了解决办法：

- 查了下官方文档，只能得到配置文件到家目录下的.config里
- 没多余的linux系统，我找到了我的U盘，插上进入shell挂载我的家目录进行除错
- 最开始我是在.config找kde开头的文件，emm只有两个，看了下没一个像亮度的
- 然后我仔细想了想，最开始用manjaro的时候，我一直没找到亮度条，原因就是它并不在显示里面，而是在电源管理里面，于是我换目标，找power开头的文件
- 我找到了里面的powermanagementprofilesrc(乖乖，名字真长)，看到brightness control，那一刻我感觉我看了上帝，看到设置值，果然是0，我改成10重启，终于看到了我熟悉的桌面QAQ