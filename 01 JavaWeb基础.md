# C/S模型与B/S模型
## C/S模型
> 服务器-客户机，即Client-Server([C/S](https://baike.baidu.com/item/C%2FS/826311))结构。C/S结构通常采取两层结构。服务器负责[数据](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE/5947370)的管理，客户机负责完成与用户的交互任务。
> 例如我们需要下载QQ、微信、电脑版吃鸡，如果该客户端软件需要升级，用户需要重新下载最新版本的客户端下载安装。

C（客户端Client）/S（Server）架构  桌面应用程序 
特点：需要下载安装后才能使用

## B/S模型
> B/S架构的全称为Browser/Server，即浏览器/服务器结构，


优点：易于维护升级，服务器端升级后，客户端(浏览器端无需升级就可获取最新的版本
缺点：会占用服务器端带宽资源。

> 静态web资源开发技术：Html 、js、css……
> 动态web资源开发技术：JSP/Servlet、ASP、PHP……
> 在Java中，动态web资源开发技术统称为Javaweb。


## 小结
C/S模型与B/S模型的关系，可以理解为手游和端游的关系，或者APP与网站的关系；
二者最主要的区别为，系统升级的方式的不同

# HTTP协议原理
## HTTP协议概述
> HTTP：基于HTTP传输协议（超文本传输协议）客户端与服务器端之间数据传输规则。


HTTP特点：

1. 基于TCP协议实现，面向连接
2. 基于请求与响应模型
3. HTTP协议属于无状态协议，对于事务处理没有任何记忆功能
4. HTTP协议的多次请求无法共享，在JavaWeb开发中通过cookie、session解决
5. HTTP协议进行同步数据传输（即请求与响应）

## HTTP请求格式
请求行：请求的第一行 组成：请求方法、URL 以及协议版本，之间由空格分隔

1. 请求方法包括GET、HEAD、PUT、POST、TRACE、OPTIONS、DELETE以及扩展方法，当然并不是所有的服务器都实现了所有的方法，部分方法即便支持，处于安全性的考虑也是不可用的
2. 协议版本的格式为：HTTP/主版本号.次版本号，常用的有HTTP/1.0和HTTP/1.1

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673608837348-524c5b6c-3fa2-4174-91cf-c305414e3afd.png#averageHue=%23fefefe&clientId=u4ffe0806-48cb-4&from=paste&height=196&id=u0e331de3&name=image.png&originHeight=391&originWidth=1496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68840&status=done&style=shadow&taskId=u5ae29bfe-f541-4e35-ae41-06aa2118811&title=&width=748)

get与post请求区别

1. get请求请求的参数在请求行中，没有请求体；
2. post请求请求参数在请求体中；
3. get请求请求参数有大小限制，post请求没有

请求头：除去第一行的部分，以键值对的形式显示常见http协议请求头
Host:接受请求的服务器地址，可以是IP:端口号，也可以是域名
User-Agent:发送请求的应用程序名称
Connection:指定与连接相关的属性，如Connection:Keep-Alive
Accept-Charset:通知服务端可以发送的编码格式
Accept-Encoding:通知服务端可以发送的数据压缩格式
Accept-Language:通知服务端可以发送的语言
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673608857994-75d86316-0dca-4cc3-976f-a0af35b267ea.png#averageHue=%23d6e0ee&clientId=u4ffe0806-48cb-4&from=paste&height=223&id=u413711ef&name=image.png&originHeight=445&originWidth=1494&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100954&status=done&style=shadow&taskId=u2bad6f5d-dede-4cdd-9be3-ecc904d7137&title=&width=747)

请求体：即 请求的参数post请求的最后一部分，存放发送请求的参数
userName=mayikt&age=26

## HTTP响应格式

响应行：响应数据第一行组成：协议及版本号、状态码、状态
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673609069802-2af8677a-db3e-4129-b0d8-c30abcbcf5e5.png#averageHue=%23fefefe&clientId=u4ffe0806-48cb-4&from=paste&height=216&id=u7309251f&name=image.png&originHeight=431&originWidth=874&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46259&status=done&style=shadow&taskId=u5a7042b7-022a-49ff-b5ce-eb66061072a&title=&width=437)

响应头：第二行开始 格式 key value![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673609131205-67090c43-c012-4800-9b9a-26ac4e783604.png#averageHue=%23fefefe&clientId=u4ffe0806-48cb-4&from=paste&height=221&id=ue17a96da&name=image.png&originHeight=441&originWidth=899&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46847&status=done&style=shadow&taskId=u6d5286fa-d745-420a-8f8e-59f08b6ef83&title=&width=449.5)

响应体：存放服务器响应给客户端的内容

## HTTP响应状态码
分类
1XX：临时响应，表示临时响应并需要请求者继续执行操作100： （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。
101： （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。
2XX：成功200： （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
201： （已创建） 请求成功并且服务器创建了新的资源。
202： （已接受） 服务器已接受请求，但尚未处理。
203： （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。
204： （无内容） 服务器成功处理了请求，但没有返回任何内容。
205： （重置内容） 服务器成功处理了请求，但没有返回任何内容。
206： （部分内容） 服务器成功处理了部分 GET 请求。
3XX：重定向，表示要完成请求，需要进一步操作。300： （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。
301： （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。
302： （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
303： （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
304： （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
305： （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
307： （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

4XX：请求错误，一般是客户端问题，请求可能出错，妨碍了服务器的处理。400： （错误请求） 服务器不理解请求的语法。
401： （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
403： （禁止） 服务器拒绝请求。
404： （未找到） 服务器找不到请求的网页。
405： （方法禁用） 禁用请求中指定的方法。
406： （不接受） 无法使用请求的内容特性响应请求的网页。
407： （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。
408： （请求超时） 服务器等候请求时发生超时。
409： （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。
410： （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。
411： （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。
412： （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。
413： （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。
414： （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。
415： （不支持的媒体类型） 请求的格式不受请求页面的支持。
416： （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。
417： （未满足期望值） 服务器未满足"期望"请求标头字段的要求。
5XX：服务器错误，一般是服务端问题，服务器在尝试处理请求时发生内部错误。500： （服务器内部错误） 服务器遇到错误，无法完成请求。
501： （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
502： （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
503： （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
504： （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。
505： （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。

# socket
## 概述

1. 计算机网络是通过传输介质、通信设施和网络通信协议，把分散在不同地点的计算机设备互连起来，实现资源共享和数据传输的系统。网络编程就就是编写程序使联网的两个(或多个)设备(例如计算机)之间进行数据传输。Java语言对网络编程提供了良好的支持，通过其提供的接口我们可以很方便地进行网络编程。例如我们的QQ聊天
2. Java是 Internet 上的语言，它从语言级上提供了对网络应用程 序的支持，程序员能够很容易开发常见的网络应用程序。
3. Java提供的网络类库，可以实现无痛的网络连接，联网的底层细节被隐藏在 Java 的本机安装系统里，由 JVM 进行控制。并 且 Java 实现了一个跨平台的网络库，程序员面对的是一个统一的网络编程环境。
## 网络通信三要素

1. IP地址：是对MAC地址的映射
2. 端口号： 范围为0~65535（其中包含专用、泛用），一个端口号对应一个进程
3. 协议：实现网络通信的规则，可以理解为计算机要遵守的法律，如：TCP、UDp

## InetAddress
作用：用于封装IP地址，并提供一系列方法获取IP地址的相关信息

| 方法 | 功能 |
| --- | --- |
| InetAddress getByName(String host) | 获取给定主机名的的IP地址，host参数表示指定主机 |
| InetAddress getLocalHost() | 获取本地主机地址 |
| String getHostName() | 获取本地IP地址的主机名 |
| String getHostAddress() | 获取字符串格式的原始IP地址 |

```java
import java.net.InetAddress;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/10 21:08:36
 **/
@SuppressWarnings({"all"})
public class jave23 {
    public static void main(String[] args) throws Exception {
        InetAddress inetAddress = InetAddress.getByName("localhost");
        InetAddress localHost = InetAddress.getLocalHost();
        String hostName = inetAddress.getHostName();
        String hostAddress = inetAddress.getHostAddress();
        System.out.println(inetAddress + "\t\t" + localHost + "\t" +
                hostName + "\t" + hostAddress);
    }
}

```
> 注意“localhost”与“127.0.0.1”都表示本机

## DNS域名解析
> 将IP地址映射为域名

```java
import java.net.InetAddress;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/10 21:08:36
 **/
@SuppressWarnings({"all"})
public class jave23 {
    public static void main(String[] args) throws Exception {
        InetAddress inetAddress = InetAddress.getByName("www.baidu.com");
        System.out.println(inetAddress);
    }
}
```
# UDP协议
## 概述

1. 作用：为程序提供无连接的数据传输方式
2. 特点：面向无连接、不可靠传输、速度快

## 发送数据

1. 创建发送端socket对象；
2. 提供数据，并将数据封装到数据包中；
3. 通过socket服务的发送功能，将数据包发出去；
4. 释放资源；
```java
package javaWeb_Socket;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/10 12:52:03
 **/
@SuppressWarnings({"all"})
public class Udp_Client {
    public static void main(String[] args) throws Exception{
        // 创建发送端socket对象；
        DatagramSocket datagramSocket = new DatagramSocket();
    	// 提供数据，并将数据封装到数据包中；
        byte[] massage = "my name is jackson".getBytes();
        InetAddress name = InetAddress.getByName("小新");
        DatagramPacket datagramPacket = new DatagramPacket(massage, 
                massage.length, name, 9527);
    	// 通过socket服务的发送功能，将数据包发出去；
        System.out.println("send massage.....");
        datagramSocket.send(datagramPacket);
    	// 释放资源；
        System.out.println("massage is sended....");
        datagramSocket.close();
        System.out.println("link is over......");
    }
}
```

## 接收数据

1. 创建接收端socket对象；
2. 接收数据；
3. 解析数据；
4. 输出数据；
5. 释放资源；
```java
package javaWeb_Socket;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/10 13:10:52
**/
@SuppressWarnings({"all"})
    public class Udp_Server {
        public static void main(String[] args) throws Exception {
            // 1. 创建接收端socket对象；
            DatagramSocket datagramSocket = new DatagramSocket(9527);
            // 2. 接收数据；
            System.out.println("receive massage.....");
            byte[] bytes = new byte[1024];
            DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length);
            // 3. 解析数据；
            datagramSocket.receive(datagramPacket);
            System.out.println("massage is received.....");
            String massage = new String(datagramPacket.getData());
            // 4. 输出数据；
            System.out.println("massage is:" + massage);
            // 5. 释放资源；
            datagramSocket.close();
            System.out.println("link is over....");
        }
    }
```

## 练习
> clien一直发送数据，server一直接收数据

```java
package javaWeb_Socket.UdpPractice;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/10 13:42:57
**/
@SuppressWarnings({"all"})
    public class Udp_Server {
        public static void main(String[] args) throws Exception {
            DatagramSocket datagramSocket = new DatagramSocket(7710);
            int count = 0;
            while (count < 100){
                System.out.println("waiting for recieve massage...");
                count ++;
                byte[] bytes = new byte[1024];
                DatagramPacket datagramPacket = new DatagramPacket(bytes, bytes.length);
                datagramSocket.receive(datagramPacket);
                System.out.println("massage is received:" + 
                                   new String(datagramPacket.getData()));
            }
            datagramSocket.close();
        }
    }
```
```java
package javaWeb_Socket.UdpPractice;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.util.Scanner;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/10 13:31:50
**/
@SuppressWarnings({"all"})
    public class Udp_Client {
        public static void main(String[] args) throws Exception {
            boolean flag = true;
            DatagramSocket datagramSocket = new DatagramSocket();
            DatagramPacket datagramPacket;
            while (flag){
                Scanner scanner = new Scanner(System.in);
                System.out.print("input massage:");
                // next() : 读取数据   nextLine() : 读取一行数据
                String massage = scanner.nextLine();
                flag = massage == "q" ? false : true;
                byte[] bytes = massage.getBytes();
                datagramPacket = new DatagramPacket(bytes, bytes.length,
                                                    InetAddress.getByName("小新"),7710);
                datagramSocket.send(datagramPacket);
                System.out.println("massage is sended...");
            }
            datagramSocket.close();
        }
    }
```

# TCP协议
> 三次握手和四次挥手

![8b04b61f561bd42e6cd5626726a5943e.jpeg](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1673613870609-431fc901-47a6-4a71-b185-8446506257aa.jpeg#averageHue=%23faf6ef&clientId=u4ffe0806-48cb-4&from=paste&height=355&id=ua94e6213&name=8b04b61f561bd42e6cd5626726a5943e.jpeg&originHeight=709&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59281&status=done&style=shadow&taskId=u15eca631-9091-4c14-baaf-56573299c62&title=&width=540)

## 客户端
```java
package javaWeb_Socket;

import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/10 14:08:22
**/
@SuppressWarnings({"all"})
    public class Tcp_Client {
        public static void main(String[] args) throws Exception {
            // 1.创建发送端Socket对象（创建连接）
            Socket socket = new Socket(InetAddress.getByName("小新"), 9527);
            // 2.获取输出流对象；
            OutputStream outputStream = socket.getOutputStream();
            String massage = "my name is jackson";
            System.out.println("massage is sending...");
            //  3.发送数据；
            outputStream.write(massage.getBytes());
            System.out.println("massage is sended...");
            // 4.释放资源；
            outputStream.close();
            socket.close();
        }
    }
```

## 服务器端

1. 创建接收端Socket对象；
2. 监听（阻塞:如果建立连接失败，程序会一直阻塞，不往下执行）；
3. 获取输入流对象；
4. 获取数据；
5. 输出数据；
6. 释放资源；
```java
package javaWeb_Socket;

import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/10 14:13:27
**/
@SuppressWarnings({"all"})
    public class Tcp_Server {
        public static void main(String[] args) throws Exception {
            // 1.创建接收端Socket对象；
            ServerSocket serverSocket = new ServerSocket(9527);
            // 2.监听（阻塞:如果建立连接失败，程序会一直阻塞，不往下执行）；
            Socket accept = serverSocket.accept();
            // 3.获取输入流对象；
            InputStream inputStream = accept.getInputStream();
            System.out.println("waiting for massage...");
            // 4.获取数据；
            byte[] bytes = new byte[1024];
            int length = inputStream.read(bytes);
            // 5.输出数据；
            System.out.println("massage is: " + new String(bytes, 0, length));
            // 6.释放资源；
            inputStream.close();
            accept.close();
            serverSocket.close();
        }
    }
```

## 练习
要求：使用tcp协议 客户端可以一直发送数据给服务器端，服务器端可以一直接受到客户端发送的数据。
如果客户端输入 666 就会直接退出程序。
```java
package javaWeb_Socket.TCPPractice;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.UUID;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/13 20:53:14
 **/
@SuppressWarnings({"all"})
public class Server01 {
    public static void main(String[] args) throws IOException {
        // 创建serversocket对象
        ServerSocket serverSocket = new ServerSocket(8080);
        System.out.println("服务器端启动成功....");
        while (true) {
            // 接受客户端数据
            Socket socket = serverSocket.accept();
            //inputStream
            InputStream inputStream = socket.getInputStream();
            byte[] bytes = new byte[1024];
            int len = inputStream.read(bytes);
            System.out.println("服务器端接受客户端：" + new String(bytes, 0, len));
            // 服务端响应数据给客户端
            OutputStream outputStream = socket.getOutputStream();
            String resp = "我收到啦" + UUID.randomUUID().toString();
            outputStream.write(resp.getBytes());
            inputStream.close();
            outputStream.close();
            socket.close();
        }
    }
}

```
```java
package javaWeb_Socket.TCPPractice;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.Scanner;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/13 20:56:08
 **/
@SuppressWarnings({"all"})
public class Client01 {
    public static void main(String[] args) throws IOException {

        while (true) {
            // 创建socket对象
            Socket socket = new Socket("127.0.0.1", 8080);
            System.out.println("客户端：请输入发送数据的内容");
            Scanner sc = new Scanner(System.in);
            String context = sc.nextLine();
            if (context.equals("666")) {
                break;// 退出循环
            }
            // 获取getOutputStream
            OutputStream outputStream = socket.getOutputStream();
            // 写入数据给服务器端
            outputStream.write(context.getBytes());
            // 接受服务器端响应的内容
            InputStream inputStream = socket.getInputStream();
            byte[] bytes = new byte[1024];
            int len = inputStream.read(bytes);
            System.out.println("服务端响应数据给客户端：" + new String(bytes, 0, len));
            outputStream.close();
            socket.close();
        }

    }
}

```

# HTML

# CSS

# JavaScript
