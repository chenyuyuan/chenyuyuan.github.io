---
title: Java-Socket
date: 2019-08-05 16:54:11
tags: Java
---

##### 基于TCP协议实现的网络通信的类：

客户端Socket类和服务端ServerSocket类

##### 通信的一般步骤：

(1)先在服务器端生成一个ServerSocket实例对象，并通过accept()方法随时监听客户端的连接请求。
(2)当客户端需要连接时，相应地要生成一个Socket实例对象，并发出连接请求，其中host参数指明该主机名，port参数指明该主机端口号。
(3)服务器端通过accept()方法接收到客户端的请求后，开辟一个接口与之进行连接，并生成所需的I/O数据流。
(4)客户端和服务器端的通信都是通过一对InputStream和OutputStream进行的。通信结束后，两端分别关闭对象的Socket接口。

##### 客户端Socket类：

客户端可以通过构造一个Socket类对象来建立与服务器的连接。

Socket类的构造方法：

public Socket (String address, int port)
public Socket(InetAddress address, int port)
public Socket(String host, int port, InetAddress  localAddr, int localPort)
public Socket(InetAddress address, int port, InetAddress localAddr, int localPort)

Socket类的主要方法：

void close()  关闭Socket连接

InetAddress getInetAddress()  获取当前连接的远程主机的Internet地址

InputStream getInputStream()  获取Socket对应的输入流

InetAddress getLocalAddress()  获取本地主机的Internet地址

int getLocalPort()  获取本地连接的端口号

OutputStream getOutputStream()  获取Socket的输出流

int getPort()  获取远程主机端口号

void shutdownInput()  关闭输入流

void shutdownOutput()  关闭输出流

##### ServerSocket类：

ServerSocket类用在服务器端，用来监听所有来自**指定端口**的连接，并为每个新的连接创建一个Socket，之后客户端便可以与服务器开始通信了。

ServerSocket类的构造方法：
public ServerSocket(int port)  在指定端口上创建一个ServerSocket类对象。
public ServerSocket(int port, int backlog)  在指定端口上创建一个ServerSocket类对象，并进入监听状态，参数backlog是服务器忙时保持连接请求的等待客户数量。
public ServerSocket(int port, int backlog, InetAddress bindAddr)  使用指定的端口和和要绑定到的服务器 IP 地址创建一个ServerSocket类对象，并进入监听状态。

ServerSocket类的主要方法：

Socket accept()  接收该连接并返回该连接的Socket对象

void close()  关闭此服务器的Socket

InetAddress getInetAddress()  获取该服务器Socket所绑定的地址

int getLocalPort()  获取该服务器Socket所监听的端口号

int getSoTimeout()  获取连接的超时数

void setSoTimeout(int timeout)  设置连接的超时数，参数表示 ServerSocket的 accept()方法等待客户连接的超时时间。如果参数值为0 , 表示永远不会超时，进入阻塞状态，这也是它的默认值

##### Example:

一个很简单的例子：

Server端:

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class Server {
    ServerSocket ss = null;
    Socket socket;
    InputStream in;
    OutputStream out;
    DataInputStream din;
    DataOutputStream dout;
    public Server() {
        try {
            ss = new ServerSocket(10000);
            System.out.println("Waiting for connecting......");
            socket = ss.accept();
            System.out.println("Connected!");
            in = socket.getInputStream();
            out = socket.getOutputStream();
            din = new DataInputStream(in);
            dout = new DataOutputStream(out);
            dout.writeUTF("Hello!");
            String name = din.readUTF();
            String s = din.readUTF();
            System.out.println(name + s);
            in.close();;
            out.close();
            din.close();
            dout.close();
            ss.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static void main(String[] args) {
        Server server = new Server();
    }
}
```

Client端: 

```java
import java.io.*;
import java.net.Socket;

public class Client {
    Socket socket;
    InputStream in;
    OutputStream out;
    DataInputStream din;
    DataOutputStream dout;
    public Client() {
        try {
            socket = new Socket("127.0.0.1", 10000);
            in = socket.getInputStream();
            out = socket.getOutputStream();
            din = new DataInputStream(in);
            dout = new DataOutputStream(out);
            din.readUTF();
            dout.writeUTF("cyuyuan");
            dout.writeUTF("hello, world!");
            in.close();;
            out.close();
            din.close();
            dout.close();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static void main(String[] args) {
        Client client = new Client();
    }
}
```

