---
title: 数组
date: 2020-2-27
author: shepherd
toc: ture
categories: [Java]
tag: [数组]
---

## 引用数据类型和基本数据类型在创建的区别

- 基本数据类型只要声明了，就会被**赋予内存**，不管有没有赋值
- 引用数据类型创建分两步，声明和初始化
  - 声明只在**栈**里面分配内存，用来**存储引用（地址）**
  - 初始化在堆里或方法区里分配内存，**创建实际数据**
    - `new`在堆里
    - 程序运行常量放在方法区里

<!-- more -->

## 栈

所有**局部变量**都在栈里

## 一维数组

### 声明

有两种声明方法，我使用int类型举例

1.`int[] ArrayName`

2.`int ArraryName[]`

注意：没初始化的数组无法使用

### 初始化

初始化才能使用数组，三种方式

```java
//初始化使用默认值(基本数据类型0,false，引用数据类型null)
int[] ArrayName = new int[3];
//根据元素个数
int[] ArrayName = new int[]{1,2,2};
//和上面差不多
int[] ArrayName = {1,2,2};
```

###  使用数组

`ArrayName[索引]`

索引从0开始，可以对元素进行赋值和使用

使用时需要注意：

- 索引不能超出范围
- 引用空数组

填充数组：

```java
import java.util.Arrays;
//使用
Arrays.fill(ArrayName,value);
```

## 二维数组

### 1.声明

- `数据类型[][] ArrayName`
- `数据类型 ArrayName[][]`

### 2.初始化

```java
//
数据类型[][] 数组名=new 数据类型[m][n];//使用默认值
//
数据类型 数组名[][]={{a,b},{c,d}};
//
int[][] arrayName = new int[10][];//默认值null
//赋值
arrayName[0] = new int[]{a,b,c};
```

### 3.使用

`arrayName[索引][索引]` 

举一个遍历二维数组的例子

```java
int[][] a = {{1,2,3,4,5},{0,9,8,7,6}};

for(int i=0;i<a.length;i++){
	for(int j=0;j<a[i].length;j++){
		System.out.print(a[i][j]+" ");
	}
}
```

