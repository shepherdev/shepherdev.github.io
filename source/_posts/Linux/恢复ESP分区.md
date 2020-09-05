---
thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/thumbnail/manjaro/manjaro-cat.jpg
title: linux误删ESP分区?开机无法引导进linux?pacman -Syu后无法进入系统?
date: 2020-08-07 22:00
author: shepherd
categories: [操作系统,Linux]
toc: true
tags: [manjaro,esp,boot,双系统]
---

 此文章适合Manjaro+Windows10双系统&UEFI+GPT食用, 其他情况仅供参考.

<!-- more -->

博主在Win系统中误操作中格式化了引导Manjaro的ESP(`EFI System Partition`)分区, 这下好了, 开机只能进Win10.

虽然我知道备份数据+重装系统不失为一种好方法, 但身为一个年轻人, 不折腾折腾怎么行? 所以开始查资料恢复ESP, 由于不是很懂linux系统启动原理导致走了很多弯路, 整理成文章分享给大家. 

<!--文章分为操作部分和理论部分, 如果你想快点恢复ESP分区, [请点击跳转操作部分](##实际操作).-->

## 新建引导

操作前需要准备一点东西:

1. U盘
2. 一个多余的操作系统

### 制作livecd(U盘启动盘)

先去[Manjaro官网](https://manjaro.org/)下载iso镜像文件(或者镜像站下载, 这里推荐[清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/osdn/storage/g/m/ma/manjaro/)), 如果使用的是Win10操作系统, 建议使用[rufus](https://github.com/pbatard/rufus/tree/master/src)工具向U盘写入镜像; 如果是Linux操作系统, 使用dd指令即可. 如果是Android系统, 能接入U盘并识别的话, 可以考虑使用Termux的dd指令.

### 进入livecd

将U盘插入需要修复的电脑进入BIOS, 选择U盘启动, 进入临时的Manjaro系统.

### 挂载&chroot

打开终端输入以下指令自动挂载电脑上的Manjaro操作系统, 自动chroot.

```bash
sudo manjaro-chroot -a
## 正常情况等待提示输入'1'即可
```

> 注意, 如果是格式化esp分区(uuid发生变化), 会导致格式化的分区不能自动挂载, 有两种解决方案
>
> 1. 进入chroot后修改/etc/fstab修改格式化分区uuid后再次使用上述指令
> 2. 清楚你的linux系统所在分区, 并尝试手动挂载

因为我是格式化的esp分区, 不能自动挂载上efi分区, 我尝试的是手动挂载, 列出我挂载的分区

- /dev/nvme0n1p7  --> /mnt/Manjaro/
- /dev/nvme0n1p6 --> /mnt/Manjaro/boot/
- Restore_the_GRUB_Bootloader/dev/nvme0n1p8 --> /mnt/Manjaro/home/
- /dev/nvme0n1p9 --> /mnt/Manjaro/boot/efi/

手动挂载还需要输入以下指令

```bash
sudo mount -t proc proc /mnt/proc
sudo mount -t sysfs sys /mnt/sys
sudo mount -o bind /dev /mnt/dev
sudo mount -t devpts pts /mnt/dev/pts/
sudo modprobe efivarfs
sudo chroot /mnt
mount -t efivarfs efivarfs /sys/firmware/efi/efivars
```

### 重装并更新grub

```bash
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=manjaro --recheck
sudo update-grub
# 提示'未知的设备nvme0n1'不用管他
```

如果上述指令无误的话就可以重启了.

注意格式化esp分区的需要修改/etc/fstab文件的UUID, 不然重启会出现挂载超时提示, 进不了系统.

进入系统后再次输入`update-grub`, grub会识别到电脑的其他系统, 开机才会出现grub多重引导.

## pacman -Syu后无法进入系统

进入系统后, 熟练的输入了`pacman -Syu`(距离上一次更新2个月了), 提示失败, 尝试数次, 成功. 重启后...引导不见了? 并且系统黑屏, 不能切换tty.

粗略猜测内核错误, 使用U盘chroot后, 输入`pacman -S Linux`后再次重启, 成功进入系统. 

> 参考文章
>
> [Restore_the_GRUB_Bootloader](https://wiki.manjaro.org/index.php?title=Restore_the_GRUB_Bootloader)
>
> [第十九章、开机流程、模组管理与Loader](http://linux.vbird.org/linux_basic/0510osloader.php#process_1)

