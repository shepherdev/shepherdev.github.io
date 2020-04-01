---
title: 
date: 2020-3-6
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

## 可变参数

当我们需要的参数不确定，或则很多时，方法重载就不适用了，可变参数有两种形式

### 1.可变参数

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

### 2.数组参数

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

## IO流-1.0（文件操作）

也就是读取硬盘数据的操作，介绍一下文件

文件

- 文件
  - 文本文件，二进制文件
- 目录

数据存放的地方

- 运行的程序数据放在内存
- 照片，文档放在硬盘

### 什么是IO流

- 把数据从硬盘读取到内存==>`I`流
- 把数据从内存存到硬盘==>`O`流

### File

File可以指向一个文件，也可以指向目录

```java
package com.shepherd3.file;

import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        File f1 = new File("/home/shepherd");
        File f2 = new File("/home/shepherd/Java/src/day1/Math.java");
        //构造方法有很多，就不一一列举了
        System.out.println(f1.isDirectory());
        System.out.println(f1.isFile());
        System.out.println(f2.isFile());
        System.out.println(f2.exists());
        System.out.println(f2.canRead());
        System.out.println(f2.canWrite());
        System.out.println(f2.canExecute());
    }
}
```

创建和删除

```java
public class FileDemo {
    public static void main(String[] args) throws IOException {
        File f1 = new File("/home/shepherd/Java/Test.md");
        File f2 = new File("/home/shepherd/Java/Test");
        f1.createNewFile();
        f2.mkdir();
        f2.mkdirs();//创建多级目录
//        f1.delete();
        f1.renameTo(new File("/home/shepherd/Java/Test2.md"));
        //这个renameTo还可以移动文件，和linux的mv指令是真的像
        f2.delete();
    }
}
```

获取一些信息

```java
public class FileDemo {
    public static void main(String[] args) throws IOException {
        File f1 = new File("/home/shepherd/Java/Test2.md");
        File f2 = new File("Test");
//        f2.createNewFile();
        System.out.println(f1.getAbsolutePath());//绝对路径
        System.out.println(f2.getPath());//相对路径
        System.out.println(f1.getName());
        System.out.println(f1.length());//文件大小
        System.out.println(f1.lastModified());//获取最后修改时间，单位毫秒
        System.out.println(f1.getParent());
    }
}
```

list

```java
import java.io.File;
import java.io.IOException;

public class FileDemo {
    public static void main(String[] args) throws IOException {
        File dir = new File("/home/shepherd/Java_learn_workspace/Learn02");
        String[] s = dir.list();
        for(String i : s) {
            System.out.println(i);
        }
        File[] s2 = dir.listFiles();
        for(File i : s2) {
            System.out.println(i.getName());
        }
}
```

两个题目，觉得挺有意思，放上来

1.复制文件

```java
package com.shepherd4.practice;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;

public class CopyFiles {
    public static void main(String[] args) throws IOException {
        File source = new File("/home/shepherd/Java/Test2.md");
        File target = new File("/home/shepherd/Java/Test2");
        
        Files.copy(source.toPath(), target.toPath());
    }
}
```

2.递归查找文件

```java
package com.shepherd4.practice;

import java.io.File;

public class RTD {
    public static void TraversalDiractory(File parentDir) {
        File[]  temp = parentDir.listFiles(); 
        for(File file : temp) {
            if(file.isFile()) {
                System.out.print(file.getName()+"  ");
            }
        }
        System.out.println();
        for(File file : temp) {
             if(file.isDirectory()) {
                System.out.println(file.getPath()+"  ");
                TraversalDiractory(file.getAbsoluteFile());
            }
        }
    }
}
```

## IO流-2.0（文件内容操作）

​	数据类型

- 文本数据
- 二进制数据

数据流

- 字节流
  - 抽象基类：InputStream OutputStream
- 字符流
  - 抽象基类：Reader Writer

### 字节流

- 默认没有缓冲区（代表频繁与硬盘交互，浪费性能）
- 操作一个字节

#### 输入

- InputStream-->FileInputStream


```java
    public void testFileInputStream() {
        FileInputStream input = null;
        try {
            input = new FileInputStream("Test/test1.txt");
            //有一个指针，会自动改变指向
            //1.一字节的读取
//            while (true) {
//                int a = input.read();
//                if(a == -1)break;
//                System.out.print((char) a);
//            //注意，因为是字节流，所以读取中文会乱码，中文占两字节
//            }
            //2.字节数组的读取
            byte[] data = new byte[4];
            while(true) {
                int length = input.read(data);
                if(length==-1) break;
                //输出方法一
                for(int i=0; i<length; i++) {
                    System.out.print((char)data[i]);
                }
                //输出方法2
                String str = new String(data,0,length);
                //表示从data里的0开始取数据，长度为length
                System.out.print(str);
            }
        } catch (IOException e) {
            // TODO: handle exception
            e.printStackTrace();
        } finally {
            try {
                if (input != null) {
                    input.close();// 释放资源
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 输出

- OutputSteam-->FileOutputStream

```java
    @Test
    public void testFileOutputStream() {
        FileOutputStream output = null;
        try {
            //true表示追加模式
            output = new FileOutputStream("Test/test1.txt",true);
//            output.write(97);
            String str = new String("abcd");
            output.write(str.getBytes());
            //截取字串添加
            output.write(str.getBytes(), 1, 3);
        }catch (IOException e) {
            // TODO: handle exception
            e.printStackTrace();
        } finally {
            try {
                if(output != null)
                    output.close();
            }catch (Exception e) {
                // TODO: handle exception
                e.printStackTrace();
            }
        }
    }
```

文件文本的复制练习

- 我使用了字节数组完成
- 后面有对比单字节和字节数组效率的数据

```java
package com.shepherd.iostream;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class IOCopy {
    public static void copyFile(String source,String target) {
        FileInputStream input = null;
        FileOutputStream output = null;
        try {
            //input
            input = new FileInputStream(source);
            output = new FileOutputStream(target);
            byte[] data  = new byte[1024];
            int length=-1;
            while((length=input.read(data))>-1) {
                String str = new String(data,0,length);
//                System.out.println("读取成功");
                //output
                output.write(str.getBytes());
//                System.out.println("写入成功");
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(input!=null)
                   input.close();
            }catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(output!=null)
                    output.close();
            } catch(IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- 对比复制104.5M的压缩包

```java
public void testCopy() {
        long start = System.currentTimeMillis(); 
        copyFileByArray("Test/test.zip","Test/test2.zip");
        long end = System.currentTimeMillis();
        System.out.println(end-start);
        start = System.currentTimeMillis(); 
        copyFileByByte("Test/test.zip","Test/test3.zip");
        end = System.currentTimeMillis();
        System.out.println(end-start);
    }
```

结果（单位毫秒）

```
1024字节：5515
单字节：493533
```

#### 缓冲区

作用：节约性能，当缓冲区满时就会与硬盘交互

#### 缓冲输出流（包装流）

缓冲区的数据只有在下面三种情况才会写入硬盘，之所以说是包装流是因为它依赖于File

1. 满了
2. 调用flush刷新缓冲区
3. 调用close

```java 
package com.shepherd5.buffer;

import java.io.BufferedOutputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

import org.junit.Test;

public class TestBufferedOutputStream {
    @Test
    public void testBufferedOutputStream() {
        BufferedOutputStream bos = null;
        try {
            bos = new BufferedOutputStream(new FileOutputStream("Test/test1.txt"),1);
            //后面代表自定义缓冲区大小，一般使用默认就好了
            bos.write(97);
            String str = new String("hello");
            bos.write(str.getBytes());
            bos.flush();
            //建议调用flush，怕close出错
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(bos!=null)
                    bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 缓冲输入流

缓冲区的作用

- 读满缓冲区，然后你需要多少给你多少
- 节约性能

```java
public class BufferedIOStream {
    @Test
    public void testBufferedInputStream() {
        BufferedInputStream bis = null;
        try {
            bis = new BufferedInputStream(new FileInputStream("Test/test1.txt"));
//            int data = bis.read();
//            System.out.println(data);
            byte[] data = new byte[1024];
            int length = bis.read(data);
            String str = new String(data,0,length);
            System.out.println(str); 
        }  catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 }
```

### 字符流

- 操作一个Unicode码元（2字节）
- 默认是有缓冲区的，但不能设置大小

#### 输出

- Writer-->OutputStreamWriter（包装流）
- 构造OutputStreamWriter对象时，可以指定编码，默认系统编码

```java
package com.shepherd.iostream;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

import org.junit.Test;

public class IO_WriterRead {
    @Test
    public void testOutputStreamWriter() {
        OutputStreamWriter writer = null;
        try {
            FileOutputStream fileOutput = new FileOutputStream("Test/test1.txt");
            writer = new OutputStreamWriter(fileOutput);
            //可以传递两个参数，另一个是编码
            writer.write("你好");
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally { 
            try { 
                if(writer!=null); 
                	writer.close(); 
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
    }
}
```

#### 输入

- Reader-->InputStreamWriter（包装流）
- 构造InputStreamWriter对象时，可以指定编码，默认系统编码

```java
    @Test
    public void testInputStreamReader() {
        InputStreamReader reader = null;
        try {
            reader = new InputStreamReader(new FileInputStream("Test/test1.txt"),"utf-8");
            char[] data = new char[1024];
            int length = reader.read(data); 
            System.out.println(new String(data,0,length));
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } finally {
            try {
                if(reader!=null)
                    reader.close(); 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        } 
    }
```

#### FileWriter

- Writer-->OutputStreamWriter-->FileWriter
- 相对与`OutputStreamWriter`，`FileWriter`构造时时不需要`new FileOutputStream`，因为它在调用父类构造方法时自动`new FileOutputStream()`

例子

```java
@Test
public void testFileWriter() {

    FileWriter writer = null;
    try {
        writer = new FileWriter("Test/test1.txt");

        writer.write("hello,我是shepherd!");

    } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    } finally {
        try {
            if(writer!=null)
                writer.close();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }  
}
```

#### FileReader

- Reader-->InputStreamReader-->FileReader
- 构造简单

例子

```java
public void testFileReader() {
    FileReader reader = null;
    try {
        reader = new FileReader("Test/test1.txt");
        char[] data = new char[1024];
        int length = reader.read(data);
        System.out.println(new String(data,0,length));
    } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    } finally {
        try {
            if(reader!=null)
                reader.close();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

- 如果要设定编码，则需要在构造时给出Charset型的数据即可


如

`reader = new FileReader("Test/test1.txt",Charset.forName("GBK"));`

#### BufferedWriter

- Writer-->BufferedWriter

- 可以设置缓冲区大小
- 有一个`readLine()`

#### BufferedReader

- Reader-->BufferedReader

- 可以设置缓冲区大小