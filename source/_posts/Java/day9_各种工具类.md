---
title: 工具类
date: 2020-3-4
author: shepherd
toc: ture
categories: [Java]
tag: [随机数,包装类]
---

## Arrays数组工具类

举个例子

```java
import java.util.Arrays;

public class ArrarysTest {
    public static void main(String[] args) {
        int[] a = {234,24,25,2323,25,45};
        Arrays.sort(a);
        for(int i : a) {
            System.out.print(i+" ");
        }
        System.out.println();
        //先排序，后查找
        int index = Arrays.binarySearch(a, 234);
        System.out.println(index);
    }
}
```

<!-- more -->

## Math工具类

举个例子

```java
public class MathTest {
    public static void main(String[] args) {
        System.out.println(Math.pow(2, 1));
        System.out.println(Math.E);
        System.out.println(Math.PI);
        System.out.println(Math.abs(-9));
        System.out.println(Math.ceil(3.4));
        System.out.println(Math.floor(3.5));
        System.out.println(Math.max(32,34));
        System.out.println(Math.cos(3));
    }
}
```

结果

```bash
2.0
2.718281828459045
3.141592653589793
9
4.0
3.0
34
-0.9899924966004454
```

### 随机数

```java
System.out.println(Math.random());//[0-1)的随机
System.out.println(Math.random()*4);//[0-4)的随机数
System.out.println((Math.random()*4)+5);//[5-9)的随机数
System.out.println((int）((Math.random()*4)+5));//5,6,7,8的随机数
```

### BigDecimal

先简单说明一下double精度的问题，由于浮点数在计算机里面特殊的存储方法，所以double总会有精度的损失，double的精度在15-16位，所以只要出错大于这个位数，double就不会显示出来，如果在这个范围内出错了，double也会显示出来，反正不管如何，用double存储数据都是有风险的，于是可以用Math里的BigDecimal解决这个问题

举个例子

```java
package com.shepherd3.bigdecimal;

import java.math.BigDecimal;

public class BigDecimalTest {
    public static void main(String[] args) {      
//        double a = 2.32435435;
//        double b = 0.30000000000000002;
//        double c = 0.1;
//        double d = 0.2;
//        System.out.println(1.0+0.1);
//        System.out.println(b);
//        System.out.println(c+d);
        BigDecimal num1 = new BigDecimal("0.04");
        BigDecimal num2 = BigDecimal.valueOf(0.02);
        System.out.println(num1);
        System.out.println(num2);
        
        BigDecimal result1 = num1.add(num2);
        BigDecimal result2 = num1.subtract(num2);
        BigDecimal result3 = num1.multiply(num2);
        BigDecimal result4 = num1.divide(num2);
        
        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
        System.out.println(result4);
    }
}
```

## Date&Calender类

举个例子

```java
package com.shepherd3.date;

import java.util.Calendar;
import java.util.Date;

public class DateCalenderTest {
    public static void main(String[] args) {
        Date now = new Date();
        System.out.println(now);
        
        Calendar c = Calendar.getInstance();
        System.out.println(c);
        
        System.out.println(c.get(Calendar.AM));
        System.out.println(c.get(Calendar.DATE));
        System.out.println(c.get(Calendar.HOUR));
        System.out.println(c.get(Calendar.MINUTE));
    }
}
```

## 基本类型包装类

基本数据一般都在栈里面，而包装就是栈存地址，数据在堆里

1.Byte

2.Short

3.Integer

4.BigInteger（可以存一个大整数），与BigDecimal差不多

5.Long

6.Float

7.Double

8.Character

9.Boolean

举个例子吧

```java
package com.shepherd4.packageclasss;

public class PackagingClass {
    public static void main(String[] args) {
        Integer n1 = Integer.valueOf("100");
        System.out.println(n1);
        Byte n2 = Byte.valueOf("100");
        System.out.println(n2);
        Long n3 = Long.valueOf("100");
        System.out.println(n3);
        Float n4 = Float.valueOf("100.234");
        System.out.println(n4);
        Double n5 = Double.valueOf("100.2345325");
        System.out.println(n5);
    }
}
```

### 拆箱和装箱

其实赋值没必要像上面那样麻烦

```java
Integer n1 = 100;//自动装箱，把100包装再复制
n1 += 100;//自动拆箱，i = i.valueInt()+100,再装箱
```

