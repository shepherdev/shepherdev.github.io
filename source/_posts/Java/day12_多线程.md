---
title: 
date: 2020-3-9
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
  - 多线程
---

# 进程与线程

- 启动一个程序就是一个进程，计算机都是多进程的
- 而进程有主线程和其他线程
- 也就是在进程里面有多线程
- 线程的作用：完成多任务，比如游戏角色和AI

### 主线程

- java的主线程都是从main方法里执行的，主线程是系统默认创建出来的（JVM）

> 其实CPU在同一时间只能运行一个线程，但他运算能力太快了(一秒几亿次)，多线程指的是CPU将每个线程的执行时间做了规划，这段时间执行那个线程，那段时间执行那个进程，人是感觉不到的

# 创建线程

## extends Thread

- 继承Thread
- 重写run方法
- 创建对象

- 调用start方法运行

```java
package com.shepherd.student01;

public class MyThread extends Thread {
    
    @Override
    public void run() { 
        for(int i=0; i<1000; i++) {
        System.out.println("MyThread:"+i);
        }
    }
}
```

运行

```java
package com.shepherd.student01;

public class CreateThread {
    public static void main(String[] args) {
        
        MyThread thread =new MyThread();
        thread.start();
        for(int i=0; i<1000; i++) {
            System.out.println("MainThread:"+i);
            }
    }
}
```

### 线程名字

线程通常有一个默认的名字，可以在创建线程时写一个构造方法来初始化名字

- 调用
  - `对象.getName();`
- 在类里
  - `getName();`

### 线程调度规则

- 分时调度（平均分配）

- 抢占式调度（按照优先级）
  - 优先级高的被执行的几率更高

### 设置优先级

```java
//获取
thread.getPriority();
//设置
thread.setPriority();
```

### 获取当前线程

```java
Thread.currentThread();
```

### 休眠

```java
thread.sleep(1000);
```

### 加入主线程

重新回归主线程的那条线，从上到下执行代码

```java
thread.join();
```

### 守护线程

顾名思义，若没有了其他线程，守护线程也会消失（不管有没有执行完代码）

守护线程的代码必须在启动代码前

```java
thread.setDaemon();
thread.start();
```

### 中断线程

```java
//直接击毙
thread.stop();
//改变标志,线程根据这个标志主动处理中断
thread.interrupt();
```

### 线程的生命周期

<img src="/home/shepherd/blog_pic/threadStat.png" alt="threadStat" style="zoom:50%;" />

### 特点

- 因为单继承，所以他无法继承别的类了

## implements Runnable

这个接口里面只有一个抽象方法run，继承它必须实现这个方法

```java
package com.shepherd.student01;

import org.junit.Test;

public class MyRunnable implements Runnable {
    
    @Override
    public void run() {
        
        Thread t = Thread.currentThread();
        for(int i=0; i<100; i++) {
            System.out.println(t.getName()+"-"+i);
        }
    }
}

//测试方法
   public void testRunnable() {
        
        MyRunnable r = new MyRunnable();
        
        Thread t1 = new Thread(r);
        Thread t2 = new Thread(r,"thread-2");
        
        t1.start();
        t2.start();
    }
}
```

### 特点

- 是一个接口，单继承多实现，可以继承别的类
- 线程之间数据共享，如上r，t1,t2实际上都会调用同一个对象的run方法

## 匿名内部类线程

```java
Runnable r = new Runnable() {

	@Override
	public void run() {
		for(int i=0; i<100; i++) {
		System.out.println(Thread.currentThread().getName()+":"+i);
		}
	}
};

//        Thread t = new Thread(r,"InnerClassThrad");
//        t.start();

new Thread(r,"InnerClassThrad").start();
```

```java
    new Thread() {
        
        @Override
        public void run() {
            for(int i=0; i<100; i++) {
                System.out.println(Thread.currentThread().getName()+":"+i);
            }
        }
    }.start();
```
# 线程安全

### 发现问题

先举一个售票的例子

> 四个售票场所共有100张票，使用多线程解决卖票

- 继承重写run方法类

```java
package com.shepherd.student01;

import org.junit.Test;

public class TicketThread extends Thread {
    
    private static int count = 100;
    
    public TicketThread() {}
    
    public TicketThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        
        while(true) {
            if(count>0) {
                System.out.println(Thread.currentThread().getName()+"卖出了第"+count+"票");
                count--;
            }else {
                break;
            }
        }
        
    }
}
```

创建四个线程

```java
package com.shepherd.student01;

import org.junit.Test;

public class Prctice {
    @Test
    public void test() {
        TicketThread t1 = new TicketThread("售票点1");
        TicketThread t2 = new TicketThread("售票点2");
        TicketThread t3 = new TicketThread("售票点3");
        TicketThread t4 = new TicketThread("售票点4");
        
        t1.start();
        t2.start();
        t3.start();
        t4.start();
    }
}
```

结果（多运行几次就会这样）

```
售票点1卖出了第100票
售票点2卖出了第100票
....
```

> 原因：多线程就是每个线程都在抢代码运行，以上面的例子，线程1在抢到输出后，count没有自减，然后线程2抢到了输出（此时count还是100），这就是线程不安全问题

### 解决方法

思想就是将代码上锁，将你需要执行的代码上“锁”，比如在执行2-3行代码时，先将这2-3行代码上锁，钥匙由线程来抢，这时其他线程没有钥匙，无法执行这2-3行代码，当抢得钥匙的线程执行完后将钥匙归还，这时再由线程抢钥匙来执行，这样就保证了线程安全，但在执行期间其他线程在等待就造成了效率问题

- 代码部分

继承Thread

```java
package com.shepherd.student01;

import org.junit.Test;

public class TicketThread extends Thread {
    
    private static int count = 100;
    private static Object lock = new Object();
    //对象不一样，锁必须一样
    
    public TicketThread() {}
    
    public TicketThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        
        while(true) {
            
            synchronized (lock) {
                if(count>0) {
                    System.out.println(Thread.currentThread().getName()+"卖出了第"+count+"票");
                    count--;
                }else {
                    break;
                }
            }
            //加入延时使售票点更加随机
            try {
                Thread.sleep(100);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

实现Runnable

```java
package com.shepherd.student01;

public class TicketRunnable implements Runnable {
    
    private int count = 100;

    @Override
    public void run() {
        
        while(true) {
            
            synchronized (this) {
                //this代表当前对象，对象一样锁也一样
                if(count>0) {
                    System.out.println(Thread.currentThread().getName()+"卖出了第"+count+"票");
                    count--;
                }else {
                    break;
                }
            }
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
    }

}
```

### synchronized

可以设置一个sycnhronized方法，将同步代码放在这个方法里面

```java
public synchronized void test(){
	//代码
}
```

### ReentrantLock

第二种锁

使用方法

- 创建对象

  `ReentrantLock lock = new ReentrantLock();`

- 加锁

  `lock.lock`

- 解锁

  `lock.unlock`

为了防止加锁的代码异常无法解锁，通常要使用try finally

> 当有两个线程类访问同一数据时，可以在构造时传递一把锁，这样不同的类就能使用同一把锁了

## 死锁

一个门两把锁，线程1拿了一把锁，线程2拿了一把锁，最终导致谁都打不开门

# 线程组

将线程进行统一管理

如

```java
package com.shepherd.student01;

public class MyThreadGroup {
    public static void main(String[] args) {
    
        ThreadGroup tg = new ThreadGroup("线程组");
        MyRunnable r = new MyRunnable();
        
        Thread t1 = new Thread(tg, r);   
        Thread t2 = new Thread(tg, r);   
        Thread t3 = new Thread(tg, r);   
        Thread t4 = new Thread(tg, r);   
        
        tg.setDaemon(true);
    }
}
```

# 定时器

```java
package com.shepherd.student01;

import java.util.Timer;
import java.util.TimerTask;

public class TestTimer {
    public static void main(String[] args) {
        //定时器对象
        Timer t = new Timer();
        //安排
        t.schedule(new MyTimerTask(), 2000, 1000);
    }
}

//创建一个定时任务类
class MyTimerTask extends TimerTask {
    
    @Override
    public void run() {
        System.out.println("这是一个定时任务");
    }
}
```

