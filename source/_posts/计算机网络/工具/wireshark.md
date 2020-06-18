---
title: 使用wireshark分析TCP、UDP
date: 2020-05-29 20:00
thumbnail: https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/thumbnail/logo/Wireshark-logo.png
author: shepherd
toc: true
mathJax: true
categories: [计算机网络,工具]
tags: [TCP,UDP,wireshark]
---

 分析完理论知识后，不实践怎么知道是不是真的，wireshark就是一个非常方便的抓包工具，先用它来分析TCP和UDP

<!-- more -->

## 安装

我的是Manjaro系统，安装指令

```bash
pacman -Ss wireshark
```

## 使用

抓包涉及更高的权限，需要sudo使用

抓包界面

![](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-gui.png)

分为上中下三个窗口，最上面的窗口显示的是包的简略信息，中间是详细信息，下面是原始16进制数据。

## 分析TCP

wireshark将SEQ设置为相对，为了方便观察，也就是从0开始，不适和新手了解TCP协议，需要在TCP简略包上右键鼠标-->首选项-->relative sequence number关闭

使用过滤器可以过滤其他不必要的协议![wireshark-filter](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-filter.png)

通过给http给服务器上传文件进行抓包得到以下界面

![wireshark-tcp-1](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-tcp.png)

### 三次握手

![wireshark-tcp-1](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-tcp-1.png)

通过上图分析，我的计算机发送SYN标志tcp报文段，服务端回应了我SYN-ACK报文段，我的计算机回应ACK，三次握手建立连接完毕，然后开始发送数据（PSH表示不需要缓存，及时上交）。

- 连接成功后的**发送方和接收方的SEQ**=上一次SEQ+Len

此外我还发现了我的计算机发送的第一个tcp报文端的选项区

`SACK_PERM=1 TSval=3007114814 TSecr=0 WS=128`

- SACK表示**选择性ACK**，将提前到达的ACK序号放在这个字段，这样就避免了不必要的重传
- **TSval**(Timestamp value)和**TSecr**(Timestamp Echo Reply)都是32位的，发送方将当前时间填入TSval，接收方将当前时间填入TSecr，作用就是计算RTT(Round-Trip-Time)和防止回绕

- WS（15位）用来扩大Win（16位），将Win扩大到31bit，`128`表示2的7次方，Win向左移动了7位

我的数据包最后还有这样一段英文

```t
[TCP segment of a reassembled PDU]
```

它代表该TCP区段是属于上层（比如HTTP）协议的数据

> [选项区参考](https://luoguochun.cn/post/2016-09-23-tcp-fuck/#%E5%B0%8F%E7%BB%93)

### 是否丢包

![wireshark-tcp-2](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-tcp-2.png)

打开序列/时间统计图，路径：`统计`-->`TCP流图形`-->`时间序列`

观察到序列号都是上升的，说明没有丢包，丢包在抓包过程中也会有提示（TCP **Retransmission**）

### 计算吞吐量

因为我使用HTTP协议进行传输数据，所以直接在http的post就能查看本次传输的相关信息

在TCP首选项里打开`calculate conversation timestamp`

观察TCP层

![wireshark-conversation-timestamp](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-conversation-timestamp.png)
$$
吞吐量=153010bytes/1.885852617s=81135.714byte/s=79.2340964389k/s
$$

### 四次挥手

![wireshark-tcp-over](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-tcp-over.png)

重抓了一遍，这里只考虑正常的释放过程，其他本人知识水平不够暂不研究

## 分析UDP

QQ登录使用的就是`UDP`协议，它自己在应用层的协议是`OICQ`

![wireshark-udp-oicq](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-udp-oicq.png)

UDP报文段很简单，简单观察可以知道UDP有四个字段，每个字段占2字节，接下来分析UDP和IP的里面的长度是什么关系

![wireshark-udp-lenth](https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/wireshark-udp-lenth.png)

仅考虑IPv4

IP长度=IP头（20字节）+UDP头（8字节）+UDP数据

UDP长度=UDP头（8字节）+UDP数据

### 有效负载

length只有2byte=65536bit，所以有效负载=65536-8=65528字节

### 发送与接收的端口关系

发送者发送端口号在接收返回（响应）UDP 时候会变成接收端口号。
接收者发送返回（响应）UDP 时候接受端口号会变成发送端口号。