---
thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/thumbnail/manjaro/school-project.jpg
title: 在Manjaro上使用virtualbox
date: 2020-5-2
author: shepherd
toc: ture
categories: [操作系统,Linux]
tags: [manjaro,virtualbox,虚拟机]
---

### 安装问题

使用`pacman -S virtualbox`后，打开创建新的新的虚拟机出现错误

```bash
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (5.4.35-1-MANJARO) or it failed to
         load. Please recompile the kernel module and install it by

           sudo /sbin/vboxconfig

         You will not be able to start VMs until this problem is fixed.
```

<!-- more -->

说模块没有加载成功，需要使用`sudo /sbin/vboxconfig`修复，但实际上没有这个文件

参考[在 Arch 里安装 VirtualBox](https://wiki.archlinux.org/index.php/VirtualBox_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))这篇文章后，我安装了两个模块

1. virtualbox-host-dkms
2. linux-lts-headers

查看模块加载情况

```bash
# shepherd @ MY in ~ [14:04:17] C:1
$ sudo vboxreload                  
Unloading modules: 
Loading modules: vboxnetadp vboxnetflt vboxdrv 
```

然后重新打开virtualbox，运行正常

![virtualbox](https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/2020/vm-virtualbox.png)

