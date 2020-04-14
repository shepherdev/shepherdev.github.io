---
title: 网络编程
date: 2020-3-10
author: shepherd
toc: ture
categories: [Java,多线程]
---

## ip与port

- ip可以访问我们的设备
- 端口可以访问到设备里的程序

<!-- more -->

## 通信协议

### UDP

- 通信快
- 不需要建立连接
- 不可靠

### TCP

- 通信慢
- 三次握手建立连接
- 可靠

> UDP就像发短信，TCP就像打电话是

## Socket套接字

在程序中使用Socket进行通信，用来指定IP,port，协议

数据发送的两步：

- 监听：等待数据发送过来，需要指定监听的端口
- 发送：指定发送到哪台计算机，哪个端口

socket分为接受端（服务器）和发送端（客户）

# UDP

## 发送端

```java
package com.shepherd.student02;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

public class UDP_Send {
    public static void main(String[] args) throws Exception {
        
        DatagramSocket ds = new DatagramSocket();
        InetAddress address = InetAddress.getByName("localhost");
        //指定接受端的端口
        int port = 9001;
        
        byte[] buf = "测试".getBytes();
        int length = buf.length;
        //数据打包
        DatagramPacket p = new DatagramPacket(buf, length, address, port);

        ds.send(p);
        ds.close();
    }
}
```

## 接受端

```java
package com.shepherd.student02;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDP_Receive {

    public static void main(String[] args) throws Exception {
        
        //设置监听端口
        DatagramSocket ds = new DatagramSocket(9001);
        byte[] buf = new byte[1024];
        int length = buf.length;
        DatagramPacket p = new DatagramPacket(buf, length);
        ds.receive(p);
        
        System.out.println(new String(p.getData(),0,length));
        ds.close();
    }

}
```

> Linux的端口在1024以内需要root才能执行，或者关闭SELinux
>
> 发送端的端口号是系统自动分配的

## 循环接受

```java
package com.shepherd.student02;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDP_Receive {

    public static void main(String[] args) throws Exception {
        
        DatagramSocket ds = new DatagramSocket(9001);
        
        while(true) {
        byte[] buf = new byte[1024];
        int length = buf.length;
        DatagramPacket dp = new DatagramPacket(buf, length);
        ds.receive(dp);
        
        InetAddress address = dp.getAddress();
        String str = new String(dp.getData(),0,length);
        System.out.println(address+":"+str);
        }
        
//        ds.close();
    }

}
```

## 循环发送

```java
package com.shepherd.student02;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;
import java.util.Scanner;

public class UDP_Send {
    public static void main(String[] args) throws Exception {
        
        DatagramSocket ds = new DatagramSocket();
        InetAddress address = InetAddress.getByName("localhost");
        int port = 9001;
        while(true) {
            
            Scanner s = new Scanner(System.in);
            String str = s.nextLine();
            if(str.equals("end"))break;
            
            byte[] buf = str.getBytes();
            int length = buf.length;
            DatagramPacket p = new DatagramPacket(buf, length, address, port);
            ds.send(p);
        }
        ds.close();
    }
}
```

# TCP

举一个双向聊天例子好了

## 发送端（client） 

```java
package com.shepherd.student02;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

public class TCP_Send {
    public static void main(String[] args) throws Exception {
        //建立连接
        Socket ts = new Socket("localhost",9001);
        OutputStream out = ts.getOutputStream();
        InputStream in = ts.getInputStream();
        Scanner s  = new Scanner(System.in);
        
        byte[] buf  = new byte[1024];
        new Thread() {
            public void run() {
                int length = -1;
                try {
                    while((length  = in.read(buf))!=-1) {
                        String s = new String(buf, 0, length);
                        System.out.println(s);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            };
        }.start();
        
        while(true) {
            String str = s.nextLine();
            if(str.equals("end"))break;
                out.write(str.getBytes());
        }
        
        ts.close();
        
    }
}
```

## 接收端（server）

```java
package com.shepherd.student02;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.Scanner;

public class TCP_Receive {
    public static void main(String[] args) throws Exception {
        
        ServerSocket ss = new ServerSocket(9001);
        //接受并建立发送端的连接
        Socket client = ss.accept();
        System.out.println("连接成功");
        Scanner s = new Scanner(System.in);
        
        InputStream input  = client.getInputStream();
        OutputStream output = client.getOutputStream();
        
        byte[] buf = new byte[1024];
        
        new Thread() {
            
            public void run() {
                while(true) {
                    try {
                        String str = s.nextLine();
                        if(str.equals("end"))break;
                            output.write(str.getBytes());
                    } catch (Exception e) {
                        e.printStackTrace();
                    }          
                }
            };
        }.start();
        
        int length = -1;
        while((length  = input.read(buf))!=-1) {
            String str = new String(buf, 0, length);
            System.out.println(s);
        }
        
        client.close();
        ss.close();
    }
}
```

> TCP的客户端可以有多个，服务端只能有一个（需要循环监听端口），在循环监听端口时监听到一个端口就可以启动客户端线程操作