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

<!-- more -->

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

但是，这样的实现方式只能建立一个连接，Server端的accept()方法时阻塞式的，也就是说accept()方法会阻塞的等待，直到有Client端连接。如果要建立多个连接，那我们可以用多线程的方法来连接，或者使用Java的Nio的非阻塞的方法。

##### note：

需要注意的是，从Socket的输入流中读取数据并不能读取文件那样，一直调用read()方法直到返回-1为止，因为对Socket而言，只有当服务端关闭连接时，Socket的输入流才会返回-1，而是事实上服务器并不会不停地关闭连接。假设我们想要通过一个连接发送多个请求，那么在这种情况下关闭连接就显得非常愚蠢。

因此，从Socket的输入流中读取数据时我们必须要知道需要读取的字节数，这可以通过让服务器在数据中告知发送了多少字节来实现，也可以采用在数据末尾设置特殊字符标记的方式连实现。

##### 非阻塞的ServerSocketChannel：

套接字通道多路复用的思想是创建一个Selector，将多个通道对它进行注册，当套接字有关注的事件发生时，可以选出这个通道进行操作。

服务器端的代码如下，相关说明就带在注释里了:

```java
// 创建一个选择器，可用close()关闭，isOpen()表示是否处于打开状态，他不隶属于当前线程
Selector selector = Selector.open();
// 创建ServerSocketChannel，并把它绑定到指定端口上
ServerSocketChannel server = ServerSocketChannel.open();
server.bind(new InetSocketAddress("127.0.0.1", 7777));
// 设置为非阻塞模式, 这个非常重要
server.configureBlocking(false);
// 在选择器里面注册关注这个服务器套接字通道的accept事件
// ServerSocketChannel只有OP_ACCEPT可用，OP_CONNECT,OP_READ,OP_WRITE用于SocketChannel
server.register(selector, SelectionKey.OP_ACCEPT);
while (true) {
	// 测试等待事件发生，分为直接返回的selectNow()和阻塞等待的select()，另外也可加一个参数表示阻塞超时
	// 停止阻塞的方法有两种: 中断线程和selector.wakeup()，有事件发生时，会自动的wakeup()
	// 方法返回为select出的事件数(参见后面的注释有说明这个值为什么可能为0).
	// 另外务必注意一个问题是，当selector被select()阻塞时，其他的线程调用同一个selector的register也会被阻塞到select返回为止
	// select操作会把发生关注事件的Key加入到selectionKeys中(只管加不管减)
	if (selector.select() == 0) { //
		continue;
	}
    // 获取发生了关注时间的Key集合，每个SelectionKey对应了注册的一个通道
    Set<SelectionKey> keys = selector.selectedKeys();
    // 多说一句selector.keys()返回所有的SelectionKey(包括没有发生事件的)
    for (SelectionKey key : keys) {
        // OP_ACCEPT 这个只有ServerSocketChannel才有可能触发
        if (key.isAcceptable()) {
            // 得到与客户端的套接字通道
            SocketChannel channel = ((ServerSocketChannel) key.channel()).accept();
            // 同样设置为非阻塞模式
            channel.configureBlocking(false);
            // 同样将于客户端的通道在selector上注册，OP_READ对应可读事件(对方有写入数据),可以通过key获取关联的选择器
            channel.register(key.selector(), SelectionKey.OP_READ, ByteBuffer.allocate(1024));
        }
        // OP_READ 有数据可读
        if (key.isReadable()) {
            SocketChannel channel = (SocketChannel) key.channel();
            // 得到附件，就是上面SocketChannel进行register的时候的第三个参数,可为随意Object
            ByteBuffer buffer = (ByteBuffer) key.attachment();
            // 读数据 这里就简单写一下，实际上应该还是循环读取到读不出来为止的
            channel.read(buffer);
            // 改变自身关注事件，可以用位或操作|组合时间
            key.interestOps(SelectionKey.OP_READ | SelectionKey.OP_WRITE);
        }
        // OP_WRITE 可写状态 这个状态通常总是触发的，所以只在需要写操作时才进行关注
        if (key.isWritable()) {
            // 写数据掠过，可以自建buffer，也可用附件对象(看情况),注意buffer写入后需要flip
            // ......
            // 写完就吧写状态关注去掉，否则会一直触发写事件
            key.interestOps(SelectionKey.OP_READ);
        }

        // 由于select操作只管对selectedKeys进行添加，所以key处理后我们需要从里面把key去掉
        keys.remove(key);
    }
}
```

这里需要着重说明一下select操作做了什么(根据现象推的，具体好像没有找到这个的文档说明)，他每次检查keys里面每个Key对应的通道的状态，如果有关注状态时，就决定返回，这时会同时将Key对象加入到selectedKeys中，并返回selectedKeys本次变化的对象数(原本就在selectedKeys中的对象是不计的)，由于一个Key对应一个通道(可能同时处于多个状态，所以注意上面的if语句我都没有写else)，所以select返回0也是有可能的。另外OP_WRITE和OP_CONNET这两个状态是不能长期关注的，只在有需要的时候监听，处理完必须马上去掉。如果没有发现有任何关注状态，select会一直阻塞到有状态变化或者超时什么的。
SelectionKey的其他几个方法，attach(Object)为key设置附件，并返回之前的附件；interestOps()和readyOps()返回关注状态和当前状态；cancel()为取消注册；isValid()表示key是否有效(在key取消注册，通道关闭，选择器关闭这三个事情发生之前，key均为有效的，但不包括对方关闭通道，所以读写应注意异常)。

还有一个状态上面没有使用，OP_CONNECT这个主要是用于客户端，对应的key的方法是isConnectable()表示已经创建好了连接。

非阻塞实现的客户端如下:

```java
Selector selector = Selector.open();
// 创建一个套接字通道，注意这里必须使用无参形式
SocketChannel channel = SocketChannel.open();
// 设置为非阻塞模式，这个方法必须在实际连接之前调用(所以open的时候不能提供服务器地址，否则会自动连接)
channel.configureBlocking(false);
// 连接服务器，由于是非阻塞模式，这个方法会发起连接请求，并直接返回false(阻塞模式是一直等到链接成功并返回是否成功)
channel.connect(new InetSocketAddress("127.0.0.1", 7777));
// 注册关联链接状态
channel.register(selector, SelectionKey.OP_CONNECT);
while (true) {
	// 前略 和服务器端的类似
	// ...
	// 获取发生了关注时间的Key集合，每个SelectionKey对应了注册的一个通道
	Set<SelectionKey> keys = selector.selectedKeys();
	for (SelectionKey key : keys) {
		// OP_CONNECT 两种情况，链接成功或失败这个方法都会返回true
		if (key.isConnectable()) {
			// 由于非阻塞模式，connect只管发起连接请求，finishConnect()方法会阻塞到链接结束并返回是否成功
			// 另外还有一个isConnectionPending()返回的是是否处于正在连接状态(还在三次握手中)
			if (channel.finishConnect()) {
				// 链接成功了可以做一些自己的处理，略
				// ...
				// 处理完后必须吧OP_CONNECT关注去掉，改为关注OP_READ
				key.interestOps(SelectionKey.OP_READ);
			}
		}
		// 后略 和服务器端的类似
		// ...
	}
}
```


​    虽然例子是这样的，不过服务器和客户端可以自己单方面选择是否采用非阻塞模式，用阻塞模式的客户端连接非阻塞模式的服务器端是OK的。

二 NIO2的异步IO通道

以下API是由Java7提供。老版本无法使用。

异步IO通道的实现有两种实现方式，一是在阻塞模式的原方法(主要指的是read和write,具体可以查看API文档)上传于一个CompletionHandler实例以实现回调，另外也可以令其返回一个Future实例(Java5新增同步工具包java.util.concurrent中的API)，然后再适当的时候通过其get方法来获取返回的结果。异步文件I/O通道为AsynchronousFileChannel，而异步套接字通道为AsynchronousServerSocketChannel，分别对应其各自的原始通道。

异步I/O需要与一个AsynchronousChannelGroup对象关联，他实质上就是一个用于I/O的线程池。AsynchronousChannelGroup对象可以通过其自身静态方法的withThreadPool()，withCachedThreadPool()，withFixedThreadPool()提供一个线程池来创建(线程池也是Java5新增同步工具包java.util.concurrent中的API)。在异步通道创建open()时，可将这个对象传入进行关联。如果没有提供这个对象的话，就默认使用系统分组。但是需要注意的是系统分组的线程池是个守护线程池，JVM是可能在没有读写完成前正常结束的。AsynchronousChannelGroup在使用完后需要shutdowm()，这方面和线程池的关闭是类似的。