---
title: wifiphisher安装与使用
date: 2020-4-30
author: shepherd
toc: ture
categories: [操作系统,Linux]
tags: [钓鱼,wifi,manjaro,wifiphisher]
---

Wifiphisher是用于进行红队参与或Wi-Fi安全测试的恶意访问点框架。使用Wifiphisher，渗透测试人员可以通过执行有针对性的Wi-Fi关联攻击轻松地在针对无线客户端的中间人位置。Wifiphisher可以进一步用于对连接的客户端发起受害者自定义的网络钓鱼攻击，以捕获凭据（例如，从第三方登录页面或WPA / WPA2预共享密钥）或用恶意软件感染受害者站点。

<!-- more -->

曾使用字典暴力破解wifi，花费时间太长，而且不一定成功（与字典有关），所以尝试一下钓鱼

安装与使用前提：

- Kali Linux 或者其他linux发行版；
- 一个支持AP模式的无线网络适配器，驱动要求支持netlink；
- 一个支持Monitor模式并且能够实现命令注入的无线网络适配器，驱动要求支持netlink。如果你没有这种适配器，那么你在运行WiFiPhisher时请使用—nojamming选项；

## 安装

官方推荐在kali上使用wifiphisher，本着学习精神，我决定在我的manjaro上尝试安装

在我的manjaro折腾了一天(虽然软件管理pacman有wifiphisher，但是因为python2-scapy的依赖问题导致构建不成功)。于是从github上下载1.4版本，从python2到python3，从pip2到pip3，自己手动下载各种依赖:angel:，最后发现不是依赖的问题，是发布的wifiphisher-1.4版本貌似只支持python2，但使用python2安装后，报错

```bash
ImportError: cannot import name hostapd_controller
```

这就很奇怪，明明1.4已经说了解决了hostapd的问题。在lssue也没找到很好的解决方法（看英文看到头痛）

于是直接clone最新开发进度，使用python3进行安装

## 使用

在终端使用`wifiphisher`进入了界面，但是搜不到接入点

![wifiphisher](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wifiphisher.png)

可能是网卡的缘故吧，官方对网卡的要求

> 一个无线网络适配器，支持AP＆Monitor模式并能够注入。驱动程序应支持netlink。

使用`iw list`查了下我的网卡

```bash
Supported interface modes:
    * IBSS
    * managed
    * AP
    * monitor
    * mesh point
    * P2P-client
    * P2P-GO
    * P2P-device
```

可以看到是支持AP和监听的，难道是不能注入？

尝试使用指令查看是否支持注入，结果

```bash
➜  ~ aireplay-ng -9 wlp1s0      
11:02:52  Trying broadcast probe requests...
11:02:53  No Answer...
11:02:53  Found 0 APs
```

> manjaro很多关于网络的指令都没有，需要自行安装net-tools(ifconfig)，aircrack-ng(aireplay-ng)

正在摸索中，考虑网卡的原因，下单了一个3070芯片的网卡，插上后输入在我的电脑里的设备名是`wlp0s20f0u1`

再次使用wifiphisher

```bash
sudo wifiphisher -i wlp0s20f0u1
```

> 当只有一个网卡且这个网卡支持监听和AP时，可以使用`-i`手动生成恶意AP并进行攻击
>
> 如果有多余的网卡，使用`-eI`参数指定监听网卡，`-aI`指定生成AP的网卡，更多参数参见`wifiphisher -h`

成功了！

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wifiphisher-ap.png" style="zoom:33%;" />

选择攻击目标后的钓鱼方案

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wifiphisher-phis.png" style="zoom:33%;" />

攻击成功！

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wifiphisher-phis2.png" style="zoom: 33%;" />

手机连接后跳转界面

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wifiphisher-moban.png" style="zoom: 25%;" />

选择攻击方案的模板可自定义，在我的电脑上模板路径是`/usr/lib/python3.8/site-packages/wifiphisher-1.4-py3.8.egg/wifiphisher/data/phishing-pages`

## 声明

- 请不要在没有征得目标网络管理员许可的情况下利用WiFiPhisher实施攻击，这种行为将会被视作非法活动。笔者不承担任何责任，请谨慎使用。

参考文章

> [测试注入](https://blog.csdn.net/qq_28208251/article/details/48086249)
>
> [wifiphisher官方](https://wifiphisher.readthedocs.io/en/latest/?badge=latest)
>
> [多版本Python安装pip及pip版本管理终极教程](https://zhuanlan.zhihu.com/p/37473690)
>
> [2to3 - 自动将 Python 2 代码转为 Python 3 代码](https://docs.python.org/zh-cn/3/library/2to3.html)
>
> [aircrack-ng判断网卡是否兼容](https://www.aircrack-ng.org/doku.php?id=compatible_cards)

