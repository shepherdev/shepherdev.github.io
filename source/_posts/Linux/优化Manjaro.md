---

thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/shepherdev.github.io@hexo/static/article/thumbnail/manjaro/manjaro-dog.jpg
title: 优化Manjaro
date: 2020-5-17 18:00
author: shepherd
categories: [操作系统,Linux]
toc: true
tags: [Linux,美化,系统优化]
---

为了让系统完美的契合自己，查了很多资料来完善它，写这篇文章记录下过程

<!-- more -->

## sh

首先就是将bash改成了zsh，使用主题ys，高亮插件和语法补全插件

参考文章[oh-my-zsh,让你的终端从未这么爽过](https://www.jianshu.com/p/d194d29e488c?open_source=weibo_search)

## mail

在使用crond时，总感觉manjaro的mail有点问题，重新弄了一下

下载s-nail、sendmail、procmail、m4

在使用过程中发现sendmail很卡顿，主要原因是`WARNING: local host name (MY) is not qualified; see cf/README: WHO AM I?`

查了下将/etc/hosts里的

```bash
127.0.0.1	localhost
```

改为了（MY是我的主机名）

```bash
127.0.0.1    localhost.localdomain localhost MY
```

这样启动和运行就快多了，原理就是sendmail在解析我的主机名，而我的主机名MY根本就找不到，所以浪费时间，在hosts里把主机名作为别名添加到IP后面即可

参考文章

[archwiki sendmail](https://wiki.archlinux.org/index.php/Sendmail_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

[linux sendmail 启动慢及mail 无法发送的问题一例](https://my.oschina.net/u/2503743/blog/786010)

## rsyslog

因为是看鸟哥的书学的linux，对rsyslog日志管理比较熟悉，而manjaro自带的是syslog，于是查wiki安装了rsyslog

首先卸载syslog-ng，以免冲突，然后下载rsyslog，注意它在AUR源里

为了从systemd提取日志

需要加载imjournal模块，在/etc/rsyslog.conf添加

```bash
$ ModLoad imjournal
```

然后转发journal，在/etc/systemd/jouranld.conf修改

```bash
ForwordToSyslog = yes
```

重启rsyslog服务即可

参考文章

[Archwiki-rsyslog](https://wiki.archlinux.org/index.php/Rsyslog)

> 记住要设置开机自启

下面是本机上的程序(不包括依赖)

```bash  program &gt;folded
acpi 1.7-3
acpid 2.0.32-1
aircrack-ng 1.6-2
alsa-firmware 1.2.1-2
alsa-utils 1.2.2-1
android-tools 29.0.6-1
android-udev 20200410-1
apparmor 2.13.4-4
archlinuxcn-keyring 20200226-1
ark 20.04.0-1
autoconf 2.69-7
automake 1.16.2-1
avahi 0.8+15+ge8a3dd0-1
b43-fwcutter 019-3
baidunetdisk-bin 3.0.1.2-7
bash-completion 2.10-1
bauh 0.9.0-1
bind-tools 9.16.2-2
binutils 2.34-2.1
bison 3.5.4-1
bluedevil 1:5.18.5-1
bluez-utils 5.54-2
btrfs-progs 5.6-1
bzip2 1.0.8-3
coreutils 8.32-1
cpupower 5.6-1
crda 4.14-3
cronie 1.5.5-1
cryptsetup 2.3.2-1
cups 2.3.3-1
cups-pdf 3.0.1-5
cups-pk-helper 0.2.6-3
deepin-wine-wechat 2.8.0.121-1
deepin.com.qq.office 2.0.0_4-2
device-mapper 2.02.187-2
dhclient 4.4.2-2
dhcpcd 9.0.2-1
dia 0.97.3-5
diffutils 3.7-3
dkms 2.8.1-2
dmidecode 3.2-1
dmraid 1.0.0.rc16.3-12
dnsmasq 2.81-4
dolphin 20.04.0-1
dolphin-plugins 20.04.0-1
dosfstools 4.1-3
e2fsprogs 1.45.6-2
eclipse-java 4.15-1
ecryptfs-utils 111-3
efibootmgr 17-1
exfat-utils 1.3.0-1
f2fs-tools 1.13.0-1
fakeroot 1.24-2
fcitx 4.2.9.7-3
fcitx-configtool 0.4.10-3
fcitx-googlepinyin 0.1.6-6
fcitx-qt5 1.2.4-4
ffmpeg 1:4.2.2-6
ffmpegthumbs 20.04.0-1
file 5.38-3
filelight 20.04.0-1
filesystem 2019.10-4
findutils 4.7.0-2
firefox 76.0.1-1
flatpak 1.7.2-1
flex 2.6.4-3
fwupd 1.4.0-1
gawk 5.1.0-1
gcc 9.3.0-1
gcc-libs 9.3.0-1
gettext 0.20.2-1
ghostscript 9.52-1
gimp 2.10.18-6
git 2.26.2-1
gitg 1:3.32.1+17+gb4c8155f-1
glibc 2.31-2
gnome-icon-theme 3.12.0-5
gnome-settings-daemon 3.36.1-1
gnome-themes-extra 3.28-1
google-chrome 81.0.4044.138-1
grep 3.4-1
grub 2.04-10
grub-theme-manjaro 18.1-1
gsfonts 20180524-2
gst-libav 1.16.2-1
gst-plugins-bad 1.16.2-9
gst-plugins-base 1.16.2-1
gst-plugins-good 1.16.2-3
gst-plugins-ugly 1.16.2-3
gtk-theme-breath 5.9.0-1
gtk3 1:3.24.20-1
gvfs 1.44.1-3
gvfs-afc 1.44.1-3
gvfs-gphoto2 1.44.1-3
gvfs-mtp 1.44.1-3
gvfs-nfs 1.44.1-3
gvfs-smb 1.44.1-3
gwenview 20.04.0-2
gzip 1.10-3
haveged 1.9.8-1
hostapd 2.9-3
hplip 1:3.20.3-2
htop 2.2.0-3
illyria-wallpaper 1.4-1
imagewriter 1.10.1420800585.134a9b3-4
inetutils 1.9.4-8
inkscape 1.0-3
intel-ucode 20191115-3
intellij-idea-community-edition 2:2020.1.1-1
inxi 3.0.37-1
iproute2 5.6.0-1
iptables 1:1.8.4-1.1
iputils 20190709-2
ipw2100-fw 1.3-10
ipw2200-fw 3.1-8
jdk 14.0.1-1
jfsutils 1.1.15-7
kaccounts-providers 20.04.0-1
kamera 20.04.0-1
kate 20.04.0-1
kcalc 20.04.0-1
kde-gtk-config 5.18.5-1
kde-servicemenus-rootactions 2.9.1-1
kdeconnect 20.04.0-1
kdegraphics-thumbnailers 20.04.0-1
kdenetwork-filesharing 20.04.0-1
kdenlive 20.04.0-1
kdeplasma-addons 5.18.5-1
keditbookmarks 20.04.0-1
kernel-alive 0.5-1
kfind 20.04.0-1
kgamma5 5.18.5-1
kget 20.04.0-1
khelpcenter 20.04.0-1
kimageformats 5.69.0-1
kinfocenter 5.18.5-1
kio-extras 20.04.0-1
kmenuedit 5.18.5-1
konsole 20.04.0-1
konversation 1.7.5-3
kscreen 5.18.5-1
kscreenlocker 5.18.5-1
ksshaskpass 5.18.5-1
ksysguard 5.18.5-1
ksystemlog 20.04.0-1
kwallet-pam 5.18.5-1
kwalletmanager 20.04.0-1
kwayland-integration 5.18.5-1
kwin 5.18.5-1
kwrited 5.18.5-1
latte-dock 0.9.11-1
less 551-3
lib32-flex 2.6.4-2
lib32-libcanberra 0.30+2+gc0620e4-3
lib32-libcanberra-pulse 0.30+2+gc0620e4-3
lib32-libva-intel-driver 2.4.0-1
lib32-libva-mesa-driver 20.0.6-2
lib32-libva-vdpau-driver 0.7.4-6
lib32-mesa-demos 8.4.0-1
lib32-mesa-vdpau 20.0.6-2
lib32-vulkan-intel 20.0.6-2
lib32-vulkan-radeon 20.0.6-2
libcanberra 0.30+2+gc0620e4-3
libcanberra-gstreamer 0.30+2+gc0620e4-3
libcanberra-pulse 0.30+2+gc0620e4-3
libdvdcss 1.4.2-2
libestr 0.1.11-1
libfastjson 0.99.8-2
libktorrent 2.1.1-1
liblogging 1.0.6-3
librelp 1.4.0-1
libtool 2.4.6+42+gb88cebd5-12
libva-intel-driver 2.4.0-1
libva-mesa-driver 20.0.6-2
libva-vdpau-driver 0.7.4-4
licenses 20200427-1
linux-firmware 20200424.r1632.b2cad6a-1
linux-lts-headers 1:5.4-1
linux-rt-lts-manjaro 5.4-1
linux-rt-lts-manjaro-headers 5.4-1
linux54 5.4.39-1
linux54-rt 5.4.28_rt19-1
linux54-rt-headers 5.4.28_rt19-1
logrotate 3.16.0-1
lsb-release 1.4-13
lvm2 2.02.187-2
m4 1.4.18-3
make 4.3-3
man-db 2.9.1-2
man-pages 5.06-2
manjaro-alsa 20200126-1
manjaro-application-utility 1.3.2-2
manjaro-bluetooth 20200126-1
manjaro-browser-settings 20200124-1
manjaro-documentation-en 20181009-1
manjaro-firmware 20160419-1
manjaro-hello 0.6.5-11
manjaro-hotfixes 2018.08-6
manjaro-kde-settings 20200430-1
manjaro-pulse 20200126-1
manjaro-release 20.0.1-1
manjaro-settings-manager-kcm 0.5.6-8
manjaro-settings-manager-knotifier 0.5.6-8
manjaro-system 20200427-1
manjaro-wallpapers-18.0 1.4-3
mdadm 4.1-2
memtest86+ 5.01-3
mesa-demos 8.4.0-4
mesa-vdpau 20.0.6-2
mhwd 0.6.4-2
mhwd-db 0.6.4-10
milou 5.18.5-1
mindmaster-cn 7.2-1
mkinitcpio-openswap 0.1.0-3
mobile-broadband-provider-info 20190618-1
modemmanager 1.12.10-1
mtpfs 1.1-3
mypaint 1.2.1-12
nano 4.9.2-1
net-tools 1.60.20181103git-2
netctl 1.23-1
netease-cloud-music 1.2.1-1
netease-cloud-music-gtk 1.1.1-1
networkmanager 1.24.0-1
networkmanager-openconnect 1.2.6-1
networkmanager-openvpn 1.8.12-1
networkmanager-pptp 1.2.9dev+10+gb41b0d0-1
networkmanager-vpnc 1.2.7dev+20+gdca3aea-1
nfs-utils 2.4.3-2
noto-fonts 20190926-4
noto-fonts-cjk 20190409-1
noto-fonts-emoji 20191016-6
nss-mdns 0.14.1-2
ntfs-3g 2017.3.23-4
ntp 4.2.8.p14-1
numlockx 1.2-5
obs-studio 25.0.8-1
okular 20.04.0-1
openresolv 3.10.0-1
openssh 8.2p1-3
os-prober 1.77-1
oxygen 5.18.5-1
oxygen-icons 1:5.69.0-1
p7zip 16.02-5
pacman 5.2.1-4
pamac-cli 9.5.0-1
pamac-gtk 9.5.0-1
pamac-snap-plugin 9.5.0-1
pamac-tray-appindicator 9.5.0-1
partitionmanager3 3.3.1-1
patch 2.7.6-8
patchutils 0.3.4-3
pavucontrol 1:4.0-1
pciutils 3.6.4-1
peek 1.5.1-2
perl 5.30.2-1
perl-file-mimeinfo 0.29-2
phonon-qt5-gstreamer 4.10.0-1
pinta 1.6-4
pkgconf 1.6.3-4
plasma-desktop 5.18.5-1
plasma-nm 5.18.5-1
plasma-pa 5.18.5-1
plasma-workspace 5.18.5-1
plasma-workspace-wallpapers 5.18.5-1
plasma5-themes-breath 0.4.0-2
poppler-data 0.4.9-2
powerdevil 5.18.5-1
powertop 2.12-1
print-manager 20.04.0-1
procmail 3.22-10
procps-ng 3.3.16-1
psmisc 23.3-2
pulseaudio-ctl 1.67-1
pulseaudio-zeroconf 13.0-3
pycharm-community-edition 2020.1.1-1
pygtk 2.24.0-13
python-lxml 4.5.0-1
python-pillow 6.2.1-1
python-pip 20.0.2-1
python-pygame 1.9.6-3
python-pyqt5 5.14.2-1
python-pysmbc 1.0.20-1
python-pyxdg 0.26-6
python-reportlab 3.5.42-1
python-xlib 0.27-1
python2-pbkdf2 1.3-1
python2-pyric 0.1.6.3-1
qq-linux 2.0.0_b2_1082-1
qt5-imageformats 5.14.2-1
qt5-virtualkeyboard 5.14.2-1
quadrapassel 3.36.02-1
rar 5.9.0-1
reiserfsprogs 3.6.27-3
roguehostapd-git 1.0.r45.g381b373-1
rsync 3.1.3-3
rsyslog 8.2004.0-1
ruby 2.7.1-2
s-nail 14.9.19-1
samba 4.12.2-3
scapy 2.4.3-2
scrcpy 1.13-1
screenfetch 3.9.1-1
sddm 0.18.1-2
sddm-breath-theme 0.4.0-2
sddm-kcm 5.18.5-1
sed 4.8-1
sendmail 8.15.2-9
shadow 4.8.1-1
simplescreenrecorder 0.4.1-1
skanlite 2.1.0.1-2
sl 5.02-5
snapd 2.44.5-1
snapd-glib 1.54-1
spectacle 20.04.0-1
spectre-meltdown-checker 0.43-1
splix 2.0.0-14
sshfs 3.7.0-1
steam-manjaro 1.0.0.61-7
subversion 1.13.0-2
sudo 1.8.31.p1-1
sysfsutils 2.1.0-11
system-config-printer 1.5.12+33+g23b454ef-1
systemd-fsck-silent 239-1
systemd-kcm 1.2.1-5
systemd-sysvcompat 245.5-2
systemsettings 5.18.5-1
tar 1.32-3
terminus-font 4.48-1
tesseract 4.1.1-2
texinfo 6.7-3
thunderbird 68.8.0-1
timeshift 20.03.r0.gcecd294-1
tlp 1.3.1-2
traceroute 2.1.0-5
ttf-bitstream-vera 1.10-12
ttf-droid 20121017-7.2
ttf-inconsolata 1:3.000-2
ttf-indic-otf 0.2-9
ttf-liberation 2.1.0-1
typora 0.9.86-1
udiskie 2.1.1-1
udisks2 2.8.4-2
unarchiver 1.10.1-10
usb_modeswitch 2.6.0-2
usbutils 012-2
user-manager 5.18.5-1
util-linux 2.35.1-2.1
vde2 2.3.2-13
vi 1:070224-4
vim 8.2.0717-2
virtualbox 6.1.6-1
virtualbox-ext-vnc 6.1.6-1
virtualbox-guest-iso 6.1.6-1
virtualbox-host-dkms 6.1.6-1
virtualbox-sdk 6.1.6-1
visual-studio-code-bin 1.45.1-1
vlc 3.0.10-1
vokoscreen-git 3.0.2.13.g9e58d6fb-1
vulkan-intel 20.0.6-2
vulkan-radeon 20.0.6-2
wacom-utility 1.21-6
wallpapers-2018 1.2-1
wallpapers-juhraya 1.1-2
wget 1.20.3-3
which 2.21-5
whois 5.5.6-1
wireshark-qt 3.2.3-1
wpa_supplicant 2:2.9-7
wps-office-cn 11.1.0.9505-1
wps-office-mui-zh-cn 11.1.0.9505-1
xclip 0.13-2
xdg-desktop-portal 1.7.2-1
xdg-desktop-portal-kde 5.18.5-1
xdg-su 1.2.3-1
xdg-user-dirs 0.17-2
xdg-utils 1.1.3+19+g9816ebb-1
xf86-input-elographics 1.4.2-1
xf86-input-evdev 2.10.6-1
xf86-input-libinput 0.29.0-2
xf86-input-void 1.4.1-4
xf86-video-amdgpu 19.1.0-1
xf86-video-ati 1:19.1.0-1
xf86-video-intel 1:2.99.917+906+g846b53da-1
xf86-video-nouveau 1.0.16-1
xfsprogs 5.6.0-2
xmind 3.7.9+8update9-1
xorg-server 1.20.8-2
xorg-twm 1.0.10-2
xorg-xinit 1.4.1-1
xorg-xkill 1.0.5-2
yakuake 20.04.0-1
yaourt 1.9-1
yay 9.4.7-2
youdao-dict 1.1.0-2
zeal 1:0.6.1-1
zsh 5.8-1
```

## ventoy

是一个镜像工具，可以将u盘的iso文件直接读取使用

## mindmaster，xmind

制作脑图的工具

## Packet tracer用户登录闪退问题

安装`ttf-ms-fonts`解决