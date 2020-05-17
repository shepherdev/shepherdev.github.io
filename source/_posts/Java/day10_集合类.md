---
title: 集合类
date: 2020-3-5
author: shepherd
toc: ture
categories: [Java]
tag: 集合
---

## ArrayList集合

相当于一个可变的数组，这个数组可以存放任何数据类型（但是建议存储相同类型的）

- 集合里面只能存储引用类型
- 他会自动把基本类型装箱

<!-- more -->

举个例子

```java
package com.shepherd1.arraylist;

import java.util.ArrayList;

public class ArrayListDemo {
    public static void main(String[] args) {
        ArrayList al1 = new ArrayList();
        
        al1.add(123);
        al1.add(2);
        al1.add(2, 3);
//        al1.add("test"); 
        al1.addAll(0,al1);
        System.out.println(al1.get(1));
        
        for(int i=0; i<al1.size(); i++) {
            System.out.print(al1.get(i)+" ");
        }
        System.out.println();
        
        al1.remove(2);
        for(int i=0; i<al1.size(); i++) {
            System.out.print(al1.get(i)+" ");
        }
        System.out.println();
        //进行运算
        int result = 0;
        for(int i=0; i<al1.size(); i++) {
            result += (int)al1.get(i);
        }
        System.out.println(result);
    }
}
```

- ArraryList里存储的类型都是Object类型
- 所以运算需要进行强制转换
- add的类型决定转换的类型

### 遍历器

除了使用for进行遍历外，还可以使用遍历器进行遍历，遍历器有一个指针，一开始空指向，随着调用next而改变指向

```java
Iterator iterator = al1.iterator();
        while(iterator.hasNext()) {
          	System.out.println(iterator.next());;
}
```

### for高级遍历

```java
for(Object o : al1) {
            System.out.println(o);
}
```

## Vector集合

大致语法和ArrayList一样，主要区别是**Vector线程安全**，适合多线程，不是多线程的话可以使用ArrayList

## LinkedList

- 语法同样和ArrayList差不多
- 主要区别是存储结果不同，ArrayList里面是数组，LinkedList存的是引用
- 这样的优点就是**插入数据和删除数据的效率快**

## 泛型

在上面，使用ArrayList创建的数组存储int型数据时，计算需要强制转换类型，解决这个强制转换的方法就是泛型

- 注意`<>`里只能是引用数据类型
- 数据会自动装箱

```java
//泛型
ArrayList<Integer> a = new ArrayList<Integer>();
a.add(234);
a.add(34);
System.out.println(a.get(0)+a.get(1));
```

## HashSet

上面三个都是有序的，可重复的

而HashSet是无序的，不可重复的（所以没有get方法）

## TreeSet

和HashSet差不多，只是存储结构不一样

## 存储自定义的类

```java
package com.shepherd2;

import java.util.ArrayList;

public class Test {
    public static void main(String[] args) {
        ArrayList<Cut> c = new ArrayList<Cut>();
        
        Cut cut = new Cut(1,"xiaohua");
        c.add(cut);
        c.add(new Cut(2,"abiao"));
        
        for(Cut i : c) {
            System.out.println(i.age+" "+i.name);
        }
    }
}
```

## HashMap

举个例子

```java
package com.shepherd2;

import java.util.HashMap;

public class HashMapDemo {
    public static void main(String[] args) {
        //HashMap里面存储的是键值对，可指定key和value的类型
        HashMap m = new HashMap();
        //HashMap<String, Integer> = new HashMap<String, Integer>();
        m.put("shepherd", 2001);
        m.put("hello", 102);
        //键不能相同，相同会覆盖前面的值
    
        //遍历key
        for(Object key : m.keySet()) {
            System.out.println(key+" "+m.get(key));
        }
        //遍历value
        for(Object value : m.values()) {
            System.out.println(value+" ");
        } 
        System.out.println(m.containsKey("hello"));
        System.out.println(m.containsValue(2001));
    }
}
```

## TreeMap

和HashMap差不多 ，主要是存储上不同

## HashTable

与HashMap的区别是线程安全

性能略低

## 总结

### Collection(存储单个值)

- list，重复有序
  - ArrayList
  - LinkedList
  - Vector
- Set，不重复，无序
  - HashSet
  - TreeSet

### Map(存储键值对)

- HashMap
- TreeMap
- HashTable

## 自己写Conlection

当然功能很简陋

```java
package com.shepherd1;

public class MyArrayList<T> {
    private T[] dataArray = (T[])new Object[100];
    //虽然不能子类声明，父类构造，但可以强制转换
    private int index = 0;
    
    public void add(T data) {
        dataArray[index] = data;
        index++;
    }
    
    public T get(int index) {
        if(index>=0 && index<(this.index-1)){
            return dataArray[index];
        }else {
            return null;
        }
    }
    
    public int size () {
        return index;
    }
}
```

### 异常处理Exception

- 在java里面，所有错误类都属于Exception类

- 正常程序碰到错误会直接崩溃

- 可以用try语句对有可能出现错误的地方进行处理，即使错误也会执行后面的代码

语法

```java
try{
//这里是错误代码	
}catch(错误类型 对象){
//取得错误后的操作代码，如：对象.方法
}finally{
//不管有没有错都会执行的语句
}
```

- catch可以写无数个

- 如果想包含所有的错误可以把错误类型写成Exception