---
title: 方法
date: 2020-2-28
author: shepherd
toc: ture
categories: [Java]
tag: [方法,函数]
---

方法就像C语言的函数，用来**简化代码**用的

<!-- more -->

## 定义格式

```java
修饰符(如public static) 返回值类型 方法名(参数){
	方法体;
}
```

## 返回值

- 如果类型为void可以不需要返回值
- 通过return可以有一个返回值，要与返回值类型相同

## 参数

- 基本数据类型只传值(不会破坏元数据)
- 引用数据类型是传引用(破坏原数据)

当我们需要的参数不确定，或则很多时，方法重载就不适用了，可变参数有两种形式

### 可变参数

```java
package com.shepherd.parameter;

public class KeBian {
    public static void main(String[] args) {
        System.out.println(add(23,23,23,45));
    }
    
    public static int add(int... args) {
        int result=0;
        for(int i : args) {
            result += i;
        }
        return result;
    }
}
```

### 数组参数

```java
package com.shepherd.parameter;

public class Arrays {
    public static void main(String[] args) {
        System.out.println(add(new int[]{23,34,34}));
    }
    
    public static int add(int[] args) {
        int result=0;
        for(int i : args) {
            result += i;
        }
        return result;
    }
}
```

注意

- 可变参数的是个数可变
- 在调用方法时，如果可变参数方法和固定参数方法重复的话，优先固定参数
- 一个方法只能有一个可变参数，而且放在最后
  - `(int a, String... s)`

## 单元测试

### Junit

在写大型程序时，肯定是写一些代码测试一些代码，不可能再把所有代码运行一遍

在eclipse里面添加Junit库后在方法前面添加注解`@Test`即可进行单元测试

添加方法：eclipse的Project Explorer右键-->Build Path-->Configure Path-->Java Build Path-->Libraries-->Moudlespath-->add Library-->classpath-->Junit

## 方法重载(reload)

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

### 示例

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

### 类型

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

## toString()

- 所有类都继承java.lang.Object
- 它有一个toString()方法可以显示内存地址
- 利用eclipse可以重写toString()方法（显示所有成员）

## 比较

- `==`比较的是引用
- `equals`比较的是数据
- 有时候还是要重写`equals`进行对象之间的比较

## jar库

- 就是一个压缩包，里面包含类
- 用户也可以生成jar包

## Scanner

- hasNextxxx可以用来判断输入的是不是xxx类型的

## StringBuff

- 相对与String字符占多少申请多少空间，String是直接申请很多空间
- String虽然也可以通过+将字符组拼，但那会重新申请内存，将字符“搬”到里面去，比较费时间
- StringBuff有一个默认的容量，他会根据你的字符进行扩容，扩容时改变指向，容量通常X2

## StringBuilder

- 基本和StringBruff相同

#### 不同

- StringBuilder线程不安全，性能略高
- StringBuff线程安全，性能相对低

> 字符串在JDK9之前使用char（2字节）存储，JDK9使用byte存储