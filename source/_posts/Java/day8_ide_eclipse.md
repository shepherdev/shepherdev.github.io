---
title: 
date: 2020-3-3
author: shepherd
img: 
top: false
cover: false
coverImg: 
password:
toc: ture
mathjax: false
summary: 
categories: 
  - 编程
tags:
  - ide
  - Java
---

> 注意记录一些快捷键和一些欲哭无泪的事

注意改成等宽字体，不然很难受

### 快捷键

| 组合键                       | 含义                     |
| ---------------------------- | ------------------------ |
| `ctrl+shift+F`               | 代码格式化               |
| `alt+方向左右`或`ctrl+PD/PU` | 切换项目卡               |
| `alt+方向上下`               | 交换上下行位置           |
| `ctrl+/`                     | 将选中的行注释或取消注释 |
| `ctrl+shift+/或\`            | 将选中注释或取消注释     |
| `ctrl+d`                     | 删除一行                 |
| `ctrl+shift+O`               | 自动引入包删除包         |
| `ctrl+F11`                   | run                      |
| `ctrl+alt+down/up`           | 快速复制                 |
| `ctrl+1`                     | 修复错误                 |

### toString()

- 所有类都继承java.lang.Object
- 它有一个toString()方法可以显示内存地址
- 利用eclipse可以重写toString()方法（显示所有成员）

### 比较

- `==`比较的是引用
- `equals`比较的是数据
- 有时候还是要重写`equals`进行对象之间的比较

### jar库

- 就是一个压缩包，里面包含类
- 用户也可以生成jar包

### eclipse编译环境

下载对应版本，在eclipse设置即可

### Scanner

- hasNextxxx可以用来判断输入的是不是xxx类型的

### StringBuff和StringBuilder

#### StringBuff

- 相对与String字符占多少申请多少空间，String是直接申请很多空间
- String虽然也可以通过+将字符组拼，但那会重新申请内存，将字符“搬”到里面去，比较费时间
- StringBuff有一个默认的容量，他会根据你的字符进行扩容，扩容时改变指向，容量通常X2

#### StringBuilder

- 基本和StringBruff相同

#### 不同

- StringBuilder线程不安全，性能略高
- StringBuff线程安全，性能相对低

> 字符串在JDK9之前使用char（2字节）存储，JDK9使用byte存储

