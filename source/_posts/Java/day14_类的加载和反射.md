---
title: 类的加载和反射
date: 2020-3-11
author: shepherd
toc: true
categories: [Java]
---

 对于我来说就是不停机加载类

<!-- more -->

## 类的加载

- 当程序运行的时候，系统会首先把要使用的Java类加载到内存中，加载的是编译后的class文件
- 每个类加载到内存中，会创建一个对应的Class对像，这个对象保存了这个类有哪些成员（数据，方法）
- 我们的程序用到什么类，才会加载什么类

## 类的加载器

作用：将.class文件加载到内存中，生成对应的java.lang.Class对象

## 反射

功能：在程序运行的时候，需要动态的加载一些类这些类可能之前用不到所以不用加载到JVM，而不用停止程序，重新加载程序

ok，介绍完毕，代码部分

测试的类

```java
package com.shepherd;

public class Student {
    
    private String name;
    private int id;
    private int age;
    public int score;
    
    public Student (){}
    public Student(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }
    public Student(String name, int id, int age,int score) {
        this.name = name;
        this.id = id;
        this.age = age;
        this.score = score;
    }
    private Student (int age) {
        this.age = age;
    }
    public void learn() {
        System.out.println("学习");
    }
    public void learn(String course) {
        System.out.println("学习"+course);
    }
    public void show() {
        System.out.println(name+":"+id+":"+age);
    }
    private void eat(String food) {
        System.out.println("正在吃"+food);
    }
}
```

### 获取Class对象

每个类只有与之对应的唯一的Class对象

```java
package com.shepherd;

public class GetClass {
    public static void main(String[] args) throws Exception {
        
        //获取class对象
        //1.通过对象获得
        Student s = new Student();
        Class c = s.getClass();
        //2.通过类获得（不常用）
        Class c2 = Student.class;
        //3.Class的静态方法
        Class.forName("com.shepherd.Student");
    }
}
```

### 获取构造方法

```java
package com.shepherd;

import java.lang.reflect.Constructor;

public class GetConstructor {
    public static void main(String[] args) throws Exception {
        
        Class c = Class.forName("com.shepherd.Student");
        
        //获得所有public构造方法
        Constructor[] cs = c.getConstructors();
        
        //获得指定构造方法,没有参数代表无参构造方法
//        Constructor cs2 = c.getConstructor();
        Constructor cs2 = c.getConstructor(String.class,int.class,int.class);
        
        //调用构造方法
        Object o = cs2.newInstance("shepherd",100,19);
        Student s = (Student)o;
        s.show();
        
        //忽略权限得到构造方法
        Constructor[] cs3 = c.getDeclaredConstructors();
        Constructor cs4 = c.getDeclaredConstructor(int.class);
        
        //使用private的构造方法
        cs4.setAccessible(true);
        Object o2 = cs4.newInstance(19);
        Student s2 = (Student)o2;
        s2.show();
        
    }
}
```

### 获取字段

```java
package com.shepherd;

import java.lang.reflect.Field;

public class GetField {
    public static void main(String[] args) throws Exception {
        //因为获取字段需要对象，这里new一个对象
        Student stu = new Student("shepherd",9001,19);
        Class c = Class.forName("com.shepherd.Student");
        
        //得到所有public字段（成员变量）
        Field[] fields = c.getFields();
        //指定字段名字，public
        Field field = c.getField("score");
        
        //反射形式访问字段
        System.out.println(field.get(stu));
        
        //得到所有字段（忽略权限）
        Field[] fields2 = c.getDeclaredFields();
        //指定字段名字（忽略权限）
        Field field2 = c.getDeclaredField("age");
        
        //反射形式访问字段
        field2.setAccessible(true);
        System.out.println(field2.get(stu));
        
    }
}
```

### 获取成员方法

```java
package com.shepherd;

import java.lang.reflect.Method;

public class GetMethod {
    public static void main(String[] args) throws Exception {
        
        Student stu = new Student("shepherd",9001,19);
        Class c = Class.forName("com.shepherd.Student");
        
        //得到所有public方法，包括继承父类
        Method[] method = c.getMethods();
        //指定方法名,public,
        Method m = c.getMethod("learn");
        m.invoke(stu);
        //加上参数
        Method m2 = c.getMethod("learn",String.class);
        m2.invoke(stu, "Java");
        
        //得到自身的所有方法，包括private
        Method[] m3 = c.getDeclaredMethods();
        
        //调用private方法
        Method m4 = c.getDeclaredMethod("eat", String.class);
        m4.setAccessible(true);
        m4.invoke(stu, "垃圾");
    }
}
```

