---
title: 条件判断和字符串
date: 2020-2-24
author: shepherd
toc: ture
categories: [Java]
---

 分支、循环与字符串相关的一些操作

<!-- more -->

## 条件判断

### 1.if语句

语法：

```java
//形式1
if (布尔表达式) {
}
//形式3(else if数量不限，else可有可无)
if (布尔表达式) {
    
} else if (布尔表达式) {
    
} else {
    
}
```

### 2.switch语句

```java
switch(表达式){
	case value1:
		语句块;
		break;
	case value2:
		语句块;
		break;
	default:
		语句块;
		break;
}
```

> 表达式可以是整数，字符，枚举，字符串
>
> case后面必须是常量
>
> break用来中断语句

## 循环

### 1.while循环

语法

```java
while(布尔表达式){
	循环体;
}
```

满足条件运行循环体

### 2.do while循环

语法

```java
do{
	循环体;
}while(表达式)
```

### 3.for循环

语法

```java
//1
for(初始化语句;条件判断语句;更新语句){
	循环体;
}
//2
for (循环变量类型 循环变量名称 : 要被遍历的对象) {
    循环体        
}
```

- 条件判断语句不写默认true

## break continue

- break跳出**当前**循环
- continue中断**当前**循环，进入下一次循环

### break的其他用法

- 自定义标签，跳出标签所在循环

比如

```java
outside:
while(){
	inside:
	for(){
		break outside;
	}
}
```

## 字符串

### 常量

在`""`里面的就是字符串常量

### 变量

定义一个字符串变量需要引用一个新的类`String`

有两种定义模式

1.`String str = "HelloWorld!";`

2.`String str2 = new String("HelloWorld!");`

### 连接

通过`+`可以对字符串（或其他数据类型）常量和变量作连接操作

### 特性

- 字符串是无法被修改的
- 基本数据类型都在**栈**区，栈容量比较小
- 引用数据类型在**栈**存放地址，数据存放在**方法常量区**，地址指向数据

如图

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/引用数据类型存放内存.png" alt="引用数据类型存放内存" style="zoom:50%;" />

- 基本数据类型变量改变时会直接覆盖原始数据
- 引用数据类型的原始数据不会被删除，改变时会创建新的数据，并改变地址指向

### 信息获取

也就是针对字符串的一些操作

#### 长度

`s.length()`

s代表字符变量

示例

```java
String str = "Hello World!";
System.out.println(str.length());
```

结果就是12

#### 查找子字符串的位置

查找字符会从0开始索引

1.`s.indexOf(int c);`

传递一个数字或字符，返回字符的索引

2.`s.indexOf(String str);`

传递一段字符串，返回第一个字符的索引，不存在返回-1

3.`s.lastIndexOf(String str);`

从字符串末尾开始查找，返回第一个字符的索引

#### 获取指定索引的字符

`s.charAt(int index);`

#### 获取指定字符串

`s.substring(int beginindex);`

得到索引以后的字符

`s.substring(int beginindex,int endindex);`

得到索引范围内的字符

### 判断

判断的返回值是布尔值

#### 开头字符

`s.startWith(String prefix);`

#### 结尾字符

`s.endWith(String suffix);`

> `""`虽然也返回true，但没有意义

#### 相等

1.`==`

功能：可以判断数据引用是否相同

规则：`str1==str2`

特例

```java
String str1 = "Hello World!";
String str2 = new String("Hello World!");
System.out.println(str1==str2);
```

结果是`false`

为什么？

看图：

<img src="https://cdn.jsdelivr.net/gh/shepherdev/blog_image/article/2020/字符串new存储方式.png" alt="字符串new存储方式" style="zoom:50%;" />

2.`s.equals(String str);`

功能：直接查看字符是否相同

示例：

```java
String str1 = "Hello World!";
String str2 = new String("Hello World!");
System.out.println(str1.equals(str2));
```

3.`s.equalsIgnoreCase(String str);`

功能：忽略大小写

4.`s.compareTo(String str);`

功能：比较字符串的大小，通过索引一个一个字符的比较，相同则跳过，遇到不同则返回差值，若s与str前面几个字符相同后面不同则返回字符串长度的差值

#### 含某子字符串

`s.contains(String str)`

#### 空字符串

`s.isEmpty`

空的状态

```java
String s1 = "";		//空指针
String s2 = null;	//空对象
```

### 转换操作

操作都不会改变原有字符串，会创建新的字符串

#### 大小写

`s.toLowerCase(String str)`

`s.toUpperCase(String str)`

#### 除去首尾空格

`s.trim(String str)`

#### 替换

`s.replace(old str,new str)`

