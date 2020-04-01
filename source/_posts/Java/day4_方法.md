---
title: 
date: 2020-2-28
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
  - Java
---

方法就像C语言的函数，用来**简化代码**用的

### 定义格式

```java
修饰符(如public static) 返回值类型 方法名(参数){
	方法体;
}
```

### 返回值

- 如果类型为void可以不需要返回值
- 通过return可以有一个返回值，要与返回值类型相同

### 参数

- 基本数据类型只传值(不会破坏元数据)
- 引用数据类型是传引用(破坏原数据)

### 方法重载(reload)

也就是方法名可以一样，但参数不能一样 

判断方法是否重复要看**方法名+参数**，两者一样才算重复

比如

```java
public static void Method(){
	System.out.println("Hello");
}
public static void Method(int a){
	System.out.println("Hello");
}
public static void Method(int a,int b){
	System.out.println("Hello");
}
```

### 程序健壮

不容易崩溃的程序才叫健壮

一般需要加上很多判断条件才能让程序不容易崩溃

## 枚举

功能：通常1,2,3,4标识代码里面的角色时，但就这样的话代码变得难读，而枚举就是为了增加代码可读性

### 枚举示例

```java
package com.shepherd.student03;

public class TestEnum {
    public static void main(String[] args) {
        int a = Season.AUTUMN;
        int b = Season.SPRINT;
    }
}

//使用单独的一个类来管理季节
class Season {
    public static final int SPRINT = 0;
    public static final int SUMMER = 1;
    public static final int AUTUMN = 2;
    public static final int WINTER = 3;
}

```

### 枚举类型

相当与上面的简化

```java
package com.shepherd.student03;

public enum Season {
    SPRING,SUMMER,AUTUMN,WINTER
}
```

```java
package com.shepherd.student03;

public class Demo_Enum {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN);
        System.out.println(Season.AUTUMN.ordinal());

    }
}
```

遍历

```java
for(Season s : Season.values()) {
    System.out.println(s);
}
```



> 基础学习完毕