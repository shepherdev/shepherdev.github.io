---
title: 
date: 2020-3-2
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

## 继承

### 功能

使一个类（子类）具有另一个类（父类）的功能

### 优点

当不同的类使用相同的成员变量/方法时，可以将相同的变量/方法放在其他类，简化代码，方便维护

例子(定义+使用)

```java
  1 //定义一个人的类，有年龄名字，和吃饭的行为
  2 class Person{
  3     private int age;
  4     private String name;
  5     public void eat(){
  6         System.out.println("吃饭");
  7     }
  8     public int getAge(){
  9         return age;
 10     }
 11     public void setAge(int age){
 12         this.age = age;
 13     }
 14     public String getName(){
 15         return name;
 16     }
 17     public void setName(String name){
 18         this.name = name;
 19     }
 20 }
 21 //定义一个学生，继承人，他本身有小名，学习的行为
 22 class Student extends Person{
 23     private String childboodName;
 24     public void learn(){
 25         System.out.println("学习");
 26     }
 27     public String getChildboodName(){
 28         return childboodName;
 29     }
 30     public void setChildboodName(String childboodName){
 31         this.childboodName = childboodName;
 32     }
 33 }
 34 //使用
 35 public class ExtendsDemo{
 36     public static void main(String[] args){
 37         Student a = new Student();
 38         a.setAge(18);
 39         a.setName("明明");
 40         a.setChildboodName("小明");
 41         System.out.println(a.getName()+"-"+a.getAge()+"-"+a.getChildboodName());
 42     }
 43 }
```

### 构造方法

- 在子类的构造方法里面使用`super()`可以调用父类里面的构造方法
- `super()`必须写在子类构造方法的第一句
- 实际上super还可以调用成员，如果成员公开

### 注意事项

- 不支持多继承，即一个类只能继承一个类（关系纯洁??）
- 子类继承了父类的成员
  - 子类不能访问父类的私有成员
  - 子类拥有父类的私有成员，不能直接访问
- “是一个”关系，比如学生是人，不能说人是学生
- 当子类和父类有重名变量时，谁近就访问谁，通过this，super可指定具体的变量
- 通过子类的构造方法构造对象时，都会调用父类的构造方法
  - 对父类进行初始化，不指定则调用默认的构造方法
  - 如果设置了有参构造方法时，系统默认的无参构造方法就没有了
- 一个类里面的方法可以相互调用，构造方法之间可以通过this()相互调用（不能与super()重复）

### 方法覆盖/重写(override)

当子类的方法与父类的方法重名(指参数，方法名，返回值)时，则优先使用子类的方法

- 这个方法不能设置成私有的，会说权限不够

## 方法重载(overload)与重写(override)

| 方法重写                 | 方法重载          |
| ------------------------ | ----------------- |
| 子类和父类               | 一个类            |
| 参数，返回值，方法名一样 | 方法名+参数不一样 |

## final修饰符

可以与public，private修饰符一起用

### 1.修饰类

表示这个类无法被继承

### 2.修饰方法

表示这个方法不能被子类重写

### 3.修饰变量

- `final int a=10;`

则该变量后面不能被赋值（变成常量）

- `final int a;`

可以被赋值一次

## 类包package

在开发大项目时，难免会遇到类名重复的问题，所以可以使用包类进行管理类

### 定义

在开头写上`package xx.xx.xx`

### 命名规则

- 习惯上以域名开头
  - com.xx.xx

### 编译

`javac -d . .java`

## 访问权限修饰符

|        | public | protected | default | private |
| ------ | ------ | --------- | ------- | ------- |
| 同类   | yes    | yes       | yes     | yes     |
| 同包   | yes    | yes       | yes     | no      |
| 子父类 | yes    | yes       | no      | no      |
| 不同包 | yes    | no        | no      | no      |

- 对类的修饰只有`public`和`default`，因为类分为包内包外（内部类除外）

## 内部类

类中再定义一个类就是内部类，此类相对来说就是外部类

- 内部类可以访问外部类的所有成员
- 外部类不能访问内部内的所有成员，因为他们是一对多的关系，外部类不知道是那个类的成员
- 内部类依附外部类而存在
- 非静态内部类不能声明静态成员

### 使用

```java
//1
外部类.内部类 对象 = new 外部类().new 内部类();
//2
外部类 对象 = new 外部类();
外部类.内部类 对象 = new 内部类();
```

### 局部内部类

在方法里面创建类就叫局部内部类

### 匿名内部类JDK8

当类只需要使用一次时，写成一个类有点浪费，于是有了匿名内部类

比如实现接口的功能只需要用一次时

```java
interface Product{
	int getPrice();
	String getName();
}

class innerClass{
	public static void main(String[] args){
		System.out.println(
            //因为没有名字，所以叫匿名内部类
			new Product(){
				public int getPrice(){
					return 2;
				}
				public String getName(){
					return "笔";
				}
			}.getName
		);
	}
}
```

例2

```java
class InnerClass{
    public static void test(Vehicle f){ 
        f.transit();
    }   
    public static void main(String[] args){
        test(
            new Vehicle(){
                public void transit(){
                    System.out.println("test");
                }   
            }   
        );  
    }   
}

abstract class Vehicle{
    public abstract void transit(); 
}
```

> 匿名内部类也可以传递参数，参数参照父类的构造方法

### 重名

当外部类和内部类重名时，访问遵循就近原则，若想访问特定的成员，可以使用`类名.this.成员`进行访问

