---
title: 
date: 2020-2-29
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

## 类class

类里面有成员变量和成员方法

### 定义

例子(public class只能有一个)

```java
class 类名{
	成员变量;
	
	public void practice(){
		方法体;
	}
}
```

### 特点

成员变量可以不初始化

- 自动默认值

## 对象

通过类创建的变量就叫对象

### 声明+初始化

```java
类名 对象=new 类名();
```

new相当于初始化

> new才会分配内存空间（堆），然后进行初始化数据 

`对象.成员变量/方法`可以访问类的变量和方法

### 使用

除了一般使用对象还可以作为成员变量存在于另一个类里面

## 面向对象和面向过程

- 面向过程就像做事一步一步完成(C)

- 面向对象就像操作不同的职位进行管理(Java,c#...)

- 面向对象可以让程序更加有条理性，是更好的程序表达

## 成员变量和局部变量

| 成员变量                            | 局部变量             |
| ----------------------------------- | -------------------- |
| 定义在**类**里面                    | 定义在**方法**里面   |
| 只有在`new`(自动初始化)时才占用内存 | 调用方法才占用空间   |
| 数据在**堆**里面                    | 数据在**栈**里面     |
| 通过GC回收                          | 方法调用完即释放内存 |

### GC

垃圾回收(Garbage Collection)是Java虚拟机(JVM)垃圾回收器提供的一种用于在空闲时间不定时回收**无任何对象引用的对象**占据的内存空间的一种机制

[参考](https://www.jianshu.com/p/5261a62e4d29)

## 匿名对象

直接使用new出的对象就叫匿名对象，通常只调用一次时才会使用匿名对象

如`new 类名();`

 可以作为参数使用

如`new ClassName1().method(new ClassName2());`

> 匿名对象不会在栈分配空间，而是直接在堆里分配空间，所以调用完毕会立马被GC回收
>
> 而有名字的对象由于在栈里有空间，所以要等方法调用完才会被清理，当它调用一个匿名对象时，匿名对象也会存活久一些

## 修饰符public和private

### 用法

- 在类里使用public和private修饰成员变量和成员方法

```java
class Hello{
	private int test;
	private Test(){
		int a;
	}
}
```

- public代表公开，允许其他类使用
- private代表私有，不允许其他类使用

### 功能

- 提高容错率
- 也就是让程序更加健壮

## 构造方法

功能：方便初始化

说明：当我们new一个新对象时，系统会根据类型自动初始化（默认的构造方法），而我们想要初始化成自己的数据时就不得不使用`对象名.成员=value`来初始化，于是我们可以自定义构造方法来进行初始化

操作：

```java
class Test{
	int a;
	String b;
    //无参初始化
	public Test(){
		//初始化操作
	}
    //有参初始化，参数不同，方法不同
	public Test(int _a,String _b){
		a=_a;
		b=_b;
	}
}
//此时有了两种构造方法
Test a = new Test();
Test a = new Test(13,14);
```

- 一般修饰符是public
- **单例模式**用到private

## this

功能：代表当前对象，实用上就是方便见名知意

```java
class Test{
	int a;
	String b;
	public Test(){
		//初始化操作
	}
    //此时重名就不会有影响
	public Test(int a,String b){
		this.a=a;
        //成员	参数
		this.b=b;
	}
    //还可以this.方法
}
```

## 封装

是一种编程思想，让程序更加安全

做法

- 将类的成员变量全部设置为private，不让外界直接接触
- 若要查看某成员，提供get方法
- 若要编辑，提供set方法(慎用)

## static修饰

- 可以修饰成员变量和成员方法
- 通过成员和类都可以调用，调用的是同一串数据
- 修饰成员变量表示静态变量，静态变量是所有对象共有的，**只占一份空间**
- 修饰成员方法表示静态方法，**静态方法只能访问静态变量/方法**，访问非静态变量时会不知道对象，传递一个对象参数才能访问成员变量

### 例子

```java
class Person{
	public int age;
	public static String country;
	public Person(int age,String country){
        this.age = age;
        this.country = country;
    }
}

class StaticDemo{
    public static void main(String[] args){
    	Person a = new Person(13,"China");
    	Person b = new Person(15,"America");
        System.out.println(a.age+"-"+a.country);
        System.out.println(b.age+"-"+b.country); 
        //使用“类名.静态变量”也可以访问
    }
}
```

运行结果

```bash
[shepherd@MY day5 07:33 下午]$ java StaticDemo 
13-America
15-America
```

### 解释原理图

<img src="/home/shepherd/blog_pic/static原理.png" style="zoom:50%;" />

### 结论

- 当多个对象使用同一个一模一样的成员时，使用静态变量可以节约内存空间

- 非静态成员可以访问静态成员

### 特点

1.随着类的加载而加载

2**.优先于对象，也就是说并不需要创建对象才能使用**

3.所有对象共享，可以通过对象或者类进行访问

### 实际运用

- 在创建工具类时可以使用static，这样就不需要再创建对象了
- 在编写工具类时，推荐将**构造方法私有化**，防止别人使用麻烦的代码（创建对象等）

## 静态代码块

```java
static{

}
```

功能：在加载class时先加载一次静态代码块里面的东西，以后就不会加载了

