---
title: 多态与接口
date: 2020-3-3
author: shepherd
toc: ture
categories: [Java,多态]
tag: [多态,接口]
---

## 多态

利用父类声明，子类构造就是多态，如

`父类 对象 = null;`

`对象 = new 子类();`

- 因为是用父类声明的，所以只能访问父类的成员
- 若要访问子类的成员，需要**强制类型转换**，如

`((子类)对象).成员`

<!-- more -->

> 注意，不能使用子类声明，父类构造

[详解多态](https://www.cnblogs.com/chenssy/p/3372798.html)

## 抽象类

### 功能

当我们的类需要同一个方法，但这个方法却有不同的是实现时，可以使用抽象类进行**继承重写**这个方法

### 例

```java
/* 在公司工作的都是雇员
 * 他们都需要工作，但工作的
 * 做法不同，所以将工作抽象
 */
abstract class Employee{
	private String name;
    /* 虽然是抽象类，构造方法也是可以继承的，
     * 这个构造方法啊是给子类的
     */
    public Employee(String name){
        this.name = name;
    }
    //抽象方法，子类实现
    public abstract void work();
}

class Programmer extends Employee{
    public Programmer(String name){
        super(name);
    }
    public void work(){
        //实现的代码,比如
        System.out.println("我在编程");
    }
}

class Manager extends Employee{
    public Programmer(String name){
        super(name);
    }
    public void work(){
        //实现的代码，比如
        System.out.println("我在管理");
    }
}
```

### 注意

- 抽象方法必须在抽象类里面
- 抽象类里面不一定要有抽象方法
- 抽象类可以被继承抽象类，但子类必须实现上面所有的类的抽象方法

## 接口

- 相当于一个标准，规范，一切实现都需要通过子类
- 接口里面只能包含抽象方法（完全抽象化）
- 接口可以被实现，不能被继承
- 接口可以被抽象类实现，也可以被具体类实现

### 例

```java
//定义接口
interface A{
    void method1();
    void method2();
    void method3();
}
//类被继承，接口被实现
class B implements A{
    public void methid1(){
        //具体实现
    }
    public void methid2(){
        //具体实现
    }
    public void methid3(){
        //具体实现
    }
}
```

### 注意

- 单继承，多实现

`class Test extends 类 implements 接口1,接口2{}`

- 注意用什么类声明，什么类构造
- 接口也可以被接口继承

