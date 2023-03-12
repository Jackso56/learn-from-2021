# netty概述
## netty介绍

1.  Netty 是由 JBOSS 提供的一个 **Java 开源框架**，现为 **Github 上的独立项目**。 
2. Netty 是一个**异步**的、**基于事件驱动**的网络应用框架，用以快速开发高性能、高可靠性的网络 IO 程序。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677042654893-63831e6e-3fe5-4fa0-8635-e4b48ef3a9b3.png#averageHue=%23e3efcd&clientId=u59b583c3-1ce0-4&from=paste&height=279&id=uead72ff8&name=image.png&originHeight=558&originWidth=1072&originalType=binary&ratio=2&rotation=0&showTitle=false&size=265819&status=done&style=shadow&taskId=u5e06c02c-bd71-4b76-90f2-51e55d7a53b&title=&width=536)

3. Netty 主要针对在 TCP 协议下，面向 Clients 端的高并发应用，或者 Peer-to-Peer 场景下的大量数据持续传输的 应用。 
4.  Netty 本质是一个 NIO 框架，适用于服务器通讯相关的多种应用场景

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677042732967-fdfe91fa-5101-495d-9ed7-c57a4ae03f80.png#averageHue=%23dee8e1&clientId=u59b583c3-1ce0-4&from=paste&height=144&id=u8397aeca&name=image.png&originHeight=287&originWidth=343&originalType=binary&ratio=2&rotation=0&showTitle=false&size=63114&status=done&style=shadow&taskId=ubd1b7011-f242-4857-a751-6ae71d321ec&title=&width=171.5)

5. 要透彻理解 Netty ， 需要先学习 NIO ， 这样我们才能阅读 Netty 的源码。
## netty应用场景
### 互联网行业

1.  互联网行业：在分布式系统中，各个节点之间需要远程服务调用，高性能的 RPC 框架必不可少，Netty 作为 异步高性能的通信框架，往往作为基础通信组件被这些 RPC 框架使用。 
2.  典型的应用有：阿里分布式服务框架 Dubbo 的 RPC 框架使用 Dubbo 协议进行节点间通信，Dubbo 协议默 认使用 Netty 作为基础通信组件，用于实现各进程节点之间的内部通信
### 游戏行业

1. 无论是手游服务端还是大型的网络游戏，Java 语言得到了越来越广泛的应用 
2. Netty 作为高性能的基础通信组件，提供了 TCP/UDP 和 HTTP 协议栈，方便定制和开发私有协议栈，账号登录服务器 
3. 地图服务器之间可以方便的通过 Netty 进行高性能的通信
### 大数据领域

1. 经典的 Hadoop 的高性能通信和序列化组件 Avro 的 RPC 框架，默认采用 Netty 进行跨界点通信 
2. 它的 Netty Service 基于 Netty 框架二次封装实现。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677042986736-38622873-176b-42c2-9a5a-fde2692e7d2a.png#averageHue=%238cb86e&clientId=u59b583c3-1ce0-4&from=paste&height=225&id=uc86356e5&name=image.png&originHeight=449&originWidth=864&originalType=binary&ratio=2&rotation=0&showTitle=false&size=312795&status=done&style=shadow&taskId=ud0b4aad4-02e2-4f03-879f-867b49155d6&title=&width=432)
### 其它开源项目使用到 Netty
> 网址: [https://netty.io/wiki/related-projects.html](https://netty.io/wiki/related-projects.html)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677043090078-abfb0ad7-d6f4-48bf-8a60-86df0c56f6f9.png#averageHue=%23eeeeee&clientId=u59b583c3-1ce0-4&from=paste&height=315&id=u3a79d61f&name=image.png&originHeight=630&originWidth=1752&originalType=binary&ratio=2&rotation=0&showTitle=false&size=232752&status=done&style=shadow&taskId=u9e6bfad1-5ad5-474b-9f64-ab6853cd544&title=&width=876)
## 工具书推荐
《Netty In Action》侧重实战，《Netty权威指南》侧重原理解析
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677043125583-90d1fbf4-fae9-4495-aff5-6e2e8cd1db53.png#averageHue=%23d2bea8&clientId=u59b583c3-1ce0-4&from=paste&height=288&id=u72249d3b&name=image.png&originHeight=307&originWidth=745&originalType=binary&ratio=2&rotation=0&showTitle=false&size=157283&status=done&style=shadow&taskId=u6bd20171-ece1-4375-a374-a802df5c003&title=&width=698.5)

# Java BIO编程
## I\O模型
### I\O模型简介

1. I/O 模型简单的理解：就是用什么样的通道进行数据的发送和接收，很大程度上决定了程序通信的性能 
2. Java 共支持 3 种网络编程模型/IO 模式：BIO、NIO、AIO 
3. Java BIO ： 同步并阻塞(传统阻塞型)，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器 端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677043461443-ea7651a0-dcfc-4d0a-b4b1-539b052bf9e0.png#averageHue=%23d1deb3&clientId=u59b583c3-1ce0-4&from=paste&height=494&id=ufdf09d3e&name=image.png&originHeight=377&originWidth=510&originalType=binary&ratio=2&rotation=0&showTitle=false&size=130218&status=done&style=shadow&taskId=u95707630-5a89-489b-a2b6-51746d627af&title=&width=668)

4. Java NIO ： 同步非阻塞，服务器实现模式为一个线程处理多个请求(连接)，即客户端发送的连接请求都会注 册到多路复用器上，多路复用器轮询到连接有 I/O 请求就进行处理

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677043517681-cea77589-3ba5-4c99-b9e8-8c1f21b99cde.png#averageHue=%23e3e9d1&clientId=u59b583c3-1ce0-4&from=paste&height=345&id=u2a738f70&name=image.png&originHeight=463&originWidth=938&originalType=binary&ratio=2&rotation=0&showTitle=false&size=265428&status=done&style=shadow&taskId=uf67e671f-8c91-4bb2-b53e-fe6e307fe00&title=&width=699)

5. Java AIO(NIO.2) ： 异步非阻塞，AIO 引入异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用
### I\O模型应用场景

1. BIO 方式适用于**连接数目比较小且固定**的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4 以前的唯一选择，但程序简单易理解。 
2. NIO 方式适用于**连接数目多且连接比较短**（轻操作）的架构，比如聊天服务器，弹幕系统，服务器间讯等。编程比较复杂，JDK1.4 开始支持。
3. AIO 方式使用于**连接数目多且连接比较长**（重操作）的架构，比如相册服务器，充分调用 OS 参与并发操作，编程比较复杂，JDK7 开始支持。
## Java BIO介绍

1. Java **_BIO _**就是传统的 **java io 编程**，其相关的类和接口在 java.io 
2. BIO(**blocking I/O**) ： 同步阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务端就需 要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，可以通过线程池机制改善(实 现多个客户连接服务器)。
3. BIO 方式适用于连接数目比较小且固定的架构，这种方式对**服务器资源要求比较高**，并发局限于应用中，JDK1.4 以前的唯一选择，程序简单易理解
## Java BIO工作机制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677044000007-73d2f918-3b12-425e-93fb-912b425abc0e.png#averageHue=%23585a67&clientId=u59b583c3-1ce0-4&from=paste&height=343&id=ue55489bc&name=image.png&originHeight=451&originWidth=907&originalType=binary&ratio=2&rotation=0&showTitle=false&size=228750&status=done&style=shadow&taskId=u2b728368-147f-4836-9b11-7cf1996b564&title=&width=689.5)
**BIO流程梳理**

1. 服务器端启动一个ServerSocket 
2. 客户端启动Socket对服务器进行通信，默认情况下服务器端需要对每个客户建立一个线程与之通讯
3. 客户端发出请求后,先咨询服务器是否有线程响应，如果没有则会等待，或者被拒绝
4. 如果有响应，客户端线程会等待请求结束后，在继续执行
## Java BIO 应用实例
### 实例步骤

1. 使用BIO模型编写一个服务器端，监听6666端口，当有客户端连接时，就启动一个线程与之通讯。
2. 要求使用线程池机制改善，可以连接多个客户端. 
3. 服务器端可以接收客户端发送的数据(telnet方式即可)。 
4. 代码演示
```java
package com.linmu.bio;

import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
* @Author: Jackson Black
* @CreateTime: 2023-02-22  09:13:48
*/
public class BlackInputOutput {
    public static void main(String[] args) {
        ExecutorService threadPool = null;
        ServerSocket service = null;
        try {
            threadPool = Executors.newCachedThreadPool();
            service = new ServerSocket(9999);
            System.out.println("service is running...");
            while (true) {
                System.out.println("waiting for connection....");
                final Socket socket = service.accept();
                System.out.println("connection is successfully...");
                threadPool.execute(() -> IOHander(socket));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void IOHander(Socket socket) {
        System.out.println("threadId:" + Thread.currentThread().getId() + ",threadname:" + Thread.currentThread().getName());
        try {
            byte[] bytes = new byte[1024];
            InputStream input = socket.getInputStream();
            System.out.println("waiting for read...");
            while (true) {
                int read = input.read(bytes);
                System.out.println("reading....");
                if (read != -1) {
                    System.out.println(new String(bytes, 0, read));
                } else {
                    break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                socket.close();
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }
}

```
### 操作步骤
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677045365859-f033e3c5-ced5-41ca-b09c-692e4589f61f.png#averageHue=%23333333&clientId=u59b583c3-1ce0-4&from=paste&height=214&id=uf7d1443a&name=image.png&originHeight=427&originWidth=1412&originalType=binary&ratio=2&rotation=0&showTitle=false&size=46353&status=done&style=shadow&taskId=u1c7da1ce-f270-4008-935d-6f222bae93e&title=&width=706)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677045487379-7201f091-f2b8-46cc-b4fb-0d76bbd0ffec.png#averageHue=%23424241&clientId=u59b583c3-1ce0-4&from=paste&height=290&id=u018bdcb2&name=image.png&originHeight=579&originWidth=1367&originalType=binary&ratio=2&rotation=0&showTitle=false&size=58651&status=done&style=shadow&taskId=u0b9476f3-5cc8-4ea9-a32f-3bc99824751&title=&width=683.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677045148266-56cf66b3-255b-46d5-a4df-e4f7dbde4ec4.png#averageHue=%23383736&clientId=u59b583c3-1ce0-4&from=paste&height=442&id=u0d3c0ed9&name=image.png&originHeight=884&originWidth=1800&originalType=binary&ratio=2&rotation=0&showTitle=false&size=168727&status=done&style=shadow&taskId=u5f064aa4-bc74-4745-8e81-94bf516541c&title=&width=900)
### Java BIO 存在的问题

1. 每个请求都需要创建独立的线程，与对应的客户端进行数据 Read，业务处理，数据 Write 。
2. 当并发数较大时，需要创建大量线程来处理连接，系统资源占用较大。
3. 连接建立后，如果当前线程暂时没有数据可读，则线程就阻塞在 Read 操作上，造成线程资源浪费

# Java NIO编程
## Java NIO 基本介绍

1. Java NIO 全称 **java non-blocking IO**，是指 JDK 提供的新 API。从 JDK1.4 开始，Java 提供了一系列改进的 输入/输出的新特性，被统称为 NIO(即 New IO)，是同步非阻塞的 
2. NIO 相关类都被放在 **java.nio **包及子包下，并且对原 java.io 包中的很多类进行改写。【基本案例】 
3. NIO 有三大核心部分：**Channel(通道)，Buffer(缓冲区), Selector(选择器**) 
4. **NIO 是 面向缓冲区 **，**或者面向 块 编程的**。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动，这就增加了处理过程中的灵活性，使用它可以提供非阻塞式的高伸缩性网络 
5. Java NIO 的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果 目前没有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。【后面有案例说明】 
6. 通俗理解：NIO 是可以做到用一个线程来处理多个操作的。假设有 10000 个请求过来,根据实际情况，可以分配50 或者 100 个线程来处理。不像之前的阻塞 IO 那样，非得分配 10000 个。 
7. HTTP2.0 使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比 HTTP1.1 大了好 几个数量级
8. Buffer案例说明
```java
package com.linmu.nio;

import java.nio.IntBuffer;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/23 12:33:15
 **/
@SuppressWarnings({"all"})
public class TestIntBuffer {
    public static void main(String[] args) {
        /**
         * IntBuffer 是一个抽象类
             * public abstract class IntBuffer
             *     extends Buffer
             *     implements Comparable<IntBuffer>
         * 通过传入容量，创安静IntBuffer
             * public static IntBuffer allocate(int capacity)
         */
        // 创建IntBuffer
        IntBuffer intBuffer = IntBuffer.allocate(5);

        /**
         * intBuffer.capacity()获取intBuffer的容量
         */
        // 添加元素
        for (int i = 0; i < intBuffer.capacity(); i++) {
            intBuffer.put(i+1);
        }

        /**
         * intBuffer.flip()用于读写切换
         */
        intBuffer.flip();

        /**
         * public final boolean hasRemaining()
         * 判断intBuffer内是否还有元素
         */
        while (intBuffer.hasRemaining()){
            System.out.println(intBuffer.get());
        }
    }
}

```
## NIO 与 BIO 的比较

1.  BIO 以流的方式处理数据,而 NIO 以块的方式处理数据,块 I/O 的效率比流 I/O 高很多 
2. BIO 是阻塞的，NIO 则是非阻塞的 
3. BIO 基于字节流和字符流进行操作，而 NIO 基于 Channel(通道)和 Buffer(缓冲区)进行操作，数据总是从通道 读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件（比如：连接请求,数据到达等），因此使用单个线程就可以监听多个客户端通道
## NIO 三大核心原理示意图
![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1677129394636-70233b5e-942a-4cd9-9849-747475b91b7a.jpeg)

1. 每个 channel 都会对应一个 Buffer 
2. Selector 对应一个线程， 一个线程对应多个 channel(连接) 
3. 该图反应了有三个 channel 注册到 该 selector //程序 
4. 程序切换到哪个 channel 是有事件决定的, Event 就是一个重要的概念 
5. Selector 会根据不同的事件，在各个通道上切换 
6. Buffer 就是一个内存块 ， 底层是有一个数组 
7. 数据的读取写入是通过 Buffer, 这个和 BIO , BIO 中要么是输入流，或者是 输出流, 不能双向，但是 NIO 的 Buffer 是可以读也可以写, 需要 flip 方法切换 channel 是双向的, 可以返回底层操作系统的情况, 比如 Linux ， 底层的操作系统 通道就是双向的.
## 缓冲区（Buffer）
### 基本介绍
缓冲区（Buffer）：缓冲区本质上是一个**可以读写数据的内存块**，可以理解成是一个**容器对象(含数组)**，该对象提供了一组方法，可以更轻松地使用内存块，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。Channel 提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由 Buffer
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677215569509-410b4a99-44d2-4d79-b162-27dfb59ff65b.png#averageHue=%23ced0cc&clientId=u0bca6045-c70c-4&from=paste&height=140&id=u093b1cea&name=image.png&originHeight=110&originWidth=569&originalType=binary&ratio=2&rotation=0&showTitle=false&size=42488&status=done&style=shadow&taskId=ueed83719-06f6-4cc3-af06-422d60b7493&title=&width=723.5)
### Buffer类及其子类

1. 在 NIO 中，Buffer 是一个顶层父类，它是一个抽象类, 类的层级关系图:
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677215636080-3cc5ee28-afff-4bbf-8afc-94bc9ee56ba4.png#averageHue=%23dfdcd5&clientId=u0bca6045-c70c-4&from=paste&height=219&id=u27a134e7&name=image.png&originHeight=310&originWidth=778&originalType=binary&ratio=2&rotation=0&showTitle=false&size=268151&status=done&style=shadow&taskId=uca0030f2-fc32-4199-9859-d16c2e35e23&title=&width=549)
2. Buffer 类定义了所有的缓冲区都具有的四个属性来提供关于其所包含的数据元素的信息:

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677215832999-594986cf-c7b8-4d83-bcb6-6d2eba0e0f27.png#averageHue=%23f2f1f1&clientId=u0bca6045-c70c-4&from=paste&height=210&id=u7a6d25d8&name=image.png&originHeight=305&originWidth=886&originalType=binary&ratio=2&rotation=0&showTitle=false&size=192358&status=done&style=shadow&taskId=uf9989a0a-9647-44f4-bfb8-098b68cac81&title=&width=611)

3. Buffer 类相关方法一览

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677215874092-2f4c6337-09b1-4f91-bd6f-da6f14ee0f56.png#averageHue=%23dcded1&clientId=u0bca6045-c70c-4&from=paste&height=378&id=u24ea29cb&name=image.png&originHeight=444&originWidth=712&originalType=binary&ratio=2&rotation=0&showTitle=false&size=429902&status=done&style=shadow&taskId=ue0aef77c-fba8-4467-a91f-69ab1d3eb70&title=&width=606)

4. ByteBuffer

从前面可以看出对于 Java 中的基本数据类型(boolean 除外)，都有一个 Buffer 类型与之相对应，最常用的自然是 ByteBuffer 类（二进制数据），该类的主要方法如下
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677215938336-4b3fbbcb-8954-40fa-903e-c757001c110c.png#averageHue=%23dde3d7&clientId=u0bca6045-c70c-4&from=paste&height=243&id=u2ce6531f&name=image.png&originHeight=297&originWidth=860&originalType=binary&ratio=2&rotation=0&showTitle=false&size=380688&status=done&style=shadow&taskId=u1e62221e-d5d5-47b3-b286-f9f8e8cce86&title=&width=705)
## 通道（channel）
### 基本介绍

1. NIO 的通道类似于流，但有些区别如下：
   1. 通道可以同时进行读写，而流只能读或者只能写 
   2. 通道可以实现异步读写数据 
   3. 通道可以从缓冲读数据，也可以写数据到缓冲:
2. BIO 中的 stream 是单向的，例如 FileInputStream 对象只能进行读取数据的操作，而 NIO 中的通道(Channel) 是双向的，可以读操作，也可以写操作。
3. Channel 在 NIO 中是一个接口   public interface Channel extends Closeable{}
4. 常 用 的 Channel 类 有 ： **FileChannel **、 DatagramChannel 、 **ServerSocketChannel **和 **SocketChannel **。 【ServerSocketChanne 类似 ServerSocket , SocketChannel 类似 Socket】
5. FileChannel 用于文件的数据读写，DatagramChannel 用于 UDP 的数据读写，ServerSocketChannel 和 SocketChannel 用于 TCP 的数据读写。
6. 图示

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677249289661-77298392-1c87-4c19-8997-e2165cc9137b.png#averageHue=%23e6eadf&clientId=u14fe5be7-cc28-4&from=paste&height=511&id=u81137a43&name=image.png&originHeight=452&originWidth=463&originalType=binary&ratio=2&rotation=0&showTitle=false&size=299301&status=done&style=shadow&taskId=udd11c749-dfc8-4bc5-8f90-2beb12987e7&title=&width=523.5)
### fileChannel类
FileChannel 主要用来对本地文件进行 IO 操作，常见的方法有

- public int read(ByteBuffer dst) ，从通道读取数据并放到缓冲区中 
- public int write(ByteBuffer src) ，把缓冲区的数据写到通道中 
- public long transferFrom(ReadableByteChannel src, long position, long count)，从目标通道中复制数据到当前通道 
- public long transferTo(long position, long count, WritableByteChannel target)，把数据从当前通道复制给目标通道
### 练习
#### 练习一
1) 使用前面学习后的 ByteBuffer(缓冲) 和 FileChannel(通道)， 将 "hello,尚硅谷" 写入到 file01.txt 中 
2) 文件不存在就创建 
3) 代码演示
```java
package com.linmu.nio;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.Channel;
import java.nio.channels.FileChannel;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 13:40:53
 **/
@SuppressWarnings({"all"})
public class TestChannel01 {

    public static void main(String[] args) {
        byte[] str = "hello, my name is jackson, I'm from China\n".getBytes();
        FileOutputStream fileOutputStream = null;
        try {
            fileOutputStream = new FileOutputStream("C:\\testJavaCode\\testChannel.txt",true);
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
        // 创建管道
        FileChannel channel = fileOutputStream.getChannel();
        // 创建缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate(str.length);
        // 将数据写入缓冲区
        byteBuffer.put(str);
        // 读写反转
        byteBuffer.flip();
        try {
            // 将缓冲区的数据写入通道（testChannel.txt）
            channel.write(byteBuffer);
            fileOutputStream.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```
#### 练习二
1) 使用前面学习后的 ByteBuffer(缓冲) 和 FileChannel(通道)， 将 file01.txt 中的数据读入到程序，并显示在控制台屏幕 
2) 假定文件已经存在 
3) 代码演示
```java
package com.linmu.nio;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 14:02:09
 **/
@SuppressWarnings({"all"})
public class TestChannel02 {

    public static void main(String[] args) {
        File content = new File("C:\\testJavaCode\\testChannel.txt");
        FileInputStream fileInputStream = null;
        try {
            fileInputStream = new FileInputStream(content);
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
        // 创建管道
        FileChannel channel = fileInputStream.getChannel();
        // 创建缓冲区
        ByteBuffer byteBuffer = ByteBuffer.allocate((int) content.length());
        try {
            // 读取缓冲区中的数据
            channel.read(byteBuffer);
            fileInputStream.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        System.out.println(new String(byteBuffer.array()));
    }
}

```
#### 练习三
1) 使用 FileChannel(通道) 和 方法 read , write，完成文件的拷贝 
2) 拷贝一个文本文件 1.txt , 放在项目下即可 
3) 代码演示
```java
package com.linmu.nio;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 14:23:34
 **/
@SuppressWarnings({"all"})
public class TestChannel03 {
    public static void main(String[] args) {
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;
        FileChannel fileChannel01 = null;
        FileChannel fileChannel02 = null;
        try {
            fileInputStream = new FileInputStream("C:\\testJavaCode\\testChannel.txt");
            fileChannel01 = fileInputStream.getChannel();
            fileOutputStream = new FileOutputStream("C:\\testJavaCode\\testChannel01.txt");
            fileChannel02 = fileOutputStream.getChannel();
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        }
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        while(true){
            //  position的属性初始化
            byteBuffer.clear();
            try {
                int result = fileChannel01.read(byteBuffer);
                if (result == -1){
                    break;
                }
                // 读写反转
                byteBuffer.flip();
                fileChannel02.write(byteBuffer);
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
        try {
            fileInputStream.close();
            fileOutputStream.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}

```
#### 练习四
1) 实例要求: 
2) 使用 FileChannel(通道) 和 方法 transferFrom ，完成文件的拷贝 
3) 拷贝一张图片 
4) 代码演示
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677249730243-ba8a52e3-92bd-4fce-b9a3-215645ebb2bc.png#averageHue=%23dbd0c3&clientId=u14fe5be7-cc28-4&from=paste&height=141&id=u79daac2f&name=image.png&originHeight=209&originWidth=1108&originalType=binary&ratio=2&rotation=0&showTitle=false&size=123157&status=done&style=shadow&taskId=u888c72c4-e3ab-4d7f-bc79-aebdc4253d3&title=&width=747)
```java
package com.linmu.nio;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.channels.FileChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 14:44:39
 **/
@SuppressWarnings({"all"})
public class TestChannel04 {
    public static void main(String[] args) {
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;
        try {
            fileInputStream = new FileInputStream("C:\\testJavaCode\\1.png");
            fileOutputStream = new FileOutputStream("C:\\testJavaCode\\2.png");
        }catch (IOException e){
            throw new RuntimeException(e);
        }
        FileChannel channel01 = fileInputStream.getChannel();
        FileChannel channel02 = fileOutputStream.getChannel();
        try {
            // 数据拷贝
            channel02.transferFrom(channel01,0,channel01.size());
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        try{
            fileInputStream.close();
            fileOutputStream.close();
        } catch (IOException e){
            throw new RuntimeException(e);
        }
    }
}

```
### Buffer和Channel的注意事项
1) ByteBuffer 支持类型化的 put 和 get, put 放入的是什么数据类型，get 就应该使用相应的数据类型来取出，否 则可能有 BufferUnderflowException 异常。[举例说明]
```java
package com.linmu.nio;

import java.nio.ByteBuffer;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 15:00:06
 **/
@SuppressWarnings({"all"})
public class TestBuffer01 {
    public static void main(String[] args) {
        /**
         * 读写类型要求一致
         * 读取顺序不一致时，会出现乱码、读取数据错误、异常等错误
         */
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        byteBuffer.putInt(12);
        byteBuffer.putLong(999999);
        byteBuffer.putChar('川');
        byteBuffer.putShort((short) 19);

        // 读写反转
        byteBuffer.flip();

        System.out.println(byteBuffer.getInt());
        System.out.println(byteBuffer.getLong());
        System.out.println(byteBuffer.getChar());
        System.out.println(byteBuffer.getShort());
    }
}

```
2) 可以将一个普通 Buffer 转成只读 Buffer [举例说明]
```java
package com.linmu.nio;

import java.nio.ByteBuffer;
import java.nio.IntBuffer;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 15:10:54
 **/
@SuppressWarnings({"all"})
public class TestBuffer02 {
    public static void main(String[] args) {
        IntBuffer intBuffer = IntBuffer.allocate(1024);
        for (int i = 0; i < 9; i++) {
            intBuffer.put((i + 3) * 9 + 20);
        }

        // 读写反转
        intBuffer.flip();
        IntBuffer intBuffer1 = intBuffer.asReadOnlyBuffer();


        /**
         * 只读情况下不能放入数据 ==>
         *
         * intBuffer1.put(19); ==>
         *
         * Exception in thread "main" java.nio.ReadOnlyBufferException
         * 	at java.nio.HeapIntBufferR.put(HeapIntBufferR.java:175)
         * 	at com.linmu.nio.TestBuffer02.main(TestBuffer02.java:23)
         */

        while (intBuffer1.hasRemaining()){
            intBuffer1.get();
        }
    }
}

```
3) NIO 还提供了 MappedByteBuffer， 可以让文件直接在内存（堆外的内存）中进行修改， 而如何同步到文件由 NIO 来完成. [举例说明]
```java
package com.linmu.nio;

import java.io.IOException;
import java.io.RandomAccessFile;
import java.nio.MappedByteBuffer;
import java.nio.channels.FileChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 15:27:35
 **/
@SuppressWarnings({"all"})
public class TestBuffer03 {
    public static void main(String[] args) {
        RandomAccessFile randomAccessFile = null;
        try {
            // 创建流对象
            randomAccessFile = new RandomAccessFile("C:\\testJavaCode\\testChannel.txt","rw");
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
        // 创建管道
        FileChannel channel = randomAccessFile.getChannel();
        /**
         * MappedByteBuffer实现了堆外内存级的修改，性能很好，
         * 操作系统不需要再拷贝一次
         */
        MappedByteBuffer mappedByteBuffer = null;
        try {
            /**
             * public abstract MappedByteBuffer map(MapMode mode, long position, long size)
             * mode:文件操作模式   FileChannel.MapMode.READ_WRITE表示读写模式
             * position:允许修改的其实位置  这里为其始字节
             * size:可修改的字节数
             */
            mappedByteBuffer = channel.map(FileChannel.MapMode.READ_WRITE, 0, 5);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        mappedByteBuffer.put(1,(byte) 'D');
        mappedByteBuffer.put(4,(byte) 'D');
        try {
            randomAccessFile.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}


```
4) 前面我们讲的读写操作，都是通过一个 Buffer 完成的，NIO 还支持 通过多个 Buffer (即 Buffer 数组) 完成读写操作，即 Scattering 和 Gathering 【举例说明】
```java
package com.linmu.nio;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Arrays;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/24 17:41:26
 **/
@SuppressWarnings({"all"})
public class TestBuffer04 {
    /**
     * Scattering：将数据写入到 buffer 时，可以采用 buffer 数组，依次写入 [分散]
     * Gathering: 从 buffer 读取数据时，可以采用 buffer 数组，依次读
     */
    public static void main(String[] args) throws Exception {
        // 创建ServerSocketChannel对象
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        // 创建InetSocketAddress对象
        InetSocketAddress inetSocketAddress = new InetSocketAddress(9999);
        // 管道绑定端口
        serverSocketChannel.socket().bind(inetSocketAddress);
        // 创建Buffer数组
        ByteBuffer[] byteBuffers = new ByteBuffer[2];
        byteBuffers[0] = ByteBuffer.allocate(5);
        byteBuffers[1] = ByteBuffer.allocate(3);
        // 等待客户端连接
        SocketChannel socketChannel = serverSocketChannel.accept();
        // 假设从客户端接收8个字节
        int messageLength = 8;
        // 循环的读取数据
        while (true) {
            int byteRead = 0;
            while (byteRead < messageLength) {
                long readMassageByBuffers = socketChannel.read(byteBuffers);
                byteRead += readMassageByBuffers;
                System.out.println("byteRead=" + byteRead);
                // 使用流打印 buffer、position、limit
                Arrays.asList(byteBuffers).stream().map(buffer -> "position=" +
                        buffer.position() + ",limit=" + buffer.limit()).forEach(System.out::println);
            }
            // 读写反转
            Arrays.asList(byteBuffers).forEach(buffer -> buffer.flip());
            // 将数据读出显示到客户端
            long byteWtite = 0;
            while (byteWtite < messageLength) {
                long writeMessageByBuffer = socketChannel.write(byteBuffers);
                byteWtite += writeMessageByBuffer;
            }
            // 将所有buffer进行clear
            Arrays.asList(byteBuffers).forEach(buffer -> buffer.clear());
            System.out.println(("byteRead=" + byteRead + ",byteWrite=" + byteWtite +
                    ",messageLength=" + messageLength));
        }
    }
}

```
## 筛选器（Selector）
### 基本介绍

- Java 的 NIO，用非阻塞的 IO 方式。可以用一个线程，处理多个的客户端连接，就会使用到 Selector(选择器) 
- Selector 能够检测多个注册的通道上是否有事件发生(注意:多个 Channel 以事件的方式可以注册到同一个 Selector)，如果有事件发生，便获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管 理多个通道，也就是管理多个连接和请求。【示意图】 
-  只有在 连接/通道 真正有读写事件发生时，才会进行读写，就大大地减少了系统开销，并且不必为每个连接都 创建一个线程，不用去维护多个线程 
- 避免了多线程之间的上下文切换导致的开销
### Selector 示意图和特点说明
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677307326642-0a612daf-d912-4e62-b283-e195cfe08897.png#averageHue=%23555964&clientId=ua4a37174-298e-4&from=paste&height=559&id=ub24fdadc&name=image.png&originHeight=579&originWidth=615&originalType=binary&ratio=2&rotation=0&showTitle=false&size=176736&status=done&style=shadow&taskId=u6fc079d3-0f58-489d-986f-d1c70e4e591&title=&width=593.5)

-  Netty 的 IO 线程 NioEventLoop 聚合了 Selector(选择器，也叫多路复用器)，可以同时并发处理成百上千个客户端连接。 
- 当线程从某客户端 Socket 通道进行读写数据时，若没有数据可用时，该线程可以进行其他任务。 
- 线程通常将非阻塞 IO 的空闲时间用于在其他通道上执行 IO 操作，所以单独的线程可以管理多个输入和输出通道。 
- 由于读写操作都是非阻塞的，这就可以充分提升 IO 线程的运行效率，避免由于频繁 I/O 阻塞导致的线程挂起。 
- 一个 I/O 线程可以并发处理 N 个客户端连接和读写操作，这从根本上解决了传统同步阻塞 I/O 一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升。
### Selector相关方法
Selector 类是一个抽象类, 常用方法和说明如下:
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677307537505-a7bd7b44-46d2-4a28-a1cd-c6e70198fd23.png#averageHue=%23e5e2d8&clientId=ua4a37174-298e-4&from=paste&height=248&id=ub5e38b23&name=image.png&originHeight=273&originWidth=819&originalType=binary&ratio=2&rotation=0&showTitle=false&size=258167&status=done&style=shadow&taskId=u6d61f42e-b212-4ed7-9cb2-2431bda3f59&title=&width=743.5)
### 注意事项

- NIO 中的 ServerSocketChannel 功能类似 ServerSocket，SocketChannel 功能类似 Socket
- selector 相关方法说明
   - selector.select()//阻塞 
   - selector.select(1000);//阻塞 1000 毫秒，在 1000 毫秒后返回 
   - selector.wakeup();//唤醒 selector 
   - selector.selectNow();//不阻塞，立马返还
### NIO 非阻塞 网络编程原理分析图
NIO 非阻塞 网络编程相关的(Selector、SelectionKey、ServerScoketChannel 和 SocketChannel) 关系梳理图
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677307771284-9cad3163-19af-4bb5-8c23-e100669e6760.png#averageHue=%23c19d7e&clientId=ua4a37174-298e-4&from=paste&height=414&id=ufb261c11&name=image.png&originHeight=562&originWidth=1018&originalType=binary&ratio=2&rotation=0&showTitle=false&size=383883&status=done&style=shadow&taskId=ud4c95d0b-23ed-48d4-a17d-ad1cff05669&title=&width=750)
对上图的说明: 

1. 当客户端连接时，会通过 ServerSocketChannel 得到 SocketChannel 
2. Selector 进行监听 select 方法, 返回有事件发生的通道的个数.
3. 将 socketChannel 注册到 Selector 上, register(Selector sel, int ops), 一个 selector 上可以注册多个 SocketChannel 
4. 注册后返回一个 SelectionKey, 会和该 Selector 关联(集合) 
5. 进一步得到各个 SelectionKey (有事件发生) 
6. 在通过 SelectionKey 反向获取 SocketChannel , 方法 channel() 
7. 可以通过 得到的 channel , 完成业务处理
### NIO 非阻塞 网络编程快速入门
编写一个 NIO 入门案例，实现服务器端和客户端之间的数据简单通讯（非阻塞）
```java
package com.linmu.nio;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;
import java.util.Set;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/25 14:56:28
 **/
@SuppressWarnings({"all"})
public class TestSelectorServer {
    public static void main(String[] args) throws Exception {
        // 创建 ServerSocketChannel -> ServerSocket
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        // 创建 Selector 对象
        Selector selector = Selector.open();
        // 绑定端口号，在服务端进行监听
        InetSocketAddress inetSocketAddress = new InetSocketAddress(9999);
        serverSocketChannel.socket().bind(inetSocketAddress);
        // 将 serverSocketChannel 设置为非阻塞
        serverSocketChannel.configureBlocking(false);
        // 将 serverSocketChannel 注册到 selector 关心事件为 OP_ACCEPT
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
        // 循环等待客户端连接
        while (true){
            // 等待连接 若无事件发生 则 5 秒后返回
            if (selector.select(5000) == 0){
                System.out.println("服务器等待了5秒，无连接");
                continue;
            }
            /**
             * 若返回有时间发生的通道个数>0,则获取 selectorKey 集合
             * selector.selectedKeys() 返回关注事件的集合
             *     通过 selectionKeys 反向获取通道
              */
            Set<SelectionKey> selectionKeys = selector.selectedKeys();
            // 遍历 selectionKeys
            Iterator<SelectionKey> iterator = selectionKeys.iterator();
            while(iterator.hasNext()){
                // 获取 SelectionKey
                SelectionKey next = iterator.next();
                // 根据 next 对应通道发生的事件做相应的处理
                // 吐过是 OP_ACCEPT 有新客户端连接
                if (next.isAcceptable()){
                    // 为该客户端创建一个 SocketChannel
                    SocketChannel socketChannel = serverSocketChannel.accept();
                    System.out.println("客户端连接成功:" + socketChannel.hashCode());
                    // 将 socketChannel 设置为非阻塞
                    socketChannel.configureBlocking(false);
                    // 将 socketChannel 注册到 selector 关注事件为 OP_READ 同时给 socketChannel 关联一个 Buffer
                    socketChannel.register(selector,SelectionKey.OP_READ, ByteBuffer.allocate(1024));
                }
                // 发生 OP_READ
                if (next.isReadable()){
                    // 通过 next 反向获取 Channel
                    SocketChannel channel = (SocketChannel)next.channel();
                    // 获取与 channel 关联的 Buffer
                    ByteBuffer attachment = (ByteBuffer)next.attachment();
                    channel.read(attachment);
                    System.out.println("from client:" + new String(attachment.array()));
                }
                // 手动从集合中移动当前的 selectionKey 防止重复操作
                iterator.remove();
            }
        }
    }
}

```
```java
package com.linmu.nio;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/25 14:57:03
 **/
@SuppressWarnings({"all"})
public class TestSelectorClient {
    public static void main(String[] args) throws IOException {
        // 创建 SocketChannel
        SocketChannel socketChannel = SocketChannel.open();
        // 设置为非阻塞
        socketChannel.configureBlocking(false);
        // 提供端口号和ip地址
        InetSocketAddress localhost = new InetSocketAddress("localhost", 9999);
        // 连接服务器
        if (!socketChannel.connect(localhost)){
            while (!socketChannel.finishConnect()){
                System.out.println("连接失败。。。。");
            }
        }
        // 连接成功，则发送数据
        String str = "my name is Jason Black...";
        // Wraps a byte array into a buffer.
        ByteBuffer wrap = ByteBuffer.wrap(str.getBytes());
        // 发送数据，将 buffer 数据写入 channel
        socketChannel.write(wrap);
        System.in.read();
//        System.out.println(System.in.read());
    }
}

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677323781581-22750e89-da75-4f2b-bab1-2b099f04502a.png#averageHue=%23fee6e6&clientId=u71d5ae3f-38d9-4&from=paste&height=713&id=ubfafb6cc&name=image.png&originHeight=1425&originWidth=2624&originalType=binary&ratio=2&rotation=0&showTitle=false&size=285169&status=done&style=shadow&taskId=ude8f2ff9-51ec-42a2-ae79-fc89ac5cc73&title=&width=1312)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677324115493-d9fc1869-985f-4d07-835b-6dcf4959b6b6.png#averageHue=%23fee9e9&clientId=u71d5ae3f-38d9-4&from=paste&height=453&id=ucb1a1f7f&name=image.png&originHeight=849&originWidth=1412&originalType=binary&ratio=2&rotation=0&showTitle=false&size=75199&status=done&style=shadow&taskId=ueb0be214-a52b-45e1-aaee-bbba9398118&title=&width=753)
### NIO实现群聊功能
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677393993647-00570c04-fd80-498a-be86-530e10c7933d.png#averageHue=%23ffd9d9&clientId=u2e6647a1-9437-4&from=paste&height=115&id=uc3634f6a&name=image.png&originHeight=229&originWidth=896&originalType=binary&ratio=2&rotation=0&showTitle=false&size=45244&status=done&style=shadow&taskId=u78e3794f-ce15-4c4f-bc43-537e23e044d&title=&width=448)
```java
package com.linmu.nio.test;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/26 14:43:25
 **/
@SuppressWarnings({"all"})
public class Server {

    public static void main(String[] args) {
        // 启动服务器
        Server server = new Server();
        // 
        server.listening();
    }

    // 定义属性
    private Selector selector;
    private ServerSocketChannel serverSocketChannel;
    private static final int PORT = 9999;

    // 构造器初始化属性
    public Server() {
        try {
            // 获取Selector
            selector = selector.open();
            // 获取ServerSocketChannel
            serverSocketChannel = serverSocketChannel.open();
            // 为ServerSocketChannel绑定端口号
            serverSocketChannel.socket().bind(new InetSocketAddress(PORT));
            // 设置为非注阻塞
            serverSocketChannel.configureBlocking(false);
            // 将ServerSocketChannel注册到Selector
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
        } catch (IOException IOEx) {
            IOEx.printStackTrace();
        }
    }

    
    public void listening() {
        try {
            // 循环处理
            while (true) {
                int listenResult = selector.select();
                // 有连接时处理
                if (listenResult > 0) {
                    // 遍历得到 selectionKeys 集合
                    Iterator<SelectionKey> keys = selector.selectedKeys().iterator();
                    while (keys.hasNext()) {
                        // 取出 selectionkey
                        SelectionKey key = keys.next();
                        if (key.isAcceptable()) {
                            //监听到 accept
                            SocketChannel channel = serverSocketChannel.accept();
                            // 设置为非阻塞
                            channel.configureBlocking(false);
                            // 将SocketChannel注册到Selector中
                            channel.register(selector, SelectionKey.OP_READ);
                            System.out.println(channel.getRemoteAddress() + "上线...");
                        }
                        // 发生读事件时处理
                        if (key.isReadable()) {
                            readData(key);
                        }
                        // 手动从集合中移动当前的 selectionKey, 防止重复操作
                        keys.remove();
                    }
                } else {
//                    System.out.println("waiting for connection...");
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private void readData(SelectionKey selectionKey) {
        // 声明SocketChannel
        SocketChannel socketChannel = null;
        try {
            // 获取相关的SocketChannel
            socketChannel = (SocketChannel) selectionKey.channel();
            ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
            int result = socketChannel.read(byteBuffer);
            //根据 result 的值做处理
            if (result > 0) {
                // 将缓冲区数据转为字符窜
                String message = new String(byteBuffer.array());
                System.out.println("from client: " + message);
                dataForward(message, socketChannel);
            }
        } catch (IOException e) {
            // 取消注册
            selectionKey.cancel();
            try {
                // 关闭SocketChannel
                socketChannel.close();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
            e.printStackTrace();
        }
    }

    // 将消息转发给其他用户
    private void dataForward(String message, SocketChannel socketChannel) {
        System.out.println("server forward data...");
        // 边例Selector中的SocketChannel除了数据发送的SocketChannel以外
        for (SelectionKey selectionKey : selector.keys()) {
            // 根据SelectionKey获取相关的SocketChannel
            SelectableChannel targetChannel = selectionKey.channel();
            if (targetChannel instanceof SocketChannel && targetChannel != socketChannel){
                SocketChannel directionChannel = (SocketChannel) targetChannel;
                // 创建缓冲区
                ByteBuffer byteBuffer = ByteBuffer.wrap(message.getBytes());
                try {
                    // 将缓冲区数据写入SocketChannel
                    directionChannel.write(byteBuffer);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }

}

```
```java
package com.linmu.nio.test;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;
import java.util.Scanner;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/2/26 14:43:33
 **/
@SuppressWarnings({"all"})
public class Client {

    public static void main(String[] args) {
        // 启动客户端
        Client client = new Client();
        // 启动线程读取服务器发送的数据
        new Thread(){
            @Override
            public void run() {
                client.readMessage();
            }
        }.start();
        // 发送数据给服务器
        Scanner scanner = new Scanner(System.in);
        while(scanner.hasNextLine()){
            String content = scanner.nextLine();
            client.sendMessage(content);
        }
    }

    // 定义属性
    private static final String HOST = "localhost";
    private static final int PORT = 9999;
    private Selector selector;
    private SocketChannel socketChannel;
    private String username;

    // 构造器为属性赋值
    public Client() {
        try {
            socketChannel = socketChannel.open(new InetSocketAddress(HOST, PORT));
            socketChannel.configureBlocking(false);
            selector = Selector.open();
            socketChannel.register(selector, SelectionKey.OP_READ);
            username = socketChannel.getLocalAddress().toString().substring(1);
            System.out.println(username + "is ok...");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 向服务器发送数据
    public void sendMessage(String message) {
        message = username + " speak: " + message;
        try {
            // 将缓冲数据写入管道
            socketChannel.write(ByteBuffer.wrap(message.getBytes()));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 读取服务器回复的数据
    public void readMessage() {
        try {
            int channels = selector.select();
            if (channels > 0){
                Iterator<SelectionKey> selectionKeys = selector.selectedKeys().iterator();
                while (selectionKeys.hasNext()){
                    SelectionKey selectionKey = selectionKeys.next();
                    if (selectionKey.isReadable()){
                        // 获取相关通道
                        SocketChannel socketChanel = (SocketChannel)selectionKey.channel();
                        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
                        // 将管道数据读入缓冲区
                        socketChanel.read(byteBuffer);
                        String message = new String(byteBuffer.array());
                        System.out.println(message.trim());
                    }
                }
                // 删除当前SelectionKey,防止重复操作
                selectionKeys.remove();
            } else {
//                System.out.println("without channel can be using...");
            }
        } catch (IOException e){
            e.printStackTrace();
        }
    }
}

```
## NIO 与零拷贝
### 零拷贝基本介绍

1. 零拷贝是网络编程的关键，很多性能优化都离不开。 
2.  在 Java 程序中，常用的零拷贝有 mmap(内存映射) 和 sendFile。那么，他们在 OS 里，到底是怎么样的一个 的设计？我们分析 mmap 和 sendFile 这两个零拷贝 
3. 另外我们看下 NIO 中如何使用零拷贝
### 传统 IO 数据读写
Java 传统 IO 和 网络编程的一段代码
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677476973309-d9cd6459-caa1-4aee-be50-4f2a4bd46b4c.png#averageHue=%23d7d1c8&clientId=u1af11e88-9156-4&from=paste&height=244&id=u911c1c75&name=image.png&originHeight=234&originWidth=678&originalType=binary&ratio=2&rotation=0&showTitle=false&size=137173&status=done&style=shadow&taskId=uc1100637-2c29-46c7-9823-3c67d2b9847&title=&width=706)
### 传统 IO 模型
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677476997565-36f76bbe-5e5b-4a36-a6c5-16ebb56accb6.png#averageHue=%23f8f8f8&clientId=u1af11e88-9156-4&from=paste&height=346&id=u93760946&name=image.png&originHeight=582&originWidth=1205&originalType=binary&ratio=2&rotation=0&showTitle=false&size=249108&status=done&style=shadow&taskId=u348244f6-2352-494d-b2c7-2f4ef1b1e7a&title=&width=716.5)
DMA: direct memory access 直接内存拷贝(不使用 CPU)
### mmap优化

1. mmap通过内存映射，将**文件映射到内核缓冲区**，同时，**用户空间可以共享内核空间的数据**。这样，在进行网络传输时，就可以减少内核空间到用户空间的拷贝次数。如下图
2. mmap示意图

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677477142410-7893daf6-09f4-4c8b-b5ff-3a8ded86d179.png#averageHue=%23f8f8f8&clientId=u1af11e88-9156-4&from=paste&height=347&id=u34f38df9&name=image.png&originHeight=545&originWidth=1121&originalType=binary&ratio=2&rotation=0&showTitle=false&size=202807&status=done&style=shadow&taskId=ud2c78731-25ef-40d7-99b3-82a4b373818&title=&width=714.5)
### sendFile优化

1. Linux2.1版本提供了sendFile函数，其基本原理如下：数据根本不经过用户态，直接从内核缓冲区进入到SocketBuffer，同时，由于和用户态完全无关，就减少了一次上下文切换
2. 示意图和小结

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677477225874-97668c62-b774-4b59-af0b-a6012db27fca.png#averageHue=%23f8f8f8&clientId=u1af11e88-9156-4&from=paste&height=432&id=u18e98b11&name=image.png&originHeight=582&originWidth=965&originalType=binary&ratio=2&rotation=0&showTitle=false&size=205933&status=done&style=shadow&taskId=u4fc7e5b3-66e2-4831-87ee-619389a525b&title=&width=716.5)

3. 提示：零拷贝从操作系统角度，是没有cpu拷贝
4. Linux在2.4版本中，做了一些修改，避免了从**内核缓冲区**拷贝到**Socketbuffer**的操作，直接拷贝到协议栈，从而再一次减少了数据拷贝。具体如下图和小结：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677477341076-db66bd5e-01d7-4f39-8ff3-d7a864c4a0e5.png#averageHue=%23f7f7f7&clientId=u1af11e88-9156-4&from=paste&height=293&id=u27d0d8cd&name=image.png&originHeight=586&originWidth=991&originalType=binary&ratio=2&rotation=0&showTitle=false&size=191330&status=done&style=shadow&taskId=ub3494896-54b8-403d-805b-73ebdbd18d3&title=&width=495.5)

5. 这里其实有一次cpu拷贝  kernelbuffer->socketbuffer  但是，拷贝的信息很少，比如lenght,offset,消耗低，可以忽略
### 零拷贝的再次理解

1. 我们说零拷贝，是从**操作系统的角度**来说的。因为内核缓冲区之间，没有数据是重复的（只有kernelbuffer有一份数据
2. 零拷贝不仅仅带来更少的数据复制，还能带来其他的性能优势，例如更少的上下文切换，更少的CPU缓存、共享以及无CPU校验和计算。
### mmap和sendFile的区别
1）mmap适合小数据量读写，sendFile适合大文件传输。 
2）mmap需要3次上下文切换，3次数据拷贝；sendFile需要2次上下文切换，最少2次数据拷贝。 
3）sendFile可以利用DMA方式，减少CPU拷贝，mmap则不能（必须从内核拷贝到Socket缓冲区）。
### NIO零拷贝案例
案例要求： 
1)使用传统的IO方法传递一个大文件 
2)使用NIO零拷贝方式传递(transferTo)一个大文件
3)看看两种传递方式耗时时间分别是多少
```java
packagecom.atguigu.nio.zerocopy;

importjava.net.InetSocketAddress;
importjava.net.ServerSocket;
importjava.nio.ByteBuffer;
importjava.nio.channels.ServerSocketChannel;importjava.nio.channels.SocketChannel;

publicclassNewIOServer{
    publicstaticvoidmain(String[]args)throwsException{
        InetSocketAddressaddress=newInetSocketAddress(7001);
        ServerSocketChannelserverSocketChannel=ServerSocketChannel.open();ServerSocketserverSocket=serverSocketChannel.socket();
        serverSocket.bind(address);
        //创建buffer
        ByteBufferbyteBuffer=ByteBuffer.allocate(4096);
        while(true){
            SocketChannelsocketChannel=serverSocketChannel.accept();intreadcount=0;
            while(-1!=readcount){
                try{
                    readcount=socketChannel.read(byteBuffer);
                }catch(Exceptionex){
                    //ex.printStackTrace();
                    break;
                }
                //
                byteBuffer.rewind();//倒带position=0mark作废
            }
        }
    }
}





packagecom.atguigu.nio.zerocopy;

importjava.io.FileInputStream;
importjava.net.InetSocketAddress;
importjava.nio.channels.FileChannel;
importjava.nio.channels.SocketChannel;

publicclassNewIOClient{
    publicstaticvoidmain(String[]args)throwsException{
        SocketChannelsocketChannel=SocketChannel.open();
        socketChannel.connect(newInetSocketAddress("localhost",7001));Stringfilename="protoc-3.6.1-win32.zip";
        //得到一个文件channel
        FileChannelfileChannel=newFileInputStream(filename).getChannel();
        //准备发送
        longstartTime=System.currentTimeMillis();
        //在linux下一个transferTo方法就可以完成传输
        //在windows下一次调用transferTo只能发送8m,就需要分段传输文件,而且要主要//传输时的位置=》课后思考...
        //transferTo底层使用到零拷贝
        longtransferCount=fileChannel.transferTo(0,fileChannel.size(),socketChannel);
        System.out.println("发送的总的字节数="+transferCount+"耗时:"+(System.currentTimeMillis()-startTime));
        //关闭
        fileChannel.close();
    }
}


```
### JavaAIO基本介绍

1. JDK7引入了AsynchronousI/O，即AIO。在进行I/O编程中，常用到两种模式：Reactor和Proactor。Java的NIO就是Reactor，当有事件触发时，服务器端得到通知，进行相应的处理
2. AIO即NIO2.0，叫做异步不阻塞的IO。AIO引入异步通道的概念，采用了Proactor模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用
3. 目前AIO还没有广泛应用，Netty也是基于NIO,而不是AIO，因此我们就不详解AIO了，有兴趣的同学可以参考<<Java新一代网络编程模型AIO原理及Linux系统AIO介绍>>  [http://www.52im.net/thread-306-1-1.html](http://www.52im.net/thread-306-1-1.html)
### BIO、NIO、AIO对比表
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677477696839-ec3ae5c8-abda-4291-b9ed-16fd71732335.png#averageHue=%23e3ece7&clientId=u1af11e88-9156-4&from=paste&height=344&id=uff6b1a0d&name=image.png&originHeight=378&originWidth=813&originalType=binary&ratio=2&rotation=0&showTitle=false&size=298144&status=done&style=shadow&taskId=uf91c0fa6-6ab3-4e70-aa8b-7091433cc44&title=&width=740.5)
# netty概述
## 原生 NIO 存在的问题 

- NIO 的类库和 API 繁杂，使用麻烦：需要熟练掌握 Selector、ServerSocketChannel、SocketChannel、ByteBuffer等。 
- 需要具备其他的额外技能：要熟悉 Java 多线程编程，因为 NIO 编程涉及到 Reactor 模式，你必须对多线程和网络编程非常熟悉，才能编写出高质量的 NIO 程序。 
- 开发工作量和难度都非常大：例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常流的处理等等。 
- JDK NIO 的 Bug：例如臭名昭著的 Epoll Bug，它会导致 Selector 空轮询，最终导致 CPU 100%。直到 JDK 1.7版本该问题仍旧存在，没有被根本解决。
## netty官网
官网：[https://netty.io/downloads.html](https://netty.io/downloads.html)
netty组件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677584604609-32fae728-7938-4498-a3d1-1ca54199adaf.png#averageHue=%23ebe7bd&clientId=ua263bc5e-f612-4&from=paste&height=419&id=u613bda07&name=image.png&originHeight=537&originWidth=943&originalType=binary&ratio=2&rotation=0&showTitle=false&size=341467&status=done&style=shadow&taskId=ua03cbef5-4524-4ab8-9b68-93939ef2452&title=&width=735.5)
## netty优点
Netty 对 JDK 自带的 NIO 的 API 进行了封装，解决了上述问题。 

1. 设计优雅：适用于各种传输类型的统一 API 阻塞和非阻塞 Socket；基于灵活且可扩展的事件模型，可以清晰地分离关注点；高度可定制的线程模型 - 单线程，一个或多个线程池. 
2. 使用方便：详细记录的 Javadoc，用户指南和示例；没有其他依赖项，JDK 5（Netty 3.x）或 6（Netty 4.x）就足够了。 
3. 高性能、吞吐量更高：延迟更低；减少资源消耗；最小化不必要的内存复制。 
4. 安全：完整的 SSL/TLS 和 StartTLS 支持。 
5. 社区活跃、不断更新：社区活跃，版本迭代周期短，发现的 Bug 可以被及时修复，同时，更多的新功能会被加入
## netty版本说明
1) netty 版本分为 netty3.x 和 netty4.x、netty5.x 
2) 因为 Netty5 出现重大 bug，已经被官网废弃了，目前推荐使用的是 Netty4.x 的稳定版本 
3) 目前在官网可下载的版本 netty3.x netty4.0.x 和 netty4.1.x 
4) 在本套课程中，我们讲解 Netty4.1.x 版本 
5) netty 下载地址： https://bintray.com/netty/downloads/netty/
# netty高性能框架
## 线程模型基本介绍

1. 不同的线程模式，对程序的性能有很大影响，为了搞清 Netty 线程模式，我们来系统的讲解下 各个线程模式，最后看看 Netty 线程模型有什么优越性. 
   1. 目前存在的线程模型有： 传统阻塞 I/O 服务模型，Reactor 模式 
   2. 根据 Reactor 的数量和处理资源池线程的数量不同，有 3 种典型的实现 
      1. 单 Reactor 单线程； 
      2. 单 Reactor 多线程； 
      3. 主从 Reactor 多线程 
2. Netty 线程模式(Netty 主要基于主从 Reactor 多线程模型做了一定的改进，其中主从 Reactor 多线程模型有多个 Reactor)
## 传统阻塞 I/O 服务模型
### 工作原理图
1) 黄色的框表示对象， 蓝色的框表示线程 
2) 白色的框表示方法(API)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677584994882-53fdc1dc-be53-4245-9e57-05895887588d.png#averageHue=%23585c68&clientId=ua263bc5e-f612-4&from=paste&height=660&id=u1c1ce737&name=image.png&originHeight=651&originWidth=734&originalType=binary&ratio=2&rotation=0&showTitle=false&size=401320&status=done&style=shadow&taskId=u2794b936-c274-4009-b6c3-7945676cea3&title=&width=744)
### 模型特点 
1) 采用阻塞 IO 模式获取输入的数据 
2) 每个连接都需要独立的线程完成数据的输入，业务处理, 数据返回
### 问题分析
1) 当并发数很大，就会创建大量的线程，占用很大系统资源 
2) 连接创建后，如果当前线程暂时没有数据可读，该线程会阻塞在 read 操作，造成线程资源浪费
## Reactor模式
### 针对传统阻塞 I/O 服务模型的 2 个缺点，解决方案：

1. 基于 I/O 复用模型：多个连接共用一个阻塞对象，应用程序只需要在一个阻塞对象等待，无需阻塞等待所有连接。当某个连接有新的数据可以处理时，操作系统通知应用程序，线程从阻塞状态返回，开始进行业务处理
   1. Reactor 对应的叫法: 1. 反应器模式 2. 分发者模式(Dispatcher) 3. 通知者模式(notifier)
2. 基于线程池复用线程资源：不必再为每个连接创建线程，将连接完成后的业务处理任务分配给线程进行处理，一个线程可以处理多个连接的业务。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677585252278-6740e0c1-e46c-4867-8355-897729b91ac1.png#averageHue=%23ccd9d7&clientId=ua263bc5e-f612-4&from=paste&height=265&id=u1d43239a&name=image.png&originHeight=206&originWidth=561&originalType=binary&ratio=2&rotation=0&showTitle=false&size=158611&status=done&style=shadow&taskId=uc781459f-1b4b-4a1c-b457-ac11e952e84&title=&width=721.5)
### I/O 复用结合线程池，就是 Reactor 模式基本设计思想，如图
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677585289959-276ac9b6-0b84-494a-acb3-651a49bd1102.png#averageHue=%23555a66&clientId=ua263bc5e-f612-4&from=paste&height=461&id=u11ce4b0a&name=image.png&originHeight=655&originWidth=1034&originalType=binary&ratio=2&rotation=0&showTitle=false&size=596828&status=done&style=shadow&taskId=ufaea4f64-5f89-4b6b-bc4f-59e4482ddba&title=&width=727)

1. Reactor 模式，通过一个或多个输入同时传递给服务处理器的模式(基于事件驱动) 
2. 服务器端程序处理传入的多个请求,并将它们同步分派到相应的处理线程， 因此 Reactor 模式也叫 Dispatcher模式 
3. Reactor 模式使用 IO 复用监听事件, 收到事件后，分发给某个线程(进程), 这点就是网络服务器高并发处理关键
### Reactor 模式中 核心组成

1. Reactor：Reactor 在一个单独的线程中运行，负责监听和分发事件，分发给适当的处理程序来对 IO 事件做出反应。 它就像公司的电话接线员，它接听来自客户的电话并将线路转移到适当的联系人； 
2. Handlers：处理程序执行 I/O 事件要完成的实际事件，类似于客户想要与之交谈的公司中的实际官员。Reactor通过调度适当的处理程序来响应 I/O 事件，处理程序执行非阻塞操作。
### Reactor 模式分类
根据 Reactor 的数量和处理资源池线程的数量不同，有 3 种典型的实现 
1) 单 Reactor 单线程 
2) 单 Reactor 多线程 
3) 主从 Reactor 多线程
## 单 Reactor 单线程
原理图，并使用 NIO 群聊系统验证
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677585487402-b0002eb0-74d7-48ec-8239-8897a6259d14.png#averageHue=%23565a66&clientId=ua263bc5e-f612-4&from=paste&height=475&id=u7d6c4c47&name=image.png&originHeight=637&originWidth=972&originalType=binary&ratio=2&rotation=0&showTitle=false&size=489868&status=done&style=shadow&taskId=u7a7fc459-e1c9-4fe5-8208-3ce95755651&title=&width=725)

1. Select 是前面 I/O 复用模型介绍的标准网络编程 API，可以实现应用程序通过一个阻塞对象监听多路连接请求 
2. Reactor 对象通过 Select 监控客户端请求事件，收到事件后通过 Dispatch 进行分发 
3. 如果是建立连接请求事件，则由 Acceptor 通过 Accept 处理连接请求，然后创建一个 Handler 对象处理连接完成后的后续业务处理 
4. 如果不是建立连接事件，则 Reactor 会分发调用连接对应的 Handler 来响应 
5. Handler 会完成 Read→业务处理→Send 的完整业务流程

结合实例：服务器端用一个线程通过多路复用搞定所有的 IO 操作（包括连接，读、写等），编码简单，清晰明了， 但是如果客户端连接数量较多，将无法支撑，前面的 NIO 案例就属于这种模型

### 方案优缺点分析

1. 优点：模型简单，没有多线程、进程通信、竞争的问题，全部都在一个线程中完成 
2. 缺点：性能问题，只有一个线程，无法完全发挥多核 CPU 的性能。Handler 在处理某个连接上的业务时，整个进程无法处理其他连接事件，很容易导致性能瓶颈 
3. 缺点：可靠性问题，线程意外终止，或者进入死循环，会导致整个系统通信模块不可用，不能接收和处理外部消息，造成节点故障 
4. 使用场景：客户端的数量有限，业务处理非常快速，比如 Redis 在业务处理的时间复杂度 O(1) 的情况

## 单 Reactor 多线程
### 原理图
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677585749496-40e4416e-8f53-4a58-8343-b91b3e67008f.png#averageHue=%23565a67&clientId=ua263bc5e-f612-4&from=paste&height=715&id=u468dcc3b&name=image.png&originHeight=922&originWidth=951&originalType=binary&ratio=2&rotation=0&showTitle=false&size=865651&status=done&style=shadow&taskId=u63935ee9-c97b-4fe3-b835-c2e6e1df63c&title=&width=737.5)

1. Reactor 对象通过 select 监控客户端请求事件, 收到事件后，通过 dispatch 进行分发 
2. 如果建立连接请求, 则右 Acceptor 通过accept 处理连接请求, 然后创建一个 Handler 对象处理完成连接后的各种事件 
3. 如果不是连接请求，则由 reactor 分发调用连接对应的 handler 来处理
4. handler 只负责响应事件，不做具体的业务处理, 通过 read 读取数据后，会分发给后面的 worker 线程池的某个线程处理业务 
5. worker 线程池会分配独立线程完成真正的业务，并将结果返回给 handler 
6. handler 收到响应后，通过 send 将结果返回给 client
### 方案优缺点分析 

1. 优点：可以充分的利用多核 cpu 的处理能力 
2. 缺点：多线程数据共享和访问比较复杂， reactor 处理所有的事件的监听和响应，在单线程运行， 在高并发场景容易出现性能瓶颈.
## 主从 Reactor 多线程
### 工作原理图
针对单 Reactor 多线程模型中，Reactor 在单线程中运行，高并发场景下容易成为性能瓶颈，可以让 Reactor 在多线程中运行
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677585961998-baa0a317-2e69-4823-bba5-bef3a8213570.png#averageHue=%23565a66&clientId=ua263bc5e-f612-4&from=paste&height=743&id=ue532bd67&name=image.png&originHeight=904&originWidth=864&originalType=binary&ratio=2&rotation=0&showTitle=false&size=759054&status=done&style=shadow&taskId=u1762b46f-d551-4104-8fe7-da63086a1ed&title=&width=710)
1)Reactor主线程MainReactor对象通过select监听连接事件,收到事件后，通过Acceptor处理连接事件
2)当Acceptor处理连接事件后，MainReactor将连接分配给SubReactor 
3)subreactor将连接加入到连接队列进行监听,并创建handler进行各种事件处理 
4)当有新事件发生时，subreactor就会调用对应的handler处理 
5)handler通过read读取数据，分发给后面的worker线程处理 
6)worker线程池分配独立的worker线程进行业务处理，并返回结果
7)handler收到响应的结果后，再通过send将结果返回给client 
8)Reactor主线程可以对应多个Reactor子线程,即MainRecator可以关联多个SubReactor
### ScalableIOinJava对MultipleReactors的原理图解
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677586102253-65b44703-5f62-456a-9fea-b89c2c37703b.png#averageHue=%23f4f2eb&clientId=ua263bc5e-f612-4&from=paste&height=268&id=u9e3248c4&name=image.png&originHeight=339&originWidth=936&originalType=binary&ratio=2&rotation=0&showTitle=false&size=299189&status=done&style=shadow&taskId=u3085ead4-ed2a-4e64-8005-9e0c92433c3&title=&width=739)
方案优缺点说明：

1. 优点：父线程与子线程的数据交互简单职责明确，父线程只需要接收新连接，子线程完成后续的业务处理。 
2. 优点：父线程与子线程的数据交互简单，Reactor主线程只需要把新连接传给子线程，子线程无需返回数据。 
3. 缺点：编程复杂度较高 
4. 结合实例：这种模型在许多项目中广泛使用，包括Nginx主从Reactor多进程模型，Memcached主从多线程，Netty主从多线程模型的支持
## Reactor模式小结
### 种模式用生活案例来理解
1)单Reactor单线程，前台接待员和服务员是同一个人，全程为顾客服 
2)单Reactor多线程，1个前台接待员，多个服务员，接待员只负责接待
3)主从Reactor多线程，多个前台接待员，多个服务生
### Reactor模式具有如下的优点
1)响应快，不必为单个同步时间所阻塞，虽然Reactor本身依然是同步的 
2)可以最大程度的避免复杂的多线程及同步问题，并且避免了多线程/进程的切换开销
3)扩展性好，可以方便的通过增加Reactor实例个数来充分利用CPU资源 
4)复用性好，Reactor模型本身与具体事件处理逻辑无关，具有很高的复用性
## Netty模型
### 工作原理示意图1-简单版
Netty主要基于主从Reactors多线程模型（如图）做了一定的改进，其中主从Reactor多线程模型有多个Reactor
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677586352841-a64ffc1e-1674-4713-8a27-676280fefe26.png#averageHue=%23f5f7ef&clientId=ua263bc5e-f612-4&from=paste&height=247&id=u0117ada4&name=image.png&originHeight=283&originWidth=837&originalType=binary&ratio=2&rotation=0&showTitle=false&size=181464&status=done&style=shadow&taskId=uf0ecaca0-5484-4105-9ea0-e9472c69aa4&title=&width=730.5)

1. BossGroup线程维护Selector,只关注Accecpt
2. 当接收到Accept事件，获取到对应的SocketChannel,.封装成NIOScoketChannel并注册到Worker线程（事件循环)，并进行维护
3. 当Worker线程监听到selector中通道发生自己感兴趣的事件后，就进行处理（就由handler),注意handler己经加入到通道
### 工作原理示意图2-进阶版
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677586514046-79517573-08ee-4153-9825-e64a6ee4d8ee.png#averageHue=%23dee6cf&clientId=ua263bc5e-f612-4&from=paste&height=416&id=u1f840928&name=image.png&originHeight=589&originWidth=1049&originalType=binary&ratio=2&rotation=0&showTitle=false&size=672741&status=done&style=shadow&taskId=u1ea7c2ab-a150-45a0-857b-971ccc85053&title=&width=740.5)
### 工作原理示意图-详细版
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677586543770-a2f0a644-26c9-48d7-84a7-4174f68904a7.png#averageHue=%23555965&clientId=ua263bc5e-f612-4&from=paste&height=627&id=ua3a2d02b&name=image.png&originHeight=862&originWidth=1030&originalType=binary&ratio=2&rotation=0&showTitle=false&size=676871&status=done&style=shadow&taskId=u02e67073-7820-4ec3-af34-913464a58d6&title=&width=749)

1. Netty抽象出两组线程池BossGroup专门负责接收客户端的连接，WorkerGroup专门负责网络的读写BossGroup和WorkerGroup类型都是NioEventLoopGroup
2. NioEventLoopGroup相当于一个事件循环组，这个组中含有多个事件循环，每一个事件循环是NioEventLoop
3. NioEventLoop表示一个不断循环的执行处理任务的线程，每个NioEventLoop都有一个selector,用于监听绑定在其上的socket的网络通讯
4. NioEventLoopGroup可以有多个线程，即可以含有多个NioEventLoop
5. 每个Boss NioEventLoop循环执行的步骤有3步
   1. 轮询accept事件
   2. 处理accept事件，与client建立连接，生成NioScocketChannel,并将其注册到某个worker NIOEventLoop上的selector
   3. 处理任务队列的任务，即runAllTasks
6. 每个WorkerNIOEventLoop循环执行的步骤
   1. 轮询read,write事件
   2. 处理i/o事件，即read,write事件，在对应NioScocketChannel处理
   3. 处理任务队列的任务，即runAllTasks
7. 每个WorkerNIOEventLoop处理业务时，会使用pipeline(管道),pipeline中包含了channel,即通过pipeline可以获取到对应通道,管道中维护了很多的处理器
### 案例
实例要求：使用 IDEA 创建 Netty 项目 
1) Netty 服务器在 6668 端口监听，客户端能发送消息给服务器 "hello, 服务器~" 
2) 服务器可以回复消息给客户端 "hello, 客户端~" 
3) 目的：对 Netty 线程模型 有一个初步认识, 便于理解 Netty 模型理论 
4) 看老师代码演示 
5.1 编写服务端 
5.2 编写客户端 
5.3 **对 netty 程序进行分析，看看 netty 模型特**点 
说明: 创建 Maven 项目，并引入 Netty 包 
5) 代码如下(TCP)
```java
package com.linmu.netty.example;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 13:40:34
 **/
@SuppressWarnings({"all"})
public class Server {

    public static void main(String[] args) {
        /**
         * 创建 bossGroup 和 workerGroup
         * 1.创建 bossGroup 线程组和 workerGroup 两个线程组
         * 2.bossGroup 只处理连接请求， wokerGroup 处理业务请求
         * 3.两个都是无限循环
         * 4.bossGroup 和 workerGroup 含有的子线程数（NioEventLoop）的个数
         *      默认 cpu核数 * 2
         */
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try{
            // 创建服务器的启动对象，配置参数
            ServerBootstrap serverBootstrap = new ServerBootstrap();

            // 使用链式编程设置参数
            // 设置两个线程组
            serverBootstrap.group(bossGroup, workerGroup)
                    // 使用 NioServerSocketChannel 作为服务器的通道实现
                    .channel(NioServerSocketChannel.class)
                    // 设置线程队列得到的连接个数
                    .option(ChannelOption.SO_BACKLOG,128)
                    // 设置线程保持连接状态
                    .childOption(ChannelOption.SO_KEEPALIVE,true)
                    // 给 workerGroup 的 EventLoop 对应的管道设置处理器
                    // headler() 用于设置bossGroup的处理器
                    // childHandler() 用于设置workerGroup设置处理器  
                    .handler(null) 
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        // 创建一个通道初始化对象（匿名对象）
                        // 给 pipeline 设置处理器
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            socketChannel.pipeline().addLast(new ServerHandler());
                        }
                    });
            System.out.println("server is ready...");

            // 绑定端口号并且同步，生成一个 ChannelFuture 对象
            // 启动服务器（并绑定端口号）
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch (InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

}
```
```java
package com.linmu.netty.example;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.Channel;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:22:29
 **/
@SuppressWarnings({"all"})
/**
 * 1.自定义 Handler 需要继承 netty 规定好的 HandlerAdapter（规范）
 * 2.继承后，自定义的 Handler 才是被 netty 承认的 Handler
 */
public class ServerHandler extends ChannelInboundHandlerAdapter {

    /**
     * 读取数据时间（用于读取客户端发送来的数据）
     * 1.ChannelHandlerContext ctx：表示上下文对象，包含 管道pipeline，通道channel，地址
     * 2.Object msg：客户端发送的数据
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        System.out.println("server read thread --> " + Thread.currentThread().getName());
        System.out.println("server ChannelHandlerContext --> " + ctx);
        Channel channel = ctx.channel();
        // 将 msg 转成一个 ByteBuf，该 ByteBuf 是 netty 提供的，而不是 NIO 的 ByteBuffer
        ByteBuf byteBuf = (ByteBuf) msg;
        System.out.println("message from client --> " + byteBuf.toString(CharsetUtil.UTF_8));
        System.out.println("address of client --> " + ctx.channel().remoteAddress());
    }

    /**
     * 数据读取完毕
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        /**
         * writeAndFlush 是 write 和 flush
         * 将数据写入到缓存，并刷新
         * 一般来讲，需要对发送的数据进行编码
         */
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello client, I am server...", CharsetUtil.UTF_8));
    }

    /**
     * 异常处理，一般需要关闭通道
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.close();
    }
}

```
```java
package com.linmu.netty.example;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:45:02
 **/
@SuppressWarnings({"all"})
public class Client {

    public static void main(String[] args) {

        // 客户端仅需要一个事件循环组
        EventLoopGroup ev = new NioEventLoopGroup();

        try{
            /**
             * 创建客户端启动对象
             * 注意客户端使用的不是 ServerBootstrap 而是 Bootstrap
             */
            Bootstrap bootstrap = new Bootstrap();

            // 设置相关参数
            // 设置线程组
            bootstrap.group(ev)
                    // 设置客户端通道的实现类（反射）
                    .channel(NioSocketChannel.class)
                    // 添加处理器
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            socketChannel.pipeline().addLast(new ClientHandler());
                        }
                    });
            System.out.println("client is ready...");

            /**
             * 启动客户端连接服务器
             * 关于 channelFuture 要分析，涉及到 netty 的异步模型
             */
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch(InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            ev.shutdownGracefully();
        }
    }
}

```
```java
package com.linmu.netty.example;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 15:02:38
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends ChannelInboundHandlerAdapter {

    // 当通道就绪就会触发该方法
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        System.out.println("client --> " + ctx);
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello server this is client...", CharsetUtil.UTF_8));
    }

    // 当通道有读取事件时触发

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf byteBuf = (ByteBuf) msg;
        System.out.println("message from server --> " + byteBuf.toString(CharsetUtil.UTF_8));
        System.out.println("address of server --> " + ctx.channel().remoteAddress());
    }

    // 异常处理

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}

```
### 任务队列中的 Task 有 3 种典型使用场景 

1. 用户程序自定义的普通任务「举例说明1
2. 用户自定义定时任务
3. 非当前Reactor线程调用Channel的各种方法
   1. 例如在推送系统的业务线程里面，根据用户的标识，找到对应的 Channel 引用，然后调用 Write 类方法向该用户推送消息，就会进入到这种场景。最终的 Write 会提交到任务队列中后被异步消费
> 代码演示

```java
//解决方案 1 用户程序自定义的普通任务
ctx.channel().eventLoop().execute(new Runnable() {
    @Override
    public void run() {
        try {
            Thread.sleep(5 * 1000);
            ctx.writeAndFlush(Unpooled.copiedBuffer("hello, 客户端~(>^ω^<)喵 2", CharsetUtil.UTF_8));
            System.out.println("channel code=" + ctx.channel().hashCode());
        } catch (Exception ex) {
            System.out.println("发生异常" + ex.getMessage());
        }
    }
});
ctx.channel().eventLoop().execute(new Runnable() {
    @Override
    public void run() {
        try {
            Thread.sleep(5 * 1000);
            ctx.writeAndFlush(Unpooled.copiedBuffer("hello, 客户端~(>^ω^<)喵 3", CharsetUtil.UTF_8));
            System.out.println("channel code=" + ctx.channel().hashCode());
        } catch (Exception ex) {
            System.out.println("发生异常" + ex.getMessage());
        }
    }
});
```
```java
ctx.channel().eventLoop().schedule(new Runnable() {
    @Override
    public void run() {
        try {
            Thread.sleep(5 * 1000);
            ctx.writeAndFlush(Unpooled.copiedBuffer("hello, 客户端~(>^ω^<)喵 4", CharsetUtil.UTF_8));
            System.out.println("channel code=" + ctx.channel().hashCode());
        } catch (Exception ex) {
            System.out.println("发生异常" + ex.getMessage());
        }
    }
}, 5, TimeUnit.SECONDS);
```
### 案例解析

1. Nety抽象出两组线程池，BossGroup专门负责接收客户端连接，WorkerGroup专门负责网络读写操作。
2. NioEventLoop表示一个不断循环执行处理任务的线程，每个NioEventLoop都有一个selector,用于监听绑定在其上的socket网络通道。
3. NioEventLoop内部采用串行化设计，从消息的读取->解码->处理->编码->发送，始终由IO线程NioEventLoop负责
- NioEventLoopGroup下包含多个NioEventLoop
- 每个NioEventLoop中包含有一个Selector,一个taskQueue
- 每个NioEventLoop的Selector上可以注册监听多个NioChannel
- 每个NioChannel只会绑定在唯一的NioEventLoop上
- 每个NioChannel都绑定有一个自己的ChannelPipeline
## 异步模型
### 基本介绍

1. 异步的概念和同步相对。当一个异步过程调用发出后，调用者不能立刻得到结果。实际处理这个调用的组件在完成后，通过状态、通知和回调来通知调用者。
2. Netty中的I/O操作是异步的，包括Bind、Write、.Connect等操作会简单的返▣一个ChannelFuture。
3. 调用者并不能立刻获得结果，而是通过Future-Listener机制，用户可以方便的主动获取或者通过通知机制获得IO操作结果
4. Nety的异步模型是建立在future和callback的之上的。callback就是回调。重点说Future,它的核心思想是：假设一个方法fu,计算过程可能非常耗时，等待fn返回显然不合适。那么可以在调用fun的时候，立马返回一个Future,后续可以通过Future去监控方法fun的处理过程（即：Future-Listener机制）
### future说明

1. 表示异步的执行结果，可以通过它提供的方法来检测执行是否完成，比如检索计算等等
2. ChannelFuture是一个接口：public interface ChannelFuture extends Future∝Void>我们可以添加监听器，当监听的事件发生时，就会通知到监听器
### 工作原理示意图
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677826256160-dbbe0c48-a537-4501-8129-6e37100e93cd.png#averageHue=%23f0f0f0&clientId=u57a9b110-3b1c-4&from=paste&height=165&id=u5a643415&name=image.png&originHeight=329&originWidth=995&originalType=binary&ratio=2&rotation=0&showTitle=false&size=220651&status=done&style=shadow&taskId=u7cf96928-6f71-40a3-8f2f-c77c3c1d492&title=&width=497.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677826260585-5e40b72e-fd06-4ee7-9c1a-adde46f5d225.png#averageHue=%23f8f9f4&clientId=u57a9b110-3b1c-4&from=paste&height=149&id=u54a32180&name=image.png&originHeight=298&originWidth=1004&originalType=binary&ratio=2&rotation=0&showTitle=false&size=186943&status=done&style=shadow&taskId=uabe492a7-bef5-4f1c-99f3-3d1d19a7157&title=&width=502)

1. 在使用Netty进行编程时，拦截操作和转换出入站数据只需要您提供callback或利用future即可。这使得链式操作简单、高效，并有利于编写可重用的、通用的代码。
2. Nty框架的目标就是让你的业务逻辑从网络基础应用编码中分离出来、解脱出来
### Future-Listener 机制

1. 当 Future 对象刚刚创建时，处于非完成状态，调用者可以通过返回的 ChannelFuture 来获取操作执行的状态， 注册监听函数来执行完成后的操作。
2. 常见有如下操作		
   1. 通过isDone方法来判断当前操作是否完成：
   2. 通过isSuccess方法来判断已完成的当前操作是否成功：
   3. 通过getCause方法来获取已完成的当前操作失败的原因；
   4. 通过isCancelled方法来判断已完成的当前操作是否被取消；
   5. 通过addListener方法来注册监听器，当操作已完成(isDone方法返回完成)，将会通知指定的监听器；如果Future对象己完成，则通知指定的监听器
> 演示：绑定端口是异步操作，当绑定操作处理完，将会调用相应的监听器处理逻辑

```java
//绑定一个端口并且同步, 生成了一个 ChannelFuture 对象
//启动服务器(并绑定端口)
ChannelFuture cf = bootstrap.bind(6668).sync();
//给 cf 注册监听器，监控我们关心的事件
cf.addListener(new ChannelFutureListener() {
    @Override
    public void operationComplete(ChannelFuture future) throws Exception {
        if (cf.isSuccess()) {
            System.out.println("监听端口 6668 成功");
        } else {
            System.out.println("监听端口 6668 失败");
        }
    }
});
```
### 快速入门实例-HTTP 服务
1)实例要求：使用DEA创建Ney项目
2)Nety服务器在6668端口监听，浏览器发出请求"http:/1ocalhost::6668/"
3)服务器可以回复消息给客户端"Hlo!我是服务器5"，并对特定请求资源进行过滤
4)目的：Netty可以做Http服务开发，并且理解Handler实例和客户端及其请求的关系
```java

package com.linmu.netty.http;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/3 13:28:04
 **/
@SuppressWarnings({"all"})
public class Server {
    public static void main(String[] args) throws InterruptedException {
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try{
            ServerBootstrap serverBootstrap = new ServerBootstrap();
            serverBootstrap.group(bossGroup,workerGroup).channel(NioServerSocketChannel.class)
                    .childHandler(new ServerInit());
            ChannelFuture sync = serverBootstrap.bind(9999).sync();
            sync.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }
}

```
```java

package com.linmu.netty.http;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.HttpServerCodec;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/3 13:28:24
 **/
@SuppressWarnings({"all"})
public class ServerInit extends ChannelInitializer<SocketChannel> {

    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        // 在处理器中加入管道

        // 得到管道
        ChannelPipeline pipeline = socketChannel.pipeline();

        /**
         * 加入 netty 加入的 httpServerCodec
         * httpServerCodec 是netty提供的处理http的 编-解码器
          */
        pipeline.addLast("httpServerCodec",new HttpServerCodec());

        // 添加自定义 handler
        pipeline.addLast("headleOfMine",new ServerHandler());

    }
}

```
```java
package com.linmu.netty.http;

import com.sun.jndi.toolkit.url.Uri;
import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.handler.codec.http.*;
import io.netty.util.CharsetUtil;

import java.net.URI;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/3 13:28:14
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends SimpleChannelInboundHandler<HttpObject> {
    /**
     * SimpleChannelInboundHandler 是 ChannelInboundHandlerAdapter 的子类
     * HttpObject 客户端和服务器端相互通信的数据被封装成 HttpObject
     */

    /**
     * 用于读取客户端数据
     * @param channelHandlerContext
     * @param httpObject
     * @throws Exception
     */
    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, HttpObject httpObject) throws Exception {
        // 判读 message 是不是 httprequst 请求
        if(httpObject instanceof HttpRequest){
            // 将 httpObject 强转为 HttpRequest 类型
            HttpRequest httpRequest = (HttpRequest) httpObject;
            // 获取 uri,过滤掉指定的资源
            URI uri = new URI(httpRequest.uri());
            if ("/favicon.ico".equals(uri.getPath())){
                System.out.println("request is /favicon.ico, don\'t operation...");
                return;
            }
            System.out.println("httpObject kind of --> " + httpObject.getClass());
            System.out.println("address of client --> " + channelHandlerContext.channel().remoteAddress());
            // 回复信息给浏览器 【http协议】
            ByteBuf byteBuf = Unpooled.copiedBuffer("hello, this is server...", CharsetUtil.UTF_8);
            // 构造一个http的响应，即 httpresponse
            DefaultFullHttpResponse defaultFullHttpResponse = new DefaultFullHttpResponse(
                    HttpVersion.HTTP_1_1, HttpResponseStatus.OK, byteBuf);
            // 设置头信息
            defaultFullHttpResponse.headers().set(HttpHeaderNames.CONTENT_TYPE,"text/plain");
            defaultFullHttpResponse.headers().set(HttpHeaderNames.CONTENT_LENGTH,byteBuf.readableBytes());
            // 将构建好的 DefaultFullHrrpResponse
            channelHandlerContext.writeAndFlush(defaultFullHttpResponse);
        }
    }
}


```
# Netty 核心模块组件
## Bootstrap、ServerBootstrap

1. Bootstrap 意思是引导，一个 Netty 应用通常由一个 Bootstrap 开始，主要作用是配置整个 Netty 程序，串联 各个组件，Netty 中 Bootstrap 类是客户端程序的启动引导类，ServerBootstrap 是服务端启动引导类
2. 常见的方法有
   1. public ServerBootstrap group(EventLoopGroup parentGroup, EventLoopGroup childGroup)，该方法用于服务器端，用来设置两个 EventLoop
   2. public B group(EventLoopGroup group) ，该方法用于客户端，用来设置一个 EventLoop
   3. public B channel(Class<? extends C> channelClass)，该方法用来设置一个服务器端的通道实现
   4. public <T> B option(ChannelOption<T> option, T value)，用来给 ServerChannel 添加配置
   5. public <T> ServerBootstrap childOption(ChannelOption<T> childOption, T value)，用来给接收到的通道添加配置
   6. public <T> ServerBootstrap childOption(ChannelOption<T> childOption, T value)，用来给接收到的通道添加配置
   7. public ChannelFuture bind(int inetPort) ，该方法用于服务器端，用来设置占用的端口号
   8. public ChannelFuture connect(String inetHost, int inetPort) ，该方法用于客户端，用来连接服务器端
## Future、ChannelFuture
Netty 中所有的 IO 操作都是异步的，不能立刻得知消息是否被正确处理。但是可以过一会等它执行完成或 者直接注册一个监听，具体的实现就是通过 Future 和 ChannelFutures，他们可以注册一个监听，当操作执行成功 或失败时监听会自动触发注册的监听事件

常见方法

1. Channel channel()，返回当前正在进行 IO 操作的通道 
2. ChannelFuture sync()，等待异步操作执行完毕

## Channel

1.  Netty 网络通信的组件，能够用于执行网络 I/O 操作。 
2.  通过 Channel 可获得当前网络连接的通道的状态 
3. 通过 Channel 可获得 网络连接的配置参数 （例如接收缓冲区大小） 
4. Channel 提供异步的网络 I/O 操作(如建立连接，读写，绑定端口)，异步调用意味着任何 I/O 调用都将立即返回，并且不保证在调用结束时所请求的 I/O 操作已完成 
5. 调用立即返回一个 ChannelFuture 实例，通过注册监听器到 ChannelFuture 上，可以 I/O 操作成功、失败或取消时回调通知调用方 
6. 支持关联 I/O 操作与对应的处理程序 
7. 不同协议、不同的阻塞类型的连接都有不同的 Channel 类型与之对应，常用的 Channel 类型:
- NioSocketChannel，异步的客户端 TCP Socket 连接。 
- NioServerSocketChannel，异步的服务器端 TCP Socket 连接。 
- NioDatagramChannel，异步的 UDP 连接。 
- NioSctpChannel，异步的客户端 Sctp 连接。 
- NioSctpServerChannel，异步的 Sctp 服务器端连接，这些通道涵盖了 UDP 和 TCP 网络 IO 以及文件 IO。
## Selector

1. Netty 基于 Selector 对象实现 I/O 多路复用，通过 Selector 一个线程可以监听多个连接的 Channel 事件。
2. 当向一个 Selector 中注册 Channel 后，Selector 内部的机制就可以自动不断地查询(Select) 这些注册的Channel 是否有已就绪的 I/O 事件（例如可读，可写，网络连接完成等），这样程序就可以很简单地使用一个线程高效地管理多个 Channel
## ChannelHandler 及其实现类

1. ChannelHandler 是一个接口，处理 I/O 事件或拦截 I/O 操作，并将其转发到其 ChannelPipeline(业务处理链) 中的下一个处理程序。
2. ChannelHandler 本身并没有提供很多方法，因为这个接口有许多的方法需要实现，方便使用期间，可以继承它 的子类
3. ChannelHandler 及其实现类一览图

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677829935764-a07bf55c-0e69-4d9a-8be6-a0a106a7d416.png#averageHue=%23f3f2ee&clientId=ufe1b904f-ab8a-4&from=paste&height=316&id=uf7f4880e&name=image.png&originHeight=355&originWidth=793&originalType=binary&ratio=2&rotation=0&showTitle=false&size=219373&status=done&style=shadow&taskId=u68aea5a1-d9bb-4154-8cc7-6683bd5eb6d&title=&width=706.5)

4. 我们经常需要自定义一个 Handler 类去继承 ChannelInboundHandlerAdapter，然后通过重写相应方法实现业务逻辑，我们接下来看看一般都需要重写哪些方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677830002174-736fc4a0-e698-45db-947b-5e000c04a9c4.png#averageHue=%23deded6&clientId=ufe1b904f-ab8a-4&from=paste&height=513&id=u0f1aefdc&name=image.png&originHeight=434&originWidth=601&originalType=binary&ratio=2&rotation=0&showTitle=false&size=315772&status=done&style=shadow&taskId=u172f530f-17a5-4508-8409-d2ece57fbae&title=&width=710.5)
## Pipeline和ChannelPipeline
>  ChannelPipeline是一个重点

1. ChannelPipeline是一个Handler的集合，它负责处理和拦截inbound或者outbound的事件和操作，相当于个贯穿Netty的链。（也可以这样理解：ChannelPipeline是保存ChannelHandler的List,用于处理或拦截Channel的入站事件和出站操作)
2. ChannelPipeline实现了一种高级形式的拦截过滤器模式，使用户可以完全控制事件的处理方式，以及Channel中各个的ChannelHandler如何相互交互
3. 在Netty中每个Channel都有且仅有一个ChannelPipeline与之对应，它们的组成关系如下

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1677830177332-8ffe317f-7d4b-456e-b77f-8afe18c76cf8.png#averageHue=%23dde2dd&clientId=ufe1b904f-ab8a-4&from=paste&height=313&id=uae3c7192&name=image.png&originHeight=358&originWidth=809&originalType=binary&ratio=2&rotation=0&showTitle=false&size=242666&status=done&style=shadow&taskId=u6c5b663a-5b1e-43da-80a7-97c256e7e75&title=&width=706.5)
> 常用方法

ChannelPipeline addFirst((ChannelHandler.handlers),把一个y业务处理类(handler)添加到链中的第一个位置
ChannelPipeline addLast((ChannelHandler..handlers),把一个业务处理类(handler)添加到链中的最后一个位置
## ChannelHandlerContext

1. 保存 Channel 相关的所有上下文信息，同时关联一个 ChannelHandler 对象
2. 即 ChannelHandlerContext 中 包 含 一 个 具 体 的 事 件 处 理 器 ChannelHandler ， 同 时 ChannelHandlerContext 中也绑定了对应的 pipeline 和 Channel 的信息，方便对 ChannelHandler 进行调用.
3. 常用方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678007906691-44c28177-dab5-4d54-982c-89e62c982e61.png#averageHue=%23dddad1&clientId=uc6f5f52a-0515-4&from=paste&height=109&id=uce415860&name=image.png&originHeight=122&originWidth=787&originalType=binary&ratio=2&rotation=0&showTitle=false&size=108865&status=done&style=shadow&taskId=u0ca70259-2efa-4458-8c14-5ce34051ca3&title=&width=703.5)
## ChannelOption

1. Netty 在创建 Channel 实例后,一般都需要设置 ChannelOption 参数。
2. ChannelOption 参数如下:

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678008001869-d14d8437-9b9a-43ab-a49f-24362b9e258b.png#averageHue=%23dcd8d0&clientId=uc6f5f52a-0515-4&from=paste&height=212&id=ua82db503&name=image.png&originHeight=235&originWidth=787&originalType=binary&ratio=2&rotation=0&showTitle=false&size=198403&status=done&style=shadow&taskId=u4faf10f3-3a48-45e9-887c-39291632ae1&title=&width=711.5)
## EnventLoopGroup及其实现类NioEnventLoopGroup

1. EventLoopGroup 是一组 EventLoop 的抽象，Netty 为了更好的利用多核 CPU 资源，一般会有多个 EventLoop同时工作，每个 EventLoop 维护着一个 Selector 实例。
2. EventLoopGroup 提供 next 接口，可以从组里面按照一定规则获取其中一个 EventLoop 来处理任务。在 Netty 服 务 器 端 编 程 中 ， 我 们 一 般 都 需 要 提 供 两 个 EventLoopGroup ， 例 如 ： BossEventLoopGroup 和 WorkerEventLoopGroup。
3. 通常一个服务端口即一个 ServerSocketChannel 对应一个 Selector 和一个 EventLoop 线程。BossEventLoop 负责 接收客户端的连接并将 SocketChannel 交给 WorkerEventLoopGroup 来进行 IO 处理，如下图所示

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678008313090-68ceeaa6-5807-4b68-acb9-cc82632fee0f.png#averageHue=%23e3e1d8&clientId=uc6f5f52a-0515-4&from=paste&height=261&id=u7010e3b3&name=image.png&originHeight=304&originWidth=847&originalType=binary&ratio=2&rotation=0&showTitle=false&size=280594&status=done&style=shadow&taskId=u56b53b11-dbb6-497d-a57e-ff5ede90572&title=&width=726.5)

4.  常用方法
   1. public NioEventLoopGroup()，构造方法
   2. public Future<?> shutdownGracefully()，断开连接，关闭线程
## Unpooled类

1. Netty 提供一个专门用来操作缓冲区(即 Netty 的数据容器)的工具类
2. 常用方法如下所示

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678008484575-ca1b269d-d238-4ce3-a245-4f74580f0451.png#averageHue=%23e8e6de&clientId=uc6f5f52a-0515-4&from=paste&height=84&id=u5cafe6b9&name=image.png&originHeight=103&originWidth=849&originalType=binary&ratio=2&rotation=0&showTitle=false&size=106056&status=done&style=shadow&taskId=u24f8f5d1-d48d-48aa-a8a2-a2b103550cd&title=&width=691.5)

3. 举例说明 Unpooled 获取 Netty 的数据容器 ByteBuf 的基本使用 【案例演示】

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678008556167-5fe48e95-0512-455e-8409-a88bfc616a57.png#averageHue=%23e9e9e8&clientId=uc6f5f52a-0515-4&from=paste&height=164&id=u2d7eb864&name=image.png&originHeight=206&originWidth=891&originalType=binary&ratio=2&rotation=0&showTitle=false&size=157072&status=done&style=shadow&taskId=u15e534f6-b326-418c-a78d-827a9daa59b&title=&width=710.5)
```java
package com.linmu.netty.unpool_;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/5 08:46:50
**/
@SuppressWarnings({"all"})
    public class TestBuf01 {
        public static void main(String[] args) {
            /**
* ButeBuf的概要
* 1.netty的buf中，不需要使用flip进行反转，底层维护了 readerIndex 和writerIndex
* 2.通过 readerIndex、writerIndex、capacity 将buffer分成三个区域
* 0---readerIndex  已经读取的范围
* readerIndex ---- writerIndex  可读区域
* writerIndex --- capacity  可写区域
*/
            ByteBuf byteBuf = Unpooled.buffer(10);
            int i = 0;
            /**
* isWritable 表示是否可写
* writeByte 表示可写
*/
            while(byteBuf.isWritable()){
                byteBuf.writeByte(i);
                i++;
            }
            /**
* isReadable 表示是否可读
* readByte 表示读取数据
*/
            System.out.print("this is read content -->\t");
            while (byteBuf.isReadable()) {
                System.out.print(byteBuf.readByte() + "\t");
            }
        }
    }

```
```java
package com.linmu.netty.unpool_;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.util.CharsetUtil;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/5 09:08:31
**/
@SuppressWarnings({"all"})
    public class TestBuf02 {
        public static void main(String[] args) {
            /**
* copiedBuffer方式创建Buf
* 需要指定编码格式
*/
            ByteBuf byteBuf = Unpooled.copiedBuffer("My name is Jackson Black...", CharsetUtil.UTF_8);

            /**
* 常见方法
*/
            if (byteBuf.hasArray()){
                byte[] array = byteBuf.array();
                System.out.println(new String(array,CharsetUtil.UTF_8));
            }
            System.out.println(byteBuf.arrayOffset());
            System.out.println(byteBuf.readerIndex());
            System.out.println(byteBuf.writerIndex());
            System.out.println(byteBuf.capacity());
            System.out.println(byteBuf.readableBytes());
            System.out.println(0);
            System.out.println(byteBuf.getCharSequence(0,5,CharsetUtil.UTF_8));
        }
    }

```
## Netty 应用实例-群聊系统
1) 编写一个 Netty 群聊系统，实现服务器端和客户端之间的数据简单通讯（非阻塞） 
2) 实现多人群聊 
3) 服务器端：可以监测用户上线，离线，并实现消息转发功能 
4) 客户端：通过 channel 可以无阻塞发送消息给其它所有用户，同时可以接受其它用户发送的消息(有服务器转发 得到) 
5) 目的：进一步理解 Netty 非阻塞网络编程机制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678008727112-b006438c-75fb-4fbe-91e8-d368e3167d7d.png#averageHue=%23eaeae6&clientId=uc6f5f52a-0515-4&from=paste&height=442&id=uf86e4bb4&name=image.png&originHeight=366&originWidth=606&originalType=binary&ratio=2&rotation=0&showTitle=false&size=205550&status=done&style=shadow&taskId=u7d8d3c0b-5b69-4bbc-af13-4eb014916d9&title=&width=732)
```java
package com.linmu.netty.groupchat;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.string.StringDecoder;
import io.netty.handler.codec.string.StringEncoder;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/5 09:33:24
**/
@SuppressWarnings({"all"})
    public class Server {

        private static final int PORT = 9999;

        public Server(){

        }

        /**
* 具体处理业务
*/
        public void run(){
            // 创建亮哥线程组
            EventLoopGroup boss = new NioEventLoopGroup(1);
            EventLoopGroup worker = new NioEventLoopGroup();

            try {
                // 创建 ServerBootstrap ，netty中默认从一个Bootstrap中开始
                ServerBootstrap serverBootstrap = new ServerBootstrap();
                serverBootstrap.group(boss, worker)
                    .channel(NioServerSocketChannel.class)
                    .option(ChannelOption.SO_BACKLOG, 128)
                    .childOption(ChannelOption.SO_KEEPALIVE, true)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            // 编码器
                            pipeline.addLast("decoder", new StringDecoder());
                            // 解码器
                            pipeline.addLast("encodeer", new StringEncoder());
                            // 自定义处理器
                            pipeline.addLast(new ServerHandler());
                        }
                    });
                System.out.println("Server is ready...");
                ChannelFuture channelFuture = serverBootstrap.bind(PORT).sync();
                channelFuture.channel().closeFuture().sync();
            }catch (InterruptedException e){
                throw new RuntimeException(e);
            } finally {
                boss.shutdownGracefully();
                worker.shutdownGracefully();
            }
        }

        public static void main(String[] args) {
            new Server().run();
        }
    }

```
```java
package com.linmu.netty.groupchat;

import io.netty.channel.Channel;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.channel.group.ChannelGroup;
import io.netty.channel.group.DefaultChannelGroup;
import io.netty.util.concurrent.GlobalEventExecutor;

import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 09:54:14
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends SimpleChannelInboundHandler<String> {

    /**
     * 定义一个channel组，管理所有的channel
     * GlobalEventExecutor.INSTANCE 表示全局的事件执行器，事宜个单例
     */
    private static ChannelGroup channelGroup = new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    /**
     * 1.连接建立，该方法是第一个被执行的方法
     * 2.把当前channel加入到channelGroup中
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
        Channel channel = ctx.channel();
        /**
         * 提示用户状态
         * writeAndFlush方法会自动循环channelgroup中的channel
         */
        channel.writeAndFlush("[Client]:" + channel.remoteAddress() + " is connecting..." +
                simpleDateFormat.format(new Date()) + "\n");
        // 将当前channel加入到channelGroup中
        channelGroup.add(channel);
    }

    /**
     * 断开连接时调用
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
        Channel channel = ctx.channel();
        channel.writeAndFlush("[Client]:" + channel.remoteAddress() + " is unconnectiing...." +
                simpleDateFormat.format(new Date()) + "\n");
        System.out.println("channelGroup size is --> " + channelGroup.size());
    }

    /**
     * channel活动时调用
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        System.out.println(ctx.channel().remoteAddress() + " is active..." +
                simpleDateFormat.format(new Date()) + "\n");
    }

    /**
     * channel不活动时调用
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelInactive(ChannelHandlerContext ctx) throws Exception {
        System.out.println(ctx.channel().remoteAddress() + " is left.... " +
                simpleDateFormat.format(new Date()) + "\n");
    }

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, String s) throws Exception {
        Channel channel = channelHandlerContext.channel();
        channelGroup.forEach(tempChannel -> {
            if (channel != tempChannel) {
                tempChannel.writeAndFlush("[Client]:" + channel.remoteAddress() + " send message " + s +
                        simpleDateFormat.format(new Date()) + "\n");
            } else {
                tempChannel.writeAndFlush("your send message is:" + s +
                        simpleDateFormat.format(new Date()) + "\n");
            }
        });
    }

    /**
     * 发生异常时调用该方法
     *
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.close();
    }
}

```
```java
package com.linmu.netty.groupchat;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.string.StringDecoder;
import io.netty.handler.codec.string.StringEncoder;

import java.util.Scanner;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 09:33:32
 **/
@SuppressWarnings({"all"})
public class Client {
    private final static String HOST = "localhost";
    private final int port;

    public Client(int port){
        this.port = port;
    }

    public void run(){
        EventLoopGroup group = new NioEventLoopGroup();

        Bootstrap bootstrap = new Bootstrap();
        try{
            bootstrap.group(group)
                    .channel(NioSocketChannel.class)
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            // 编码器
                            pipeline.addLast("encoder",new StringEncoder());
                            // 解码器
                            pipeline.addLast("decoder",new StringDecoder());
                            // 自定义处理器
                            pipeline.addLast("myHandler",new ClientHandler());
                        }
                    });
            ChannelFuture channelFuture = bootstrap.connect(HOST, port).sync();
            Channel channel = channelFuture.channel();
            System.out.println("[Client]" + channel.localAddress() + " is ready...");
            Scanner scanner = new Scanner(System.in);
            while(scanner.hasNextLine()){
                String message = scanner.nextLine();
                channel.writeAndFlush(message + "\n");
            }
//            channelFuture.channel().closeFuture().sync();
        } catch (InterruptedException e){
            throw new RuntimeException(e);
        } finally {
            group.shutdownGracefully();
        }
    }

    public static void main(String[] args) {
        new Client(9999).run();
    }
}

```
```java
package com.linmu.netty.groupchat;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 10:52:12
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends SimpleChannelInboundHandler<String> {

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, String s) throws Exception {
        System.out.println(s.trim());
    }
}

```
## Netty心跳机制检测案例
1) 编写一个 Netty 心跳检测机制案例, 当服务器超过 3 秒没有读时，就提示读空闲 
2) 当服务器超过 5 秒没有写操作时，就提示写空闲 
3) 实现当服务器超过 7 秒没有读或者写操作时，就提示读写空闲
```java
package com.linmu.netty.heartbeat;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.ServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.logging.LogLevel;
import io.netty.handler.logging.LoggingHandler;
import io.netty.handler.timeout.IdleStateHandler;

import java.util.concurrent.TimeUnit;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 13:46:59
 **/
@SuppressWarnings({"all"})
public class Server {
    public static void main(String[] args) {

        EventLoopGroup boss = new NioEventLoopGroup();
        EventLoopGroup worker = new NioEventLoopGroup();
        ServerBootstrap serverBootstrap = new ServerBootstrap();
        try {
            serverBootstrap.group(boss,worker)
                    .channel(NioServerSocketChannel.class)
                    .handler(new LoggingHandler(LogLevel.INFO))
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            /**
                             * 加入一个netty 提供的 IdleStateHandler
                             * 1.IdleStateHandler 是netty提供的处理空闲状态的处理器
                             * 2.long readerIdleTime:表示多长时间没有读操作，会发送一个心跳检测包检测是否连接
                             * 3.long writerIdleTime:表示多长时间没有写操作，会发送一个心跳检测包检测是否连接
                             * 4.long allIdleTime:表示多长时间没有读写操作，会发送一个心跳检测包检测是否连接
                             * 5.TimeUnit unit：表示时间单位
                             * 6.当idleStateEvent 触发后，就会传递给管道的下一个handler去处理，通过调用下一个handler的userEventTiggered，
                             * 在该方法中去处理 IdleStateEvent（读空闲，写空闲，读写空闲）
                             */
                            pipeline.addLast(new IdleStateHandler(3,4,6, TimeUnit.SECONDS));
                            // 加入自定义 心跳机制处理器
                            pipeline.addLast(new ServerHandler());
                        }
                    });
            // 启动服务器
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();
            channelFuture.channel().closeFuture().sync();
        }
        catch (InterruptedException e){
            throw new RuntimeException(e);
        }finally {
            boss.shutdownGracefully();
            worker.shutdownGracefully();
        }
    }
}

```
```java
package com.linmu.netty.heartbeat;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.handler.timeout.IdleStateEvent;
import io.netty.handler.timeout.IdleStateHandler;

import java.sql.SQLOutput;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 14:14:01
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends ChannelInboundHandlerAdapter {

    /**
     *
     * @param ctx
     * @param evt
     * @throws Exception
     */
    @Override
    public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
        if (evt instanceof IdleStateHandler){
            // 向下转型
            IdleStateEvent event = (IdleStateEvent) evt;
            String eventTyep = null;
            switch (event.state()){
                case READER_IDLE:
                    eventTyep = "READ_IDLE";
                    break;
                case WRITER_IDLE:
                    eventTyep = "WRITE_IDLE";
                    break;
                case ALL_IDLE:
                    eventTyep = "ALL_IDLE";
                    break;
            }
            System.out.println(ctx.channel().remoteAddress() + " time out " + eventTyep);
            System.out.println("server deal with....");
        }
    }
}

```
## Netty 通过 WebSocket 编程实现服务器和客户端长连接
1) Http 协议是无状态的, 浏览器和服务器间的请求响应一次，下一次会重新创建连接. 
2) 要求：实现基于 webSocket 的长连接的全双工的交互 
3) 改变 Http 协议多次请求的约束，实现长连接了， 服务器可以发送消息给浏览器 
4) 客户端浏览器和服务器端会相互感知，比如服务器关闭了，浏览器会感知，同样浏览器关闭了，服务器会感知 
5) 运行界面
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678009144320-0b07de53-da33-4e99-a71c-76208cb6ec7e.png#averageHue=%23f9f9f9&clientId=uc6f5f52a-0515-4&from=paste&height=347&id=u66c51cfb&name=image.png&originHeight=694&originWidth=1774&originalType=binary&ratio=2&rotation=0&showTitle=false&size=94333&status=done&style=shadow&taskId=u325c5a58-5dfd-4106-b421-e8b823fa803&title=&width=887)
```java
package com.linmu.netty.webSocket;

import com.linmu.netty.heartbeat.ServerHandler;
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.websocketx.WebSocketServerProtocolHandler;
import io.netty.handler.logging.LogLevel;
import io.netty.handler.logging.LoggingHandler;
import io.netty.handler.stream.ChunkedWriteHandler;
import io.netty.handler.timeout.IdleStateHandler;

import java.util.concurrent.TimeUnit;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 15:01:28
 **/
@SuppressWarnings({"all"})
public class Server {
    public static void main(String[] args) {
        EventLoopGroup boss = new NioEventLoopGroup();
        EventLoopGroup worker = new NioEventLoopGroup();
        ServerBootstrap serverBootstrap = new ServerBootstrap();
        try {
            serverBootstrap.group(boss,worker)
                    .channel(NioServerSocketChannel.class)
                    .handler(new LoggingHandler(LogLevel.INFO))
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            // 因为是基于http协议，需要使用使用http的编解码器
                            pipeline.addLast(new HttpServerCodec());
                            // 以块的方式写，添加ChunkedWriteHandler处理器
                            pipeline.addLast(new ChunkedWriteHandler());
                            /**
                             * 1.http数据传输过程是分段的，HttpObjectAggregator就是可以将多个分段聚合
                             * 2.这就是为什么，当浏览器发送大量数据时，就会发出多次http请求
                             */
                            pipeline.addLast(new HttpObjectAggregator(8192));
                            /**
                             * 1.对应 webSocket，它的数据时以帧（frame）的形式传播的
                             * 2.可以看到webSocketFrame下面有六个子类
                             * 3.浏览器请求时 ws://localhost:9999/hello  表示uri
                             * 4.WebSocketServerProtocolHandler 的核心功能是将http协议升级为ws协议，保持长链接
                             * 5.通过一个状态码转换协议 101
                              */
                            pipeline.addLast(new WebSocketServerProtocolHandler("/hello"));
                            // 自定义处理器，处理业务逻辑
                            pipeline.addLast(new FrameHandler());
                        }
                    });
            // 启动服务器
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();
            channelFuture.channel().closeFuture().sync();
        }
        catch (InterruptedException e){
            throw new RuntimeException(e);
        }finally {
            boss.shutdownGracefully();
            worker.shutdownGracefully();
        }
    }
}

```
```java
package com.linmu.netty.webSocket;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.handler.codec.http.websocketx.TextWebSocketFrame;

import java.time.LocalDateTime;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/5 15:21:24
 **/
@SuppressWarnings({"all"})
public class FrameHandler extends SimpleChannelInboundHandler<TextWebSocketFrame> {

    // TextWebSocketFrame 表示文本帧（frame）

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, TextWebSocketFrame textWebSocketFrame) throws Exception {
        System.out.println("server recieve message --> " + textWebSocketFrame.text());
        // 回复消息
        channelHandlerContext.channel().writeAndFlush(new TextWebSocketFrame("[server time]:" + LocalDateTime.now() + "-->" +
                textWebSocketFrame.text()));
    }

    /**
     * 客户端连接时触发
     * @param ctx
     * @throws Exception
     */
    @Override
    public void handlerAdded(ChannelHandlerContext ctx) throws Exception {
        /**
         * id channel的唯一标识
         * asLongText的值是唯一的，asShortText的值不唯一
         */
        System.out.println("handlerAdded is using --> " + ctx.channel().id().asLongText());
        System.out.println("handlerAdded is using --> " + ctx.channel().id().asShortText());
    }

    @Override
    public void handlerRemoved(ChannelHandlerContext ctx) throws Exception {
        System.out.println("handlerRemoved is using --> " + ctx.channel().id().asLongText());
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        System.out.println("ERROR --> "+ cause.getMessage());
        ctx.close();
    }
}

```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <form onsubmit="return false">
      <textarea name="message" style="height: 300px;width: 300px"></textarea>
      <input type="button" value="send message" onclick="send(this.form.message.value)">
      <textarea id="responseText" style="height: 300px;width: 300px" ></textarea>
      <input type="button" value="clear message" onclick="document.getElementById('responseText').value=''">
    </form>
    <script>
      var socket;
      // 判断当前浏览器是否支持webSocket
      if (window.WebSocket){
        socket = new WebSocket("ws://localhost:9999/hello");
        // 相当于channelRead0， event会接收到服务传送过来的数据
        socket.onmessage = function (event){
          var element = document.getElementById("responseText");
          element.value = element.value + "\n" + event.data;
        }
        //  相当于开启连接
        socket.onopen = function (event){
          var element = document.getElementById("responseText");
          element.value = "is connected"
        }
        // 相当于关闭连接
        socket.onclose = function (event){
          var elemnt = document.getElementById("responseText");
          elemnt.value = elemnt.value + "\n" + "connection is close..."
        }
      }else{
        alert("witout WebSocket....")
      }
      // 发送消息到服务器
      function send(message){
        if (!window.socket){ // 判断socket是否创建成功
          return;
        }
        if (socket.readyState == WebSocket.OPEN){
          // 通过socket发送消息
          socket.send(message);
        } else {
          alert("connect is failed...");
        }
      }
    </script>
  </body>
</html>
```
# Google Protobuf
## 编码和解码的基本介绍

1. 编写网络应用程序时，因为数据在网络中传输的都是二进制字节码数据，在发送数据时就需要编码，接收数据时就需要解码
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678269296567-7f29459c-facd-4cfc-8403-bf63b451e370.png#averageHue=%23f5f5f5&clientId=ub67407cb-fb72-4&from=paste&height=432&id=u266ab9b0&name=image.png&originHeight=863&originWidth=1834&originalType=binary&ratio=2&rotation=0&showTitle=false&size=90113&status=done&style=shadow&taskId=u2a642d39-9da8-47c7-bb55-53e25f2f4b1&title=&width=917)
2. codec(编解码器) 的组成部分有两个：decoder(解码器)和 encoder(编码器)。encoder 负责把业务数据转换成字节码数据，decoder 负责把字节码数据转换成业务数据
## Netty 本身的编码解码的机制和问题分析

1. Netty 自身提供了一些 codec(编解码器)
2. Netty 提供的编码器
   1. StringEncoder，对字符串数据进行编码
   2. ObjectEncoder，对 Java 对象进行编码
3. Netty 提供的解码器
   1. StringDecoder, 对字符串数据进行解码
   2. ObjectDecoder，对 Java 对象进行解码
4. Netty 本身自带的 ObjectDecoder 和 ObjectEncoder 可以用来实现 POJO 对象或各种业务对象的编码和解码，底层使用的仍是 Java 序列化技术 , 而 Java 序列化技术本身效率就不高，存在如下问题
   1. 无法跨语言 
   2. 序列化后的体积太大，是二进制编码的 5 倍多。 
   3. 序列化性能太低
5. 新的解决方案 [Google 的 Protobuf]
## Protobuf

1. Protobuf 基本介绍和使用示意图
2. Protobuf 是 Google 发布的开源项目，全称 Google Protocol Buffers，是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC[远程过程调用 remote procedure call ] 数据交换格式 
   1. **目前很多公司 http+json  tcp+protobuf**
3. 参考文档 : https://developers.google.com/protocol-buffers/docs/proto 语言指南
4. Protobuf 是以 message 的方式来管理数据的.
5. 支持跨平台、跨语言，即[客户端和服务器端可以是不同的语言编写的] （支持目前绝大多数语言，例如 C++、 C#、Java、python 等）
6. 高性能，高可靠性
7. 使用 protobuf 编译器能自动生成代码，Protobuf 是将类的定义使用.proto 文件进行描述。说明，在 idea 中编写 .proto 文件时，会自动提示是否下载 .ptotot 编写插件. 可以让语法高亮。
8. 然后通过 protoc.exe 编译器根据.proto 自动生成.java 文件
9. protobuf 使用示意图

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678269681714-429e540d-e44d-4323-b978-c681d298022a.png#averageHue=%23cfe3b6&clientId=ub67407cb-fb72-4&from=paste&height=158&id=u9f170c8e&name=image.png&originHeight=166&originWidth=759&originalType=binary&ratio=2&rotation=0&showTitle=false&size=132666&status=done&style=shadow&taskId=u7f2e5125-5483-46bb-b32e-ce5cbbd9f94&title=&width=720.5)
## Protobuf 快速入门实例1

1. 客户端可以发送一个 Student PoJo 对象到服务器 (通过 Protobuf 编码)
2. 服务端能接收 Student PoJo 对象，并显示信息(通过 Protobuf 解码)
```protobuf
//版本号
syntax = "proto3";
// 生成外部类名，同时也是文件名
option java_outer_classname = "StudentPOJO";
// protobuf 中使用 message 来管理数据
// 会在 StudentPOJO 外部类生成内部类 student, 他是真正发送的POJO对象
message Student {
  // optional表示限定符， int32 表示数据类型， id 表示属性名， 1表示属性序号，不是值
  optional int32 id = 1;
  optional string name = 2;
}
/**
字段格式：限定修饰符① | 数据类型② | 字段名称③ | = | 字段编码值/属性编号④ | [字段默认值⑤]
Required: 表示是一个必须字段，必须相对于发送方，在发送消息之前必须设置该字段的值，对于接收方，必须能够识别该字段的意思。发送之前没有设置required字段或者无法识别required字段都会引发编解码异常，导致消息被丢弃。

Optional：表示是一个可选字段，可选对于发送方，在发送消息时，可以有选择性的设置或者不设置该字段的值。对于接收方，如果能够识别可选字段就进行相应的处理，如果无法识别，则忽略该字段，消息中的其它字段正常处理。---因为optional字段的特性，很多接口在升级版本中都把后来添加的字段都统一的设置为optional字段，这样老的版本无需升级程序也可以正常的与新的软件进行通信，只不过新的字段无法识别而已，因为并不是每个节点都需要新的功能，因此可以做到按需升级和平滑过渡。

Repeated：表示该字段可以包含0~N个元素。其特性和optional一样，但是每一次可以包含多个值。可以看作是在传递一个数组的值。
*/
// protoc下载  --->  https://github.com/protocolbuffers/protobuf/releases/tag/v22.0
// protoc编译  --->  protoc.exe --java_out=. Student.proto
```
```java
package com.linmu.netty.codec;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.protobuf.ProtobufDecoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 13:40:34
 **/
@SuppressWarnings({"all"})
public class Server {

    public static void main(String[] args) {
        /**
         * 创建 bossGroup 和 workerGroup
         * 1.创建 bossGroup 线程组和 workerGroup 两个线程组
         * 2.bossGroup 只处理连接请求， wokerGroup 处理业务请求
         * 3.两个都是无限循环
         * 4.bossGroup 和 workerGroup 含有的子线程数（NioEventLoop）的个数
         *      默认 cpu核数 * 2
         */
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try{
            // 创建服务器的启动对象，配置参数
            ServerBootstrap serverBootstrap = new ServerBootstrap();

            // 使用链式编程设置参数
            // 设置两个线程组
            serverBootstrap.group(bossGroup, workerGroup)
                    // 使用 NioServerSocketChannel 作为服务器的通道实现
                    .channel(NioServerSocketChannel.class)
                    // 设置线程队列得到的连接个数
                    .option(ChannelOption.SO_BACKLOG,128)
                    // 设置线程保持连接状态
                    .childOption(ChannelOption.SO_KEEPALIVE,true)
                    // 给 workerGroup 的 EventLoop 对应的管道设置处理器
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        // 创建一个通道测试对象（匿名对象）
                        // 给 pipeline 设置处理器
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            // 添加解码器，需要指定解码对象
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            pipeline.addLast("protobufDecoder",new ProtobufDecoder(StudentPOJO.Student.getDefaultInstance()));
                            pipeline.addLast(new ServerHandler());
                        }
                    });
            System.out.println("server is ready...");

            // 绑定端口号并且同步，生成一个 ChannelFuture 对象
            // 启动服务器（并绑定端口号）
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch (InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

}
```
```java
package com.linmu.netty.codec;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.Channel;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:22:29
 **/
@SuppressWarnings({"all"})
/**
 * 1.自定义 Handler 需要继承 netty 规定好的 HandlerAdapter（规范）
 * 2.继承后，自定义的 Handler 才是被 netty 承认的 Handler
 */
public class ServerHandler extends ChannelInboundHandlerAdapter {

    /**
     * 读取数据时间（用于读取客户端发送来的数据）
     * 1.ChannelHandlerContext ctx：表示上下文对象，包含 管道pipeline，通道channel，地址
     * 2.Object msg：客户端发送的数据
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        // 读取从客户端发送的 StudentPOJO.Student
        StudentPOJO.Student student = (StudentPOJO.Student) msg;
        System.out.println("the message from client --> id=" + student.getId() + ",name=" + student.getName());
    }

    /**
     * 数据读取完毕
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        /**
         * writeAndFlush 是 write 和 flush
         * 将数据写入到缓存，并刷新
         * 一般来讲，需要对发送的数据进行编码
         */
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello client, I am server...", CharsetUtil.UTF_8));
    }

    /**
     * 异常处理，一般需要关闭通道
     *
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.close();
    }
}

```
```java
package com.linmu.netty.codec;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.protobuf.ProtobufEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:45:02
 **/
@SuppressWarnings({"all"})
public class Client {

    public static void main(String[] args) {

        // 客户端仅需要一个事件循环组
        EventLoopGroup ev = new NioEventLoopGroup();

        try{
            /**
             * 创建客户端启动对象
             * 注意客户端使用的不是 ServerBootstrap 而是 Bootstrap
             */
            Bootstrap bootstrap = new Bootstrap();

            // 设置相关参数
            // 设置线程组
            bootstrap.group(ev)
                    // 设置客户端通道的实现类（反射）
                    .channel(NioSocketChannel.class)
                    // 添加处理器
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            // 添加 ProtobufEncoder 编码器
                            pipeline.addLast("protobufEncoder",new ProtobufEncoder());
                            pipeline.addLast(new ClientHandler());
                        }
                    });
            System.out.println("client is ready...");

            /**
             * 启动客户端连接服务器
             * 关于 channelFuture 要分析，涉及到 netty 的异步模型
             */
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch(InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            ev.shutdownGracefully();
        }
    }
}

```
```java
package com.linmu.netty.codec;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 15:02:38
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends ChannelInboundHandlerAdapter {

    // 当通道就绪就会触发该方法
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        // 发送一个 Studnent 对象到 服务器
        StudentPOJO.Student studnet = StudentPOJO.Student.newBuilder().setId(7).setName("Jackson Black").build();
        ctx.writeAndFlush(studnet);
        System.out.println("client --> " + ctx);
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello server this is client...", CharsetUtil.UTF_8));
    }

    // 当通道有读取事件时触发

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf byteBuf = (ByteBuf) msg;
        System.out.println("message from server --> " + byteBuf.toString(CharsetUtil.UTF_8));
        System.out.println("address of server --> " + ctx.channel().remoteAddress());
    }

    // 异常处理

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}

```
> 注意：使用前需要添加处理器（Client和Server）

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678270000667-d677cc23-6964-4fd0-908a-ce092bb2ad35.png#averageHue=%23f9f8f7&clientId=ub67407cb-fb72-4&from=paste&height=173&id=ufc8b8dbc&name=image.png&originHeight=345&originWidth=2210&originalType=binary&ratio=2&rotation=0&showTitle=false&size=87365&status=done&style=shadow&taskId=ue794eded-8a46-4feb-98d7-72c3f119dc9&title=&width=1105)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678270068379-a41a837b-b07b-4f9c-837d-7cf126c0bba7.png#averageHue=%23f6f4ef&clientId=ub67407cb-fb72-4&from=paste&height=164&id=ud9da8ffd&name=image.png&originHeight=327&originWidth=1404&originalType=binary&ratio=2&rotation=0&showTitle=false&size=68743&status=done&style=shadow&taskId=uc66f6a44-42c3-4566-8f3b-fbc2ee65ada&title=&width=702)
## Protobuf 快速入门实例 2

1. 客户端可以随机发送 Student PoJo/ Worker PoJo 对象到服务器 (通过 Protobuf 编码
2. 服务端能接收 Student PoJo/ Worker PoJo 对象(需要判断是哪种类型)，并显示信息(通过 Protobuf 解码)
```protobuf
syntax = "proto3";
// 快速解析
option optimize_for = SPEED;
// 指定生成到哪一个包下
option java_package = "com.linmu.netty.codec.codec02";
// 外部类名
option java_outer_classname = "PeoplePOJO";

// protobuf 可以使用 message 管理其他的 message
message TopMessage {
  // 定义一个枚举类型
  enum Data {
    // 在 proto3 中要求enum的编号要求从0开始
    studentType = 0;
    teacherType = 1;
  }
  // 用 data_type 来标识传送的是哪一个枚举类
  optional Data data_type = 1;
  // 表示每次枚举类型最多只能出现一个，节省空间
  oneof data_body {
    Student student = 2;
    Teacher teacher = 3;
  }
}


message Student{
  optional int32 id = 1;
  optional string name = 2;
}

message Teacher{
  optional string name = 1;
  optional int32 salar = 2;
}
```
```java
package com.linmu.netty.codec.codec02;

import com.linmu.netty.codec.StudentPOJO;
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.handler.codec.protobuf.ProtobufDecoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 13:40:34
 **/
@SuppressWarnings({"all"})
public class Server {

    public static void main(String[] args) {
        /**
         * 创建 bossGroup 和 workerGroup
         * 1.创建 bossGroup 线程组和 workerGroup 两个线程组
         * 2.bossGroup 只处理连接请求， wokerGroup 处理业务请求
         * 3.两个都是无限循环
         * 4.bossGroup 和 workerGroup 含有的子线程数（NioEventLoop）的个数
         *      默认 cpu核数 * 2
         */
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try{
            // 创建服务器的启动对象，配置参数
            ServerBootstrap serverBootstrap = new ServerBootstrap();

            // 使用链式编程设置参数
            // 设置两个线程组
            serverBootstrap.group(bossGroup, workerGroup)
                    // 使用 NioServerSocketChannel 作为服务器的通道实现
                    .channel(NioServerSocketChannel.class)
                    // 设置线程队列得到的连接个数
                    .option(ChannelOption.SO_BACKLOG,128)
                    // 设置线程保持连接状态
                    .childOption(ChannelOption.SO_KEEPALIVE,true)
                    // 给 workerGroup 的 EventLoop 对应的管道设置处理器
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        // 创建一个通道测试对象（匿名对象）
                        // 给 pipeline 设置处理器
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            // 添加解码器，需要指定解码对象
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            pipeline.addLast("protobufDecoder",new ProtobufDecoder(
                                    PeoplePOJO.TopMessage.getDefaultInstance()
                            ));
                            pipeline.addLast(new ServerHandler());
                        }
                    });
            System.out.println("server is ready...");

            // 绑定端口号并且同步，生成一个 ChannelFuture 对象
            // 启动服务器（并绑定端口号）
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch (InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

}
```
```java
package com.linmu.netty.codec.codec02;

import com.linmu.netty.codec.StudentPOJO;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:22:29
 **/
@SuppressWarnings({"all"})
/**
 * 1.自定义 Handler 需要继承 netty 规定好的 HandlerAdapter（规范）
 * 2.继承后，自定义的 Handler 才是被 netty 承认的 Handler
 */
public class ServerHandler extends SimpleChannelInboundHandler<PeoplePOJO.TopMessage> {

    /**
     * 读取数据时间（用于读取客户端发送来的数据）
     * 1.ChannelHandlerContext ctx：表示上下文对象，包含 管道pipeline，通道channel，地址
     * 2.Object msg：客户端发送的数据
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelRead0(ChannelHandlerContext ctx, PeoplePOJO.TopMessage msg) throws Exception {
        // 根据不同的 PeoplePOJO.TopMessage 来显示不同的数据
        PeoplePOJO.TopMessage.Data dataType = msg.getDataType();
        if (dataType == PeoplePOJO.TopMessage.Data.studentType) {
            PeoplePOJO.Student student = msg.getStudent();
            System.out.println("name=" + student.getName() + ",id=" + student.getId());
        } else if (dataType == PeoplePOJO.TopMessage.Data.teacherType){
            PeoplePOJO.Teacher teacher = msg.getTeacher();
            System.out.println("name=" + teacher.getName() + ",salar=" + teacher.getSalar());
        } else {
            System.out.println("ERROR DATATYPE...");
        }
    }

    /**
     * 数据读取完毕
     *
     * @param ctx
     * @throws Exception
     */
    @Override
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        /**
         * writeAndFlush 是 write 和 flush
         * 将数据写入到缓存，并刷新
         * 一般来讲，需要对发送的数据进行编码
         */
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello client, I am server...", CharsetUtil.UTF_8));
    }

    /**
     * 异常处理，一般需要关闭通道
     *
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.close();
    }
}

```
```java
package com.linmu.netty.codec.codec02;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.SocketChannel;
import io.netty.channel.socket.nio.NioSocketChannel;
import io.netty.handler.codec.protobuf.ProtobufEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 14:45:02
 **/
@SuppressWarnings({"all"})
public class Client {

    public static void main(String[] args) {

        // 客户端仅需要一个事件循环组
        EventLoopGroup ev = new NioEventLoopGroup();

        try{
            /**
             * 创建客户端启动对象
             * 注意客户端使用的不是 ServerBootstrap 而是 Bootstrap
             */
            Bootstrap bootstrap = new Bootstrap();

            // 设置相关参数
            // 设置线程组
            bootstrap.group(ev)
                    // 设置客户端通道的实现类（反射）
                    .channel(NioSocketChannel.class)
                    // 添加处理器
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel socketChannel) throws Exception {
                            ChannelPipeline pipeline = socketChannel.pipeline();
                            // 添加 ProtobufEncoder 编码器
                            pipeline.addLast("protobufEncoder",new ProtobufEncoder());
                            pipeline.addLast(new ClientHandler());
                        }
                    });
            System.out.println("client is ready...");

            /**
             * 启动客户端连接服务器
             * 关于 channelFuture 要分析，涉及到 netty 的异步模型
             */
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();

            // 对关闭通道进行监听
            channelFuture.channel().closeFuture().sync();
        } catch(InterruptedException interruptedException){
            interruptedException.printStackTrace();
        } finally {
            ev.shutdownGracefully();
        }
    }
}

```
```java
package com.linmu.netty.codec.codec02;

import com.linmu.netty.codec.StudentPOJO;
import io.netty.buffer.ByteBuf;
import io.netty.buffer.PoolArenaMetric;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.ChannelInboundHandlerAdapter;
import io.netty.util.CharsetUtil;
import jdk.internal.org.objectweb.asm.commons.RemappingAnnotationAdapter;

import java.util.Random;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/1 15:02:38
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends ChannelInboundHandlerAdapter {

    // 当通道就绪就会触发该方法
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        // 随机发送 Student 或 Teacher 对象
        int random = new Random().nextInt(3);
        PeoplePOJO.TopMessage data = null;
        // 发送 Student 对象
        if (random == 0){
            data = PeoplePOJO.TopMessage.newBuilder().setDataType(PeoplePOJO.TopMessage.Data.studentType)
                    .setStudent(PeoplePOJO.Student.newBuilder().setId(7).setName("Jackson Black").build())
                    .build();
        // 发送 Teacher 对象
        } else {
            data = PeoplePOJO.TopMessage.newBuilder().setDataType(PeoplePOJO.TopMessage.Data.teacherType)
                    .setTeacher(PeoplePOJO.Teacher.newBuilder().setName("Jason").setSalar(300000).build())
                    .build();
        }
        ctx.writeAndFlush(data);
        System.out.println("client --> " + ctx);
        ctx.writeAndFlush(Unpooled.copiedBuffer("hello server this is client...", CharsetUtil.UTF_8));
    }

    // 当通道有读取事件时触发

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf byteBuf = (ByteBuf) msg;
        System.out.println("message from server --> " + byteBuf.toString(CharsetUtil.UTF_8));
        System.out.println("address of server --> " + ctx.channel().remoteAddress());
    }

    // 异常处理

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}

```
# Netty 编解码器和 handler 的调用机制
## 基本说明

1. netty 的组件设计：Netty 的主要组件有 Channel、EventLoop、ChannelFuture、ChannelHandler、ChannelPipe 等
2. ChannelHandler 充当了处理入站和出站数据的应用程序逻辑的容器。例如，实现 ChannelInboundHandler 接口（或 ChannelInboundHandlerAdapter），你就可以接收入站事件和数据，这些数据会被业务逻辑处理。当要给客户端 发 送 响 应 时 ， 也 可 以 从 ChannelInboundHandler 冲 刷 数 据 。 业 务 逻 辑 通 常 写 在 一 个 或 者 多 个 ChannelInboundHandler 中。ChannelOutboundHandler 原理一样，只不过它是用来处理出站数据的
3. ChannelPipeline 提供了 ChannelHandler 链的容器。以客户端应用程序为例，如果事件的运动方向是从客户端到 服务端的，那么我们称这些事件为出站的，即客户端发送给服务端的数据会通过 pipeline 中的一系列 ChannelOutboundHandler，并被这些 Handler 处理，反之则称为入站的![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678446591082-47abc2bf-1815-4669-bcb6-54a7a17e4bb5.png#averageHue=%23ededed&clientId=u501443e2-07ee-4&from=paste&height=139&id=u4cafe32a&name=image.png&originHeight=278&originWidth=905&originalType=binary&ratio=2&rotation=0&showTitle=false&size=68899&status=done&style=shadow&taskId=u66c10cc8-6018-4d73-8937-2e425e112c5&title=&width=452.5)
## 编解码器

1. 当 Netty 发送或者接受一个消息的时候，就将会发生一次数据转换。入站消息会被解码：从字节转换为另一种格式（比如 java 对象）；如果是出站消息，它会被编码成字节。
2. Netty 提供一系列实用的编解码器，他们都实现了 ChannelInboundHadnler 或者 ChannelOutboundHandler 接口。 在这些类中，channelRead 方法已经被重写了。以入站为例，对于每个从入站 Channel 读取的消息，这个方法会被调用。随后，它将调用由解码器所提供的 decode()方法进行解码，并将已经解码的字节转发给 ChannelPipeline中的下一个 ChannelInboundHandler
## 解码器-ByteToMessageDecoder

1. 关系继承图
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678446763506-0ff348d3-9919-4510-90e8-544df5d3ccf8.png#averageHue=%233a3835&clientId=u501443e2-07ee-4&from=paste&height=324&id=u1ddd71b0&name=image.png&originHeight=513&originWidth=763&originalType=binary&ratio=2&rotation=0&showTitle=false&size=176517&status=done&style=shadow&taskId=u14ec3405-b95e-4f5e-9e0c-897b759cbc5&title=&width=481.5)
2. 由于不可能知道远程节点是否会一次性发送一个完整的信息，tcp 有可能出现粘包拆包的问题，这个类会对入站数据进行缓冲，直到它准备好被处理. 
3. 一个关于 ByteToMessageDecoder 实例分析
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678446819898-d1cb2405-4324-4f1b-8f6d-b10adc91aee2.png#averageHue=%23d9d5cd&clientId=u501443e2-07ee-4&from=paste&height=170&id=u4d0f91e2&name=image.png&originHeight=340&originWidth=836&originalType=binary&ratio=2&rotation=0&showTitle=false&size=297132&status=done&style=shadow&taskId=u90dc2ae4-5da1-4c5a-9e4f-2db33bb748d&title=&width=418)
   2. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678446859089-a7204fe4-3afa-4a71-86d3-6574e942a59a.png#averageHue=%23a4c06c&clientId=u501443e2-07ee-4&from=paste&height=153&id=u61c1f95b&name=image.png&originHeight=305&originWidth=1044&originalType=binary&ratio=2&rotation=0&showTitle=false&size=214998&status=done&style=shadow&taskId=ue87f76c1-f0e5-4b16-ac08-111a8c27150&title=&width=522)、
## Netty 的 handler 链的调用机制
实例要求: 
1) 使用自定义的编码器和解码器来说明 Netty 的 handler 调用机制 
客户端发送 long -> 服务器 
服务端发送 long -> 客户端
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678446890632-2260ad69-9cd6-4993-b67b-34a38b7dec77.png#averageHue=%2331362c&clientId=u501443e2-07ee-4&from=paste&height=445&id=u18b14d8f&name=image.png&originHeight=890&originWidth=795&originalType=binary&ratio=2&rotation=0&showTitle=false&size=574016&status=done&style=shadow&taskId=u79af401f-2af0-4516-a4bc-c6a698cce38&title=&width=397.5)
2）demonstration
```java
package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.handler.codec.MessageToByteEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:48:32
 **/
@SuppressWarnings({"all"})
public class Encoder extends MessageToByteEncoder<Long> {


    /**
     * 若传入的数据不是指定的数据类型，则不会触该编码器
     * @param channelHandlerContext
     * @param l
     * @param byteBuf
     * @throws Exception
     */
    @Override
    protected void encode(ChannelHandlerContext channelHandlerContext, Long l, ByteBuf byteBuf) throws Exception {
        System.out.println("ClientEncoder encoder is runing...");
        System.out.println("message is ---> " + l);
        byteBuf.writeLong(l);
    }
}





package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.handler.codec.ByteToMessageDecoder;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:30:00
 **/
@SuppressWarnings({"all"})
public class Decoder extends ByteToMessageDecoder {

    /**
     * 若byteBuf中的数据不能一次性读取完毕，则该方法会被调用多次
     * 直到将byteBuf中的数据全部读取完为止
     * @param channelHandlerContext 上下文对象
     * @param byteBuf 入栈的handler
     * @param list list集合，会将解码后的数据传给下一个InBoundHandler
     * @throws Exception
     */
    @Override
    protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf, List<Object> list) throws Exception {
        System.out.println("ByteToMessageDecoder decode is runing...");
        // long 数据类型为8个字节,需要判断有8个字节才能读取数据
        if (byteBuf.readableBytes() >= 8){
            list.add(byteBuf.readLong());
        }
    }
}

```
```java
package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/10 14:18:19
**/
@SuppressWarnings({"all"})
    public class Server {
        public static void main(String[] args) {
            EventLoopGroup boss = new NioEventLoopGroup();
            EventLoopGroup worker = new NioEventLoopGroup();
            try{
                ServerBootstrap serverBootstrap = new ServerBootstrap();
                serverBootstrap.group(boss, worker)
                    .channel(NioServerSocketChannel.class)
                    // 自定义初始化类
                    .childHandler(new ServerInitailizer());
                ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();
                channelFuture.channel().closeFuture().sync();
            } catch (InterruptedException e){
                throw new RuntimeException(e);
            } finally {
                boss.shutdownGracefully();
                worker.shutdownGracefully();
            }
        }
    }




package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:26:59
 **/
@SuppressWarnings({"all"})
public class ServerInitailizer extends ChannelInitializer<SocketChannel> {

    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        // 添加入栈的ServerDecoder
        // pipeline.addLast(new Decoder());
        pipeline.addLast(new Decoder02());
        //  添加出栈的Encoder
        pipeline.addLast(new Encoder());
        // 自定义handler处理业务逻辑
        pipeline.addLast(new ServerHandlerOfMine());
    }
}




package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:36:33
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends SimpleChannelInboundHandler<Long> {

    /**
     * read content
     * @param channelHandlerContext
     * @param l
     * @throws Exception
     */
    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, Long l) throws Exception {
        System.out.println("message from client-->" + channelHandlerContext.channel().remoteAddress() +
                "-->message is-->" + l);
        channelHandlerContext.writeAndFlush(9999L);
    }

    /**
     * deal with Exception
     * @param ctx
     * @param cause
     * @throws Exception
     */
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}

```
```java
package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:41:29
 **/
@SuppressWarnings({"all"})
public class Client {
    public static void main(String[] args) {
        EventLoopGroup group = new NioEventLoopGroup();
        try{
            Bootstrap bootstrap = new Bootstrap();
            bootstrap.group(group).channel(NioSocketChannel.class)
                    .handler(new ClientInitailizer());
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();
            channelFuture.channel().closeFuture().sync();
        } catch (Exception e){

        } finally {
            group.shutdownGracefully();
        }
    }
}





package com.linmu.netty.inBoundHandlerAndOutboundHandler;


import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:47:06
 **/
@SuppressWarnings({"all"})
public class ClientInitailizer extends ChannelInitializer<SocketChannel> {

    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        // Encoder
        pipeline.addLast(new Encoder());
        // Decoder
        // pipeline.addLast(new Decoder());
        pipeline.addLast(new Decoder02());
        // ClientHandler
        pipeline.addLast(new ClientHandler());
    }
}




package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:55:01
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends SimpleChannelInboundHandler<Long> {

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, Long aLong) throws Exception {
        System.out.println("message from Server ip is" + channelHandlerContext.channel().remoteAddress() +
                "message is --> " + aLong);
    }

    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        System.out.println("client send message....");
        ctx.writeAndFlush(12345L);
    }
}

```
3) 结论
不论解码器 handler 还是 编码器 handler 即接收的消息类型必须与待处理的消息类型一致，否则该 handler 不会被执行 
在解码器 进行数据解码时，需要判断 缓存区(ByteBuf)的数据是否足够 ，否则接收到的结果会期望结果可能不一致
## 解码器-ReplayingDecoder

1. public abstract class ReplayingDecoder<S> extends ByteToMessageDecoder
2. ReplayingDecoder 扩展了 ByteToMessageDecoder 类，使用这个类，**我们不必调用 readableBytes()方法**。参数 S指定了用户状态管理的类型，其中 Void 代表不需要状态管理
3. 使用 ReplayingDecoder 编写解码器，对前面的案例进行简化 
```java
package com.linmu.netty.inBoundHandlerAndOutboundHandler;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.handler.codec.ReplayingDecoder;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 18:51:36
 **/
@SuppressWarnings({"all"})
public class Decoder02 extends ReplayingDecoder<Void> {

    @Override
    protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf, List<Object> list) throws Exception {
        System.out.println("Decoder02 decode running...");
        list.add(byteBuf.readLong());
    }
}

```
## 其他编码器

1. LineBasedFrameDecoder：这个类在 Netty 内部也有使用，它使用行尾控制字符（\n 或者\r\n）作为分隔符来解析数据。
2. DelimiterBasedFrameDecoder：使用自定义的特殊字符作为消息的分隔符
3. HttpObjectDecoder：一个 HTTP 数据的解码器
4. LengthFieldBasedFrameDecoder：通过指定长度来标识整包消息，这样就可以自动的处理黏包和半包消息。
5. 其他
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678447423237-b5634a3d-00bf-47b5-bd5f-31b07875f88a.png#averageHue=%23f0f1d8&clientId=u501443e2-07ee-4&from=paste&height=532&id=u1bbe6a57&name=image.png&originHeight=486&originWidth=613&originalType=binary&ratio=2&rotation=0&showTitle=false&size=466283&status=done&style=shadow&taskId=u79885451-e5d9-474e-b125-5265d01cabf&title=&width=671.5)
## Log4j 整合到 Netty
```xml
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.25</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>1.7.25</version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-simple</artifactId>
  <version>1.7.25</version>
  <scope>test</scope>
</dependency>
```
```properties
log4j.rootLogger=DEBUG, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=[%p] %C{1} - %m%n
```
# TCP 粘包和拆包 及解决方案
## TCP粘包和拆包简介

1. TCP 是面向连接的，面向流的，提供高可靠性服务。收发两端（客户端和服务器端）都要有一一成对的 socket，因此，发送端为了将多个发给接收端的包，更有效的发给对方，使用了优化方法（Nagle 算法），将多次间隔较小且数据量小的数据，合并成一个大的数据块，然后进行封包。这样做虽然提高了效率，但是接收端就难于分辨出完整的数据包了，因为面向流的通信是无消息保护边界的
2. 由于 TCP 无消息保护边界, 需要在接收端处理消息边界问题，也就是我们所说的粘包、拆包问题,
3. 示意图 TCP 粘包、拆包图解

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678601614949-ca982e4c-51e6-45c3-9aa5-316e63216c5e.png#averageHue=%23c2dab7&clientId=ufb46e663-2a4b-4&from=paste&height=353&id=uba1a25b1&name=image.png&originHeight=482&originWidth=961&originalType=binary&ratio=2&rotation=0&showTitle=false&size=182129&status=done&style=shadow&taskId=u29a0945e-c052-46a2-a185-1d26c61dee2&title=&width=703.5)
假设客户端分别发送了两个数据包 D1 和 D2 给服务端，由于服务端一次读取到字节数是不确定的，故可能存在以 
下四种情况：

- 服务端分两次读取到了两个独立的数据包，分别是 D1 和 D2，没有粘包和拆包
- 服务端一次接受到了两个数据包，D1 和 D2 粘合在一起，称之为 TCP 粘包
- 服务端分两次读取到了数据包，第一次读取到了完整的 D1 包和 D2 包的部分内容，第二次读取到了 D2 包的剩余内容，这称之为 TCP 拆包
- 服务端分两次读取到了数据包，第一次读取到了 D1 包的部分内容 D1_1，第二次读取到了 D1 包的剩余部分内容 D1_2 和完整的 D2 包。
## TCP 粘包和拆包现象实例
在编写 Netty 程序时，如果没有做处理，就会发生粘包和拆包的问题
```java
package com.linmu.netty.tcp;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/10 14:18:19
**/
@SuppressWarnings({"all"})
    public class Server {
        public static void main(String[] args) {
            EventLoopGroup boss = new NioEventLoopGroup();
            EventLoopGroup worker = new NioEventLoopGroup();
            try{
                ServerBootstrap serverBootstrap = new ServerBootstrap();
                serverBootstrap.group(boss, worker)
                    .channel(NioServerSocketChannel.class)
                    // 自定义初始化类
                    .childHandler(new ServerInitailizer());
                ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();
                channelFuture.channel().closeFuture().sync();
            } catch (InterruptedException e){
                throw new RuntimeException(e);
            } finally {
                boss.shutdownGracefully();
                worker.shutdownGracefully();
            }
        }
    }




package com.linmu.netty.tcp;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.ServerChannel;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:38:07
 **/
@SuppressWarnings({"all"})
public class ServerInitailizer extends ChannelInitializer<SocketChannel> {


    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        pipeline.addLast(new ServerHandler());
    }
}





package com.linmu.netty.tcp;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

import java.nio.charset.Charset;
import java.util.UUID;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:40:14
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends SimpleChannelInboundHandler<ByteBuf> {

    private int count = 0;
    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf) throws Exception {
        byte[] by = new byte[byteBuf.readableBytes()];
        byteBuf.readBytes(by);
        System.out.println("\tmessage from client --> " + new String(by,CharsetUtil.UTF_8));
        System.out.println("count --> " + (++count));
        // reply client
        ByteBuf reply = Unpooled.copiedBuffer(UUID.randomUUID().toString(), CharsetUtil.UTF_8);
        channelHandlerContext.writeAndFlush(reply);
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}


```
```java
package com.linmu.netty.tcp;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:41:29
 **/
@SuppressWarnings({"all"})
public class Client {
    public static void main(String[] args) {
        EventLoopGroup group = new NioEventLoopGroup();
        try{
            Bootstrap bootstrap = new Bootstrap();
            bootstrap.group(group).channel(NioSocketChannel.class)
                    .handler(new ClientInitailizer());
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();
            channelFuture.channel().closeFuture().sync();
        } catch (Exception e){

        } finally {
            group.shutdownGracefully();
        }
    }
}






package com.linmu.netty.tcp;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:29:10
 **/
@SuppressWarnings({"all"})
public class ClientInitailizer extends ChannelInitializer<SocketChannel> {

    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        pipeline.addLast(new ClientHandler());
    }
}





package com.linmu.netty.tcp;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:31:45
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends SimpleChannelInboundHandler<ByteBuf> {

    private int count = 0;

    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        for (int i = 0; i < 10; i++) {
            ByteBuf byteBuf = Unpooled.copiedBuffer("this is Client--> " + i, CharsetUtil.UTF_8);
            ctx.writeAndFlush(byteBuf);
        }
    }

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf) throws Exception {
        // receive reply from server
        byte[] bytes = new byte[byteBuf.readableBytes()];
        byteBuf.readBytes(bytes);
        System.out.println("\tmessage from server-->" + new String(bytes,CharsetUtil.UTF_8));
        System.out.println("count --> " + (++count));
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}

```

## TCP 粘包和拆包解决方案

1. 使用自定义协议 + 编解码器 来解决
2. 关键就是要解决 服务器端每次读取数据长度的问题, 这个问题解决，就不会出现服务器多读或少读数据的问题，从而避免的 TCP 粘包、拆包 。

案例

1. 要求客户端发送 5 个 Message 对象, 客户端每次发送一个 Message 对象
2. 服务器端每次接收一个 Message, 分 5 次进行解码， 每读取到 一个 Message , 会回复一个 Message 对象 给客户端.
```java
package com.linmu.netty.protocaltcp;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.handler.codec.MessageToByteEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 08:36:41
 **/
@SuppressWarnings({"all"})
public class Encoder_ extends MessageToByteEncoder<Protocal> {

    @Override
    protected void encode(ChannelHandlerContext channelHandlerContext, Protocal protocal, ByteBuf byteBuf) throws Exception {
        System.out.println("Encoder_ is running...");
        byteBuf.writeInt(protocal.getLength());
        byteBuf.writeBytes(protocal.getBytes());
    }
}




package com.linmu.netty.protocaltcp;

import io.netty.buffer.ByteBuf;
import io.netty.channel.ChannelHandlerContext;
import io.netty.handler.codec.MessageToByteEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 08:36:41
 **/
@SuppressWarnings({"all"})
public class Encoder_ extends MessageToByteEncoder<Protocal> {

    @Override
    protected void encode(ChannelHandlerContext channelHandlerContext, Protocal protocal, ByteBuf byteBuf) throws Exception {
        System.out.println("Encoder_ is running...");
        byteBuf.writeInt(protocal.getLength());
        byteBuf.writeBytes(protocal.getBytes());
    }
}




package com.linmu.netty.protocaltcp;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 08:25:20
 **/
@SuppressWarnings({"all"})
// protocal
public class Protocal {

    private int length;
    private byte[] bytes;

    public Protocal(int length, byte[] bytes) {
        this.length = length;
        this.bytes = bytes;
    }

    public int getLength() {
        return length;
    }

    public void setLength(int length) {
        this.length = length;
    }

    public byte[] getBytes() {
        return bytes;
    }

    public void setBytes(byte[] bytes) {
        this.bytes = bytes;
    }
}

```
```java
package com.linmu.netty.protocaltcp;

import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:18:19
 **/
@SuppressWarnings({"all"})
public class Server {
    public static void main(String[] args) {
        EventLoopGroup boss = new NioEventLoopGroup();
        EventLoopGroup worker = new NioEventLoopGroup();
        try{
            ServerBootstrap serverBootstrap = new ServerBootstrap();
            serverBootstrap.group(boss, worker)
                    .channel(NioServerSocketChannel.class)
                    // 自定义初始化类
                    .childHandler(new ServerInitailizer());
            ChannelFuture channelFuture = serverBootstrap.bind(9999).sync();
            channelFuture.channel().closeFuture().sync();
        } catch (InterruptedException e){
            throw new RuntimeException(e);
        } finally {
            boss.shutdownGracefully();
            worker.shutdownGracefully();
        }
    }
}





package com.linmu.netty.protocaltcp;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:38:07
 **/
@SuppressWarnings({"all"})
public class ServerInitailizer extends ChannelInitializer<SocketChannel> {


    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        pipeline.addLast(new Decoder_());
        pipeline.addLast(new Encoder_());
        pipeline.addLast(new ServerHandler());
    }
}




package com.linmu.netty.protocaltcp;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

import java.util.UUID;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:40:14
 **/
@SuppressWarnings({"all"})
public class ServerHandler extends SimpleChannelInboundHandler<Protocal> {

    private int count = 0;

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, Protocal protocal) throws Exception {
        int length = protocal.getLength();
        byte[] bytes = protocal.getBytes();
        System.out.println("message length is --> " + length);
        System.out.println("message from Client --> " + new String(bytes, CharsetUtil.UTF_8));
        System.out.println("count --> " + (++this.count) + "\n");
        // response client
        byte[] responseContent = UUID.randomUUID().toString().getBytes(CharsetUtil.UTF_8);
        int responseLength = responseContent.length;
        Protocal response = new Protocal(responseLength, responseContent);
        channelHandlerContext.writeAndFlush(response);
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        ctx.close();
    }
}


```
```java
package com.linmu.netty.protocaltcp;

import io.netty.bootstrap.Bootstrap;
import io.netty.channel.ChannelFuture;
import io.netty.channel.EventLoopGroup;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioSocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/10 14:41:29
 **/
@SuppressWarnings({"all"})
public class Client {
    public static void main(String[] args) {
        EventLoopGroup group = new NioEventLoopGroup();
        try{
            Bootstrap bootstrap = new Bootstrap();
            bootstrap.group(group).channel(NioSocketChannel.class)
                    .handler(new ClientInitailizer());
            ChannelFuture channelFuture = bootstrap.connect("localhost", 9999).sync();
            channelFuture.channel().closeFuture().sync();
        } catch (Exception e){

        } finally {
            group.shutdownGracefully();
        }
    }
}




package com.linmu.netty.protocaltcp;

import io.netty.channel.ChannelInitializer;
import io.netty.channel.ChannelPipeline;
import io.netty.channel.socket.SocketChannel;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:29:10
 **/
@SuppressWarnings({"all"})
public class ClientInitailizer extends ChannelInitializer<SocketChannel> {

    @Override
    protected void initChannel(SocketChannel socketChannel) throws Exception {
        ChannelPipeline pipeline = socketChannel.pipeline();
        pipeline.addLast(new Encoder_());
        pipeline.addLast(new Decoder_());
        pipeline.addLast(new ClientHandler());
    }
}




package com.linmu.netty.protocaltcp;

import io.netty.buffer.ByteBuf;
import io.netty.buffer.Unpooled;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.util.CharsetUtil;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/12 07:31:45
 **/
@SuppressWarnings({"all"})
public class ClientHandler extends SimpleChannelInboundHandler<Protocal> {

    private int count = 0;

    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        for (int i = 0; i < 10; i++) {
            String message = "Jackson Black";
            byte[] content = message.getBytes(CharsetUtil.UTF_8);
            int lenght = content.length;
            Protocal protocal = new Protocal(lenght,content);
            ctx.writeAndFlush(protocal);
        }
    }

    @Override
    protected void channelRead0(ChannelHandlerContext channelHandlerContext, Protocal byteBuf) throws Exception {
        // read reply
        byte[] bytes = byteBuf.getBytes();
        int length = byteBuf.getLength();
        System.out.println("the message from server-->" + new String(bytes, CharsetUtil.UTF_8));
        System.out.println("the message length is -->" + length);
        System.out.println("count --> " + (++this.count) + "\n");
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        ctx.close();
    }

}

```
# Netty核心源码剖析
**学完springBoot后再次学习**
