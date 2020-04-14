---
title: Java基础
date: 2020-2-23
author: shepherd
toc: ture
categories: [Java]
tags: [JDk]
---

## Java简介

Java是一门面向对象编  程语言，具有简单性、面向对象、[分布式](https://baike.baidu.com/item/分布式/19276232)、[健壮性](https://baike.baidu.com/item/健壮性/4430133)、[安全性](https://baike.baidu.com/item/安全性/7664678)、平台独立与可移植性、[多线程](https://baike.baidu.com/item/多线程/1190404)、动态性等特点 。Java可以编写[桌面应用程序](https://baike.baidu.com/item/桌面应用程序/2331979)、[Web应用程序](https://baike.baidu.com/item/Web应用程序)、[分布式系统](https://baike.baidu.com/item/分布式系统/4905336)和[嵌入式系统](https://baike.baidu.com/item/嵌入式系统/186978)应用程序等  。

<!-- more -->

### 跨平台

开发出来的项目可以在各个操作系统上执行，而不需要针对操作系统单独开发，原因就是Java有一个解释器，JVM(Java Virtual Machine)

### 运行原理

`.java`编译出字节码程序`.class`，通过JVM在操作系统上运行，所以每个操作系统有不同的JVM

### JDK和JRE

JRE(Java Runtime Environment)也就是java运行环境，JDK(Java Development Kit)也就是Java开发工具包，jdk里面有jre，百度下载安装就可以开始编程了

## 环境变量配置

JDK安装后，路径下u有个`bin`，`javac`、`java`等编译运行指令都在这个目录，如果不想每次编译都输入很长的命令就把它设置到环境变量吧，win10找到path添加绝对路径，linux可以在/etc/profile、~/.bashrc、\~/.bash_profile里面添加`PATH="${PATH}:你jdk的bin存放的绝对路径"`，重新打开一个shell就有了

注意新增一个变量classpath，设置路径后，就可以在任意地方运行你的java程序了

## Java标识符

1.由字母，数字，`_$`组成

2.不能以数字开头

3.**不能是Java关键字**

4.区分大小写

5.最好遵循陀峰命名法

- 类名首字母大写，其他首字母小写

6.**不能有空格**

## java语言类型

### 基本数据类型(Primitive Type)

1.数值类型

- 整数类型
  - byte:占1字节
  - short:占2字节
  - int:占4字节
  - long:占8字节
- 浮点类型
  - double：8字节
  - float：4字节
- 字符类型
  - chat：2字节

2.boolean类型

- 占1位，只有false和true两个值

> 需要特别注意的就是整数默认是int型，浮点数默认是double型，赋给其他类型时记得手动转换类型，不然编译会报错
>
> 但是short和byte如果在数据范围内的话不会报错，而且也不用手动转换类型

示例

```java
public class DataType{
        public static void main(String[] args){

                byte a   = 1000;
                long b   = 10000l;
                float c  = 1.3f;
                double d = 5.3;

                System.out.println(a);
            	System.out.println(b);
            	System.out.println(c);
            	System.out.println(d);
            
        }
}
```

### 引用数据类型(Reference Type)

1.类

2.接口

3.数组

> 需要注意的是字符串类型也是一个类，也属于引用数据类型

### 变量与常量

- 定义变量格式

  `数据类型 变量名 = value`

- 定义常量格式

  `final 数据类型 常量名  = value`

###  总结

- 定义long和float时，数字末尾加上l和f，大小写随意
- char可以存储汉字，因为他占2字节，汉字也占两个字节

> 正数采用二进制源码存储
>
> 负数采用补码存储

## 算术运算符

1.符号：`* / + - %`

2.规则：`操作数1 运算符 操作数2`

运算的特例

- 使用浮点数时，数据不会精确
- 使用`+`可以对字符串进行组拼
  - `hello"+"world" ==> "helloworld"`
  - `"44"+44 ==> "4444"`
  - `"hello"+44 ==> "hello44"`
  - `"hello"+'a' ==> "helloa"`
  - 使用单个字符时，会把字符转换成数字在进行运算

## 赋值运算符

符号：`= += -= *= /= %=`

## 比较运算符

符号：`< > <= >= == !=`

返回结果：boolean值`true`或`false`

## 逻辑运算符

符号：

- `|| `短路或：也就是说boolean1为true，则boolean2不用算直接得结果
- `&& `短路与
- `！ `
- `^`异或：结果不一样返回true
- `&`与：不管boolean1的true和false，boolean2也会计算
- `|`或

规则：`boolean1 逻辑运算符 boolean2`

注意：`1<=x<=2`是错误的写法

## 按位

- `&`按位与
- `|`按位或
- `~`
- `^`按位异或

## 移位

- `>>`右移，高位用符号位填充
- `<<`左移，低位0填充
- `>>>`无符号右移，高位0填充，低位丢弃

## 三元运算

规则：`布尔表达式?表达式1:表达式2`

`true`使用表达式1

`false`使用表达式2

> 表达式不能是赋值语句

示例

```java
public class TernaryOpretor{
        public static void main(String[] args){
			System.out.println(8>9?"正确":"错误");
		}
}
```

## 类型转换

### 隐式类型转换

数值在赋值和运算时会自动转换类型

```java
byte a=100;
float b;
b = a;
```

a就会变为`float`型

### 显式类型转换

自己转换类型

```java
float a = 100;
byte b = (byte)a;
```

如果不转换类型的话会报错

