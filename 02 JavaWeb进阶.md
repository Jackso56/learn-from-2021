# Tomcat服务器
## Web服务器简介

1. Tomcat：由Apache组织提供的一种Web服务器，提供对jsp和Servlet的支持。它是一种轻量级的javaWeb容器（服务器），也是当前应用最广的JavaWeb服务器（免费）。Tomcat主要实现了Java EE中的Servlet、JSP规范，同时也提供HTTP服务，是市场上非常流行的Java Web容器。
2. Jboss：是一个遵从JavaEE规范的、开放源代码的、纯Java的EJB服务器，它支持所有的JavaEE规范（免费）。
3. GlassFish： 由Oracle公司开发的一款JavaWeb服务器，是一款强健的商业服务器，达到产品级质量（应用很少，收费）。
4. Resin：是CAUCHO公司的产品，是一个非常流行的应用服务器  的支持，性能也比较优良，resin自身采用JAVA语言开发（收费，应用比较多）。
5. WebLogic：是Oracle公司的产品，是目前应用最广泛的Web服务器，支持JavaEE规范，而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

> web服务器 底层是如何实现 基于tcp协议封装 http协议、springboot框架 底层内嵌入我们的 Tomcat服务器
> web服务器是一个应用程序(软件)，对http协议的进行封装，让web开发更加便捷。


tomcat下载地址：[https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi) 
注意 如果大家下载好了tomcat安装包之后 ，tomcat安装位置 不要带中文、不要带任何空格路径。
纯英文路径下运行tomcat。
## Tomcat服务器的基本使用

1. bin文件夹：主要存放tomcat的操作命令，根据操作系统可以分为两大类：一是以.bat结尾（Windows）；二是以.sh结尾（Linux）

*.bat---运行在windows批处理文件：startup.bat ----启动tomcat，shutdown.bat---停止tomcat
*.sh-----linux环境中运行文件

2. conf文件夹： 存放全局配置文件 修改tomcat启动端口号码  logging.properties 
3. webapps文件夹： 存放运行程序 部署war包、jar包、静态资源。
4. lib文件夹：  tomcat 需要依赖一些jar包
5. logs 文件夹：存放 tomcat一些日志
6. temp文件夹：存放临时文件
7. work文件夹：jsp文件在被翻译之后，保存在当前这个目录下，session对象被序列化之后保存的位置

## 启动Tomcat服务器常见问题

1. 启动tomcat控制台乱码
   1. 打开tomcat/conf/logging.properties	

将代码“java.util.logging.ConsoleHandler.encoding = UTF-8”注释掉即可，原因是编码方式不一致

2. 启动tomcat闪退问题

是jdk环境变量的问题，检查下jdk安装的环境变量即可

3. 关闭tomcat服务器
   1. Ctrl+C键 关闭Tomcat服务器
   2. 点击Tomcat窗口的右上角关闭按钮 （暴力停止服务器）
   3. 找到tomcat目录/shutdown.bat文件，双击执行关闭Tomcat
## Tomcat服务器配置
> 修改端口号：找到tomcat目录/conf/server.xml，修改port的值，将port端口的值修改即可

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673616951055-c4ef90d6-455d-459c-86df-e20cf2ce9cc9.png#averageHue=%23f2f1f0&clientId=u0a959e73-7130-4&from=paste&height=367&id=u79041180&name=image.png&originHeight=733&originWidth=1633&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103093&status=done&style=shadow&taskId=u423eb255-7db9-4082-b066-dd25ebdb63c&title=&width=816.5)

## Tomcat服务器web开发项目结构

1. 创建项目

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673617932480-6e4ca9ab-87dd-4c7b-9573-55af3349ff63.png#averageHue=%23f1f1f0&clientId=u0a959e73-7130-4&from=paste&height=336&id=uc1c4e26f&name=image.png&originHeight=672&originWidth=1613&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44891&status=done&style=shadow&taskId=u9be05ac4-6969-4838-a342-5ed34d86707&title=&width=806.5)

2. 新增 add framework support  

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673617986614-0a76e83b-8fb6-4c5f-b247-ad87a5ec5ebf.png#averageHue=%23e8e8e7&clientId=u0a959e73-7130-4&from=paste&height=269&id=u32a40750&name=image.png&originHeight=537&originWidth=1652&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64109&status=done&style=shadow&taskId=u75fb0f57-9837-44a1-8896-603186a0f2e&title=&width=826)

3. 选择web application

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673618185723-6b779431-3e1f-4acf-9811-cfa349203beb.png#averageHue=%23ebeaea&clientId=u0a959e73-7130-4&from=paste&height=235&id=u757f54d2&name=image.png&originHeight=469&originWidth=1935&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81431&status=done&style=shadow&taskId=u9657b16a-4b60-4b15-a096-c66cc1a7f87&title=&width=967.5)

4. 新增tomcat

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673618226931-b9867be8-da5f-43be-b291-9e15d07ab235.png#averageHue=%23ededed&clientId=u0a959e73-7130-4&from=paste&height=351&id=u49186daa&name=image.png&originHeight=701&originWidth=1372&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78105&status=done&style=shadow&taskId=ubd381e42-2a1e-4dab-b774-2b4a9f4a979&title=&width=686)

5. 选择tomcat server，添加tomcat 路径

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673618262343-848b7250-7a0f-4e53-868c-ad3b79aec406.png#averageHue=%23e9e8e8&clientId=u0a959e73-7130-4&from=paste&height=530&id=u52946689&name=image.png&originHeight=1059&originWidth=1412&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134536&status=done&style=shadow&taskId=u1c120262-eb6f-4428-b30f-0d9f0be7b4e&title=&width=706)

6. 添加当前java项目

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673618312249-93925f98-1861-43f4-b509-a569649c3cdc.png#averageHue=%23f2f2f2&clientId=u0a959e73-7130-4&from=paste&height=371&id=u0411ac14&name=image.png&originHeight=741&originWidth=1488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68597&status=done&style=shadow&taskId=ud3f542df-8e02-428d-b3aa-74f5e9814d7&title=&width=744)

> web目录结构说明： web-inf 目录  该目录外界是无法访问的


## maven项目pom.xml文件需要修改（仅限tomcat-10）
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674182676984-2637b8dd-b008-4de6-8a8c-987cc261a823.png#averageHue=%23f5f5f4&clientId=ub4890bb8-1719-4&from=paste&height=852&id=u20b84da8&name=image.png&originHeight=1704&originWidth=2880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=493334&status=done&style=shadow&taskId=u0c388431-7205-4b99-8b1e-2ffe7081a85&title=&width=1440)
```xml
<dependency>
    <groupId>jakarta.servlet</groupId>
    <artifactId>jakarta.servlet-api</artifactId>
    <version>5.0.0</version>
    <scope>provided</scope>
</dependency>
```

# servlet
## 什么是servlet
> 定义：Servlet是基于Java技术的Web组件，由容器管理并产生动态的内容。Servlet与客户端通过Servlet容器实现的请求/响应模型进行交互。


Servlet=Server+applet
Server：服务器
applet：小程序
Servlet含义是服务器端的小程序

在整个Web应用中，Servlet主要负责处理请求、协调调度功能。我们可以把Servlet称为Web应用中的**『控制器』**

## Servlet容器
### ①容器
在开发使用的各种技术中，经常会有很多对象会放在容器中。
### ②容器提供的功能
容器会管理内部对象的整个生命周期。对象在容器中才能够正常的工作，得到来自容器的全方位的支持。

- 创建对象
- 初始化
- 工作
- 清理
### ③容器本身也是对象

- 特点1：往往是非常大的对象
- 特点2：通常的单例的
### ④典型Servlet容器产品举例

- Tomcat
- jetty
- jboss
- Weblogic
- WebSphere
- glassfish
## serlet环境搭建

1. 在我们的项目中创建libs目录存放第三方的jar包
2. 项目中导入servlet-api.jar   libs目录中就在我们tomcat安装的目录 中 lib 目录中
3. 创建servlet包 专门存放就是我们的servlet
4. 创建IndexServlet 实现Servlet 重写方法
5. IndexServlet 类上加上@WebServlet（"/mayikt"）注解定义 URL访问的路径
6. 重写Servlet 类中service 在service中编写 动态资源
```java
package com.linmu.servlet;

import jakarta.servlet.*;
import jakarta.servlet.annotation.WebServlet;

import java.io.IOException;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/15 08:47:34
**/
@SuppressWarnings({"all"})
    @WebServlet("/servlet01")
    public class Servlet01 implements Servlet {

        @Override
        public void init(ServletConfig servletConfig) throws ServletException {

        }

        @Override
        public ServletConfig getServletConfig() {
            return null;
        }

        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

        }

        @Override
        public String getServletInfo() {
            return null;
        }

        @Override
        public void destroy() {

        }
    }

```

## servlet debug调试

养成习惯 学会通过 debug模式 运行项目---非常重要
f8---下一步
f9---跳到下一个断点
![image.png](https://cdn.nlark.com/yuque/0/2022/png/25438525/1650623567912-f74306dc-1f8d-43d2-9c3c-6af97fd6c533.png#averageHue=%23f6f4f2&clientId=u3a80ffad-0c9c-4&from=paste&height=175&id=u9d5321d5&name=image.png&originHeight=306&originWidth=695&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24319&status=done&style=none&taskId=ucd36e2f3-c25f-4b77-bb13-58e7c2e31d2&title=&width=397.14285714285717)

## servlet执行流程

1. servlet是谁创建的？我们的service是谁在调用？
   1. servlet是由我们的 web服务器（tomcat）创建、该方法是由我们的 web服务器（tomcat）调用
2. .tomcat服务器如何执行到servlet中的service方法？
   1. 这是因为我们创建的servlet实现httpservlet接口 重写了service方法

## servlet生命周期

1. 创建servlet  被 服务器（tomcat）加载servlet  
   1. 首次访问才会创建servlet创建好了以后 在jvm内存中只会存在一份，（单例模式） 线程安全问题
   2. 第二次访问servlet ，不需要创建
   3. 提前创建servlet ----优点可以 第一次访问的时候就不需要创建servlet 对象可以提高效率、但是 项目启动时提前创建servlet这样就会导致tomcat启动变得比较慢
```java
/*
loadOnStartup = 0或正数：启动时创建
loadOnStartup = 负数：提前创建创建
loadOnStartup的值越小，优先级越大
*/
@WebServlet(urlPatterns = "/mayiktmeite",loadOnStartup = 1)
```

2. .执行servlet类中init---初始化方法
   1. 当我们的servlet类被创建时，执行servlet类初始化方法init 代码初始化该方法只会执行一次。
3. 执行service方法-----执行每次请求
   1. 每次客户端发送请求达到服务器端 都会执行到 servlet类service方法
      1. 如果客户端发送GET请求，容器调用Servlet的doGet方法处理并响应请求
      2. 如果客户端发送POST请求，容器调用Servlet的doPost方法处理并响应请求
      3. 或者统一用service()方法处理来响应用户请求
4. 执行destroy方法---当我们tomcat容器停止时卸载servlet
   1. 存放销毁相关代码
## servlet方法介绍
init（）方法在Servlet实例化后，Servlet容器会调用init()方法来初始化该对象，主要是为了让Servlet对象在处理客户请求前可以完成一些初始化工作，例如：建立数据库的连接，获取配置信息等。对于每一个Servlet实例，init()方法只能被调用一次。init()方法有一个类型为ServletConfig的参数，Servlet容器通过这个参数向Servlet传递配置信息。Servlet使用ServletConfig对象从Web应用程序的配置信息中获取以名-值对形式提供的初始化参数。另外，在Servlet中，还可以通过ServletConfig对象获取描述Servlet运行环境的ServletContext对象，使用该对象，Servlet可以和它的Servlet容器进行通信。
service（）方法容器调用service()方法来处理客户端的请求。要注意的是，在service()方法被容器调用之前，必须确保init()方法正确完成。容器会构造一个表示客户端请求信息的请求对象（类型为ServletRequest）和一个用于对客户端进行响应的响应对象（类型为ServletResponse）作为参数传递给service()。在service()方法中，Servlet对象通过ServletRequest对象得到客户端的相关信息和请求信息，在对请求进行处理后，调用ServletResponse对象的方法设置响应信息。
destroy（）方法当容器检测到一个Servlet对象应该从服务中被移除的时候，容器会调用该对象的destroy()方法，以便让Servlet对象可以释放它所使用的资源，保存数据到持久存储设备中，例如将内存中的数据保存到数据库中，关闭数据库的连接等。当需要释放内存或者容器关闭时，容器就会调用Servlet对象的destroy()方法，在Servlet容器调用destroy()方法前，如果还有其他的线程正在service()方法中执行容器会等待这些线程执行完毕或者等待服务器设定的超时值到达。一旦Servlet对象的destroy()方法被调用，容器不回再把请求发送给该对象。如果需要改Servlet再次为客户端服务，容器将会重新产生一个Servlet对象来处理客户端的请求。在destroy()方法调用之后，容器会释放这个Servlet对象，在随后的时间内，该对象会被java的垃圾收集器所回收。
getServletInfo（）方法返回一个String类型的字符串，其中包括了关于Servlet的信息，例如，作者、版本和版权。该方法返回的应该是纯文本字符串，而不是任何类型的标记。
getServletConfig（）方法该方法返回容器调用init()方法时传递给Servlet对象的ServletConfig对象，ServletConfig对象包含了Servlet的初始化参数。
## servlet设置字符集
```java
response.setContentType("text/html;charset=UTF-8");
```
```java
response.setCharacterEncoding("UTF-8");
```
> 需要注意的问题
> response.getWriter()不能出现在设置字符集操作的前面（两种方式都不行）。

## servlet体系结构

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673746018416-b797a3e3-71e7-4b36-8ad8-74f03219ab9b.png#averageHue=%23f8f8f7&clientId=ua6f2fb82-1ec2-4&from=paste&height=259&id=u39bab470&name=image.png&originHeight=517&originWidth=1009&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27577&status=done&style=shadow&taskId=u2cb2a73c-d639-4489-ac2b-da9f2da8c8d&title=&width=504.5)

```java
package com.linmu.servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 09:29:11
 **/
@SuppressWarnings({"all"})
@WebServlet("/servlet02")
public class Servlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
    }
}

```

### request对象和response对象

- request对象：获取客户端发送数据给服务器端
- response对象：返回对应的数据给客户端

### request对象和response对象继承关系

- ServletRequest接口： java提供的请求对象根接口
- HttpServletRequest接口：继承ServletRequest，java提供的对http协议封装请求对象接口 
- org.apache.catalina.connnector.RequestFacade类：Tomcat编写的，实现HttpServletRequest 
##### ![](https://cdn.nlark.com/yuque/0/2022/png/25438525/1651722438561-7982898e-02c3-4690-8642-0efd968767b6.png#averageHue=%23f8f2e0&from=url&id=H7JyK&originHeight=691&originWidth=502&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)

### request获取请求数据

1. 请求行
| String getMethod()		 | 获取请求方式 |
| --- | --- |
| String getContextPath()		 | 获取项目访问路径 /mayikt-tomcat04 |
| StringBuffer getRequestURL() | StringBuffer getRequestURL() |
| String getRequestURI() | 获取 URI 统一资源标识符 |
| String getQueryString() | 获取请求参数(Get 方式) |

```java
package com.linmu.servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 09:29:11
 **/
@SuppressWarnings({"all"})
@WebServlet("/servlet02")
public class Servlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        doPost(req,resp);
    }
    
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        // http://localhost:8080/untitled9_war_exploded/servlet02?username=jackson&pwd=000000
        PrintWriter writer = resp.getWriter();
        // GET
        String method = req.getMethod();
        // /untitled9_war_exploded
        String contextPath = req.getContextPath();
        // http://localhost:8080/untitled9_war_exploded/servlet02
        StringBuffer requestURL = req.getRequestURL();
        // /untitled9_war_exploded/servlet02    
        String requestURI = req.getRequestURI();
        // username=jackson&pwd=000000
        String queryString = req.getQueryString();
        writer.println(method + "\n" + contextPath  + "\n" +
                requestURL  + "\n" + requestURI + "\n" +
                queryString);
        writer.close();
    }
}

```

2. 请求头
| String getHeader(String name) | 根据请求头名称, 获取值 |
| --- | --- |

```java
package com.linmu.servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 09:29:11
 **/
@SuppressWarnings({"all"})
@WebServlet("/servlet02")
public class Servlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        PrintWriter writer = resp.getWriter();
        // Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36 SLBrowser/8.0.0.12022 SLBChan/105
        String header = req.getHeader("User-Agent");
        writer.println(header);
        writer.close();
    }
}

```

3. 请求体（POST方法才有）

需要使用对应的方法读取数据

| ServletInputStream getInputStream() | 字节输入流 |
| --- | --- |
| BufferedReader getReader()	 | 字符输入流  readLine(); |

### request请求获取参数
| Map<String,String[]> getParameterMap() | 返回这个 Map 集合 |
| --- | --- |
| String[] getParameterValues(String name) | 根据 键名 获取值 |
| String getParameter(String name) | 根据 键名 获取值 |

### request请求转发
> 定义：一种在服务器内部的资源跳转方式
> 特点：浏览器地址栏路径不发生变化；只能转发到当前服务器内部资源中；转发是一次请求；

![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1673749892944-85a1c87f-e69d-405f-abf3-10ed207d7a3e.jpeg)

> 方法

| void setAttribute(key，value) | 设置数据共享 |
| --- | --- |
| RequestDispatcher getRequestDispatcher(String path) | 通过request对象获取请求转发器对象 |
| forward(request,response) | 传入请求和响应对象 |
| request.getAttribute() | 获取共享数据 |

```java
package com.linmu.servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 09:29:11
 **/
@SuppressWarnings({"all"})
@WebServlet("/servlet02")
public class Servlet02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 业务代码
        String name = req.getParameter("name");
        req.setAttribute("name",name);
        req.getRequestDispatcher("/servlet01").forward(req,resp);
    }
}

```

### response响应数据
> response是Servlet.service方法的一个参数，类型为javax.servlet.http.HttpServletResponse。在客户端发出每个请求时，服务器都会创建一个response对象，并传入给Servlet.service()方法。response对象是用来对客户端进行响应的，这说明在service()方法中使用response对象可以完成对客户端的响应工作。


方法

| setCharacterEncoding("UTF-8") | 设置服务端的编码 |
| --- | --- |
| setHeader("Content-type","text/html;utf-8") | 通过设置响应头设置客户端（浏览器的编码） |
| setContentType("text/html;charset=utf-8") | 这个方法可以同时设置客户端和服务端，因为它会调用setCharacterEncoding方法 |
| PrintWriter getWriter() | 获取字符流对象 |
| ServletOutputStream response.getOutputStream() | 获取字节流对象 |
| setError(404,"未找到请求的资源！"); | 设置错误的响应码 |
| setStatus(200); | 设置正确的响应码 |


### response重定向
> 作用：可以访问服务器外的资源，也可以是访问服务器内部的资源

1. 重定向之后，浏览器地址栏的URL会发生改变
2. 重定向过程中会将前面Request对象销毁，然后创建一个新的Request对象
3. 重定向的URL可以是其它项目工程

### 练习：JDBC+Servlet实现登录和注册
准备工作
1. 添加jar包
   1. commons-lang3-3.4----常用工具包
   2. mysql-connector-java-8.0.13.jar  ----mysql驱动包
   3. servlet-api.jar  ----servlet-api.jar 
   4. 依赖jar包：[http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7](http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7)
```sql
CREATE TABLE USERS (
  USERID INT NOT NULL AUTO_INCREMENT,
  USERNAME VARCHAR(225) DEFAULT NULL,
  USERPWD VARCHAR(224) DEFAULT NULL,
  PRIMARY KEY(USERID)
)

```
```sql
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/javawebreview?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
username=root
password=000000
initialSize=5
maxActive=10
maxWait=3000
```
```java
package com.linmu.javaweb01.JdbcUtils;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/17 16:12:26
 **/
@SuppressWarnings({"all"})
public class Jdbcu {

    /**
     * 获取连接
     * @return
     */
    public static Connection getConnection(){
        Properties properties = null;
        Connection connectoin = null;
        try {
            InputStream resourceAsStream = Jdbcu.class.getClassLoader().getResourceAsStream("druid.properties");
            properties = new Properties();
            properties.load(resourceAsStream);
            DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
            connectoin = dataSource.getConnection();
        } catch(Exception e){
            e.printStackTrace();
            System.out.println("get connection error...");
        }
        return connectoin;
    }

    /**
     * 关闭连接
     * @param resultSet
     * @param preparedStatement
     * @param connection
     * @return
     */
    public static boolean closeConnection(ResultSet resultSet, PreparedStatement preparedStatement, Connection connection){
        if (resultSet != null && preparedStatement != null && connection != null) {
            try {
                resultSet.close();
                preparedStatement.close();
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
                System.out.println("close connection error...");
                return false;
            }
        }
        return true;
    }

    /**
     * 关闭连接
     * @param preparedStatement
     * @param connection
     * @return
     */
    public static boolean closeConnection(PreparedStatement preparedStatement, Connection connection){
        if (preparedStatement != null && connection != null) {
            try {
                preparedStatement.close();
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
                System.out.println("close connection error...");
                return false;
            }
        }
        return true;
    }

    /**
     * 关闭连接
     * @param connection
     * @return
     */
    public static boolean closeConnection(Connection connection){
        if (connection != null) {
            try {
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
                System.out.println("close connection error...");
                return false;
            }
        }
        return true;
    }
}

```
```java
package com.linmu.javaweb01.servlet;

import com.linmu.javaweb01.JdbcUtils.Jdbcu;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.PreparedStatement;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/17 16:43:22
 **/
@SuppressWarnings({"all"})
@WebServlet(urlPatterns = "/register")
public class Register extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        resp.setContentType("text/html;charset=utf-8");
        String name = req.getParameter("name");
        String password = req.getParameter("password");
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            connection = Jdbcu.getConnection();
            connection.setAutoCommit(false);
            String sql = "insert into users values(null,?,?)";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, password);
            int influenceRows = preparedStatement.executeUpdate();
            String result = influenceRows > 0 ? "register successfully..." : "register failure...";
            writer.println(result);
            connection.commit();
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("error...");
            try{
                connection.rollback();
            } catch(Exception ex){
                ex.printStackTrace();
                writer.println("rollback error...");
            }
        } finally {
            Jdbcu.closeConnection(preparedStatement,connection);
            writer.close();
        }
    }
}

```
```java
package com.linmu.javaweb01.servlet;

import com.linmu.javaweb01.JdbcUtils.Jdbcu;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/17 18:43:01
 **/
@SuppressWarnings({"all"})
@WebServlet(urlPatterns = "/login")
public class login extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        String name = req.getParameter("name");
        String password = req.getParameter("password");
        Connection connection = null;
        ResultSet resultSet = null;
        PreparedStatement preparedStatement = null;
        try {
            connection = Jdbcu.getConnection();
            String sql = "select * from users where username=? and password=?";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, password);
            resultSet = preparedStatement.executeQuery();
            String reult = resultSet.next() ? "login successfully..." : "user isn't exesits...";
            writer.println(reult);
        } catch (Exception e) {
            e.printStackTrace();
            writer.println("error");
        } finally {
            Jdbcu.closeConnection(resultSet, preparedStatement, connection);
            writer.close();
        }
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
    <a href="login.html">login</a>
    <a href="register.html">register</a>
  </body>
</html>
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <form action="/javaweb01_war_exploded/login" method="post">
      <p>
        <label for="name">name</label>
        <input type="text" name="name" Id="name">
      </p>
      <p>
        <label for="password">password</label>
        <input type="password" name="password" Id="password">
      </p>
      <p>
        <input type="submit" value="submit">
      </p>
    </form>
  </body>
</html>
```
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <form action="/javaweb01_war_exploded/register" method="post">
      <p>
        <label for="name">name</label>
        <input type="text" name="name" Id="name">
      </p>
      <p>
        <label for="password">password</label>
        <input type="password" name="password" Id="password">
      </p>
      <p>
        <input type="submit" value="register">
      </p>
    </form>
  </body>
</html>
```

## Jsp（已经淘汰，了解即可）
### jsp概述

1. 什么是JSP：Java Server Pages  java服务器端页面；JSP全称Java Server Pages，是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。
2. 程序员在开发过程中，发现Servlet做界面非常不方便，所以产生了jsp技术，JSP其实是对Servlet进行了包装而已。
3. 为什么会出现JSP（Java Server Pages）技术：jsp + java类（service、javabean）+ servlet，就会构成mvc的开发模式，mvc模式是目前软件公司中相当通用的开发模式。

- jsp底层是基于Servlet封装。
- mvc架构模式

![](https://cdn.nlark.com/yuque/0/2022/png/25438525/1653965917692-9b9dbf30-8ffa-4d1b-b9ce-413ec730c250.png#averageHue=%23f9f8f2&from=url&id=aL9ms&originHeight=781&originWidth=805&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)
![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1674002295984-bd720ae0-a0bb-4017-85e9-c7d3a659b050.jpeg)

- jsp 可以编写java代码
- jsp转化成 servletindex.jsp----底层变成 index.java(servlet)-----class index.class

### jsp特点

1. JSP的全称是Java Server Pages，它和Servlet技术一样，都是SUN公司定义的一种用于开发动态web资源的技术。
2. jsp这门技术的最大的特点在于，写jsp就像在写HTML，但：它相对于html而言，html只能为用户提供静态数据，而jsp技术允许在页面中嵌套java代码，为用户提供动态数据。
3. 相比Servlet而言，Servlet很难对数据进行排版，而jsp除了可以用java代码产生动态数据的同时，也很容易对数据进行排版。

### jsp脚本
jsp底层 基于servlet实现
依赖jar包：[http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7](http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7)
在jsp中定义java代码
```xml
<dependency>
  <groupId>org.glassfish.web</groupId>
  <artifactId>jakarta.servlet.jsp.jstl</artifactId>
  <version>2.0.0</version>
</dependency>
```

1. <% ... %>∶内容会直接放到_jspService()方法之中
2. <%=..%>∶内容会放到out.print()中，作为out.print()的参数
3. <%!...%>:内容会放到_jspService()方法之外，被类直接包含
```java
<%--
  Created by IntelliJ IDEA.
  User: 2574550346
  Date: 2023/1/13
  Time: 14:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <%
    System.out.println("begin learn jsp...");
    method();
  %>
  <%=
    "直接输出..."
  %>
  <%!
    public void method(){
      System.out.println("the method is defined out of servlet_service method...");
    }
  %>
  </body>
</html>

```

### el表达式
Expression Language表达式语言，用于简化JSP页面内的Java代码
主要功能：获取数据
语法：${expression}
例：${users}获取域中存储的key为users的数据 

1. page:当前页面有效
2. request:当前请求有效
3. session:当前会话有效
4. application:当前应用有效

### jstl标签
apache对EL表达式的扩展（也就是说JSTL依赖EL），JSTL是标签语言！JSTL标签使用以来非常方便，它与JSP动作标签一样，只不过它不是JSP内置的标签，需要我们自己导包，以及指定标签库而已
jsp 中定义c标签（需要导入）：<%@ taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
if标签的test属性必须是一个boolean类型的值，如果test的值为true，那么执行if标签的内容，否则不执行。
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<c:if test="${age<12}">
  <h1>under 12</h1>
</c:if>
<c:if test="${age<18}">
  <h1>under 18</h1>
</c:if>
</body>
</html>
```
| [<c:out>](https://www.runoob.com/jsp/jstl-core-out-tag.html) | 用于在JSP中显示数据，就像<%= ... > |
| --- | --- |
| [<c:set>](https://www.runoob.com/jsp/jstl-core-set-tag.html) | 用于保存数据 |
| [<c:remove>](https://www.runoob.com/jsp/jstl-core-remove-tag.html) | 用于删除数据 |
| [<c:catch>](https://www.runoob.com/jsp/jstl-core-catch-tag.html) | 用来处理产生错误的异常状况，并且将错误信息储存起来 |
| [<c:if>](https://www.runoob.com/jsp/jstl-core-if-tag.html) | 与我们在一般程序中用的if一样 |
| [<c:choose>](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | 本身只当做<c:when>和<c:otherwise>的父标签 |
| [<c:when>](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | <c:choose>的子标签，用来判断条件是否成立 |
| [<c:otherwise>](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | <c:choose>的子标签，接在<c:when>标签后，当<c:when>标签判断为false时被执行 |
| [<c:import>](https://www.runoob.com/jsp/jstl-core-import-tag.html) | 检索一个绝对或相对 URL，然后将其内容暴露给页面 |
| [<c:forEach>](https://www.runoob.com/jsp/jstl-core-foreach-tag.html) | 基础迭代标签，接受多种集合类型 |
| [<c:forTokens>](https://www.runoob.com/jsp/jstl-core-foreach-tag.html) | 根据指定的分隔符来分隔内容并迭代输出 |
| [<c:param>](https://www.runoob.com/jsp/jstl-core-param-tag.html) | 用来给包含或重定向的页面传递参数 |
| [<c:redirect>](https://www.runoob.com/jsp/jstl-core-redirect-tag.html) | 重定向至一个新的URL. |
| [<c:url>](https://www.runoob.com/jsp/jstl-core-url-tag.html) | 使用可选的查询参数来创造一个URL |

[<c:forEach>](https://www.runoob.com/jsp/jstl-core-foreach-tag.html)：基础迭代标签，接受多种集合类型
```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<table border="1" align="center" style="border-collapse: collapse;width: 30%">
  <tr align="center">
    <th align="center">序号</th>
    <th align="center">名称</th>
    <th align="center">年龄</th>
    <th align="center">冻结</th>
    <th align="center">操作</th>
  </tr>
  <c:forEach items="${userlist}" var="user">
    <tr align="center">
      <td align="center">0</td>
      <td align="center">${user.name}</td>
      <td align="center">${user.age}</td>
      <td align="center"><c:if test="${user.status==1}">冻结</c:if><c:if test="${user.status==0}">未冻结</c:if></td>
      <td align="center"><a href="#">修改</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="#">删除</a></td>
    </tr>
  </c:forEach>

</table>
</body>
</html>
```
### jsp+servlet+jdbc开发航班系统
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674003071148-e2023643-9ea8-44a1-b084-2eeab3b447a3.png#averageHue=%23f7f7f5&clientId=uf6d031fb-3845-4&from=paste&height=593&id=u174b36af&name=image.png&originHeight=1185&originWidth=2475&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1509380&status=done&style=shadow&taskId=uf754c3b7-cd6d-406c-bf3d-45104665d8f&title=&width=1237.5)

# Thymeleaf
> [https://www.thymeleaf.org/](https://www.thymeleaf.org/)

## 从三层结构到MVC
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse01.html#_1mvc%E6%A6%82%E5%BF%B5)①MVC概念
M：Model模型
V：View视图
C：Controller控制器
MVC是在表述层开发中运用的一种设计理念。主张把**封装数据的『模型』**、**显示用户界面的『视图』**、**协调调度的『控制器』**分开。
好处：

- 进一步实现各个组件之间的解耦
- 让各个组件可以单独维护
- 将视图分离出来以后，我们后端工程师和前端工程师的对接更方便
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse01.html#_2mvc%E5%92%8C%E4%B8%89%E5%B1%82%E6%9E%B6%E6%9E%84%E4%B9%8B%E9%97%B4%E5%85%B3%E7%B3%BB)②MVC和三层架构之间关系
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675414933039-bf81838c-b22b-41a4-bdcc-b53a6c6233d7.png#averageHue=%23f4f4f4&clientId=ua4d1f647-afee-4&from=paste&id=u17f2ba23&originHeight=344&originWidth=662&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u6335cdc5-b4f1-495d-8b3a-1dc761ede1c&title=)
## Thymeleaf概述
Thymeleaf：是一个适用于Web和独立环境的现代化服务器端Java模板引擎（用于取代JSP）
:::info
特点：
前端页面通过占位符来获取后端的数据
使用Thymeleaf就能够使得当前Web应用程序的前后端划分更加清晰
:::
### Thymeleaf的同行
JSP、Freemarker、Velocity等等，它们有一个共同的名字：**服务器端模板技术**

### Thymeleaf优势

- SpringBoot官方推荐使用的视图模板技术，和SpringBoot完美整合。
- 不经过服务器运算仍然可以直接查看原始值，对前端工程师更友好。



```html
<table>
  <thead>
    <tr>
      <th th:text="#{msgs.headers.name}">Name</th>
      <th th:text="#{msgs.headers.price}">Price</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="prod: ${allProducts}">
      <td th:text="${prod.name}">Oranges</td>
      <td th:text="${#numbers.formatDecimal(prod.price, 1, 2)}">0.99</td>
    </tr>
  </tbody>
</table>
```
## 环境搭建
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674124074357-1ae4aa78-ee3c-4f1f-bd3f-9ac6e777bafd.png#averageHue=%23252b33&clientId=u7cf6bd3a-af27-4&from=paste&height=594&id=u91d3c113&name=image.png&originHeight=1187&originWidth=1574&originalType=binary&ratio=1&rotation=0&showTitle=false&size=169414&status=done&style=shadow&taskId=u850f1370-de40-4ef5-8fe3-3f01c6478ab&title=&width=787)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674124165600-0aef9199-e936-43e3-b872-bc3138162906.png#averageHue=%23232830&clientId=u7cf6bd3a-af27-4&from=paste&height=594&id=uf66b083b&name=image.png&originHeight=1187&originWidth=1574&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104458&status=done&style=shadow&taskId=uf07258cd-346c-40bf-a5f1-64b2a4cdf99&title=&width=787)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674124337106-c5f6bb17-e145-44fe-8089-e12d000e143b.png#averageHue=%23282d35&clientId=u7cf6bd3a-af27-4&from=paste&height=852&id=ua18bc426&name=image.png&originHeight=1704&originWidth=2880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=466218&status=done&style=shadow&taskId=u9fb27132-1b49-45d7-aba1-0d4258f42f9&title=&width=1440)
```xml
<dependency>
  <groupId>jakarta.servlet</groupId>
  <artifactId>jakarta.servlet-api</artifactId>
  <version>5.0.0</version>
  <scope>provided</scope>
</dependency>
```
## Thymeleaf基础语法
> 工具类

1. 首先我们看看后端部分，我们需要通过TemplateEngine对象来将模板文件渲染为最终的HTML页面：
由于此对象只需要创建一次，之后就可以一直使用了。
```java
package com.example.utils;

import org.thymeleaf.TemplateEngine;
import org.thymeleaf.templateresolver.ClassLoaderTemplateResolver;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/19 18:53:35
**/
@SuppressWarnings({"all"})
public class ThymeleafUtils {
    private static TemplateEngine engine = null;

    public static TemplateEngine getEngine(){
        engine = new TemplateEngine();
        ClassLoaderTemplateResolver classLoaderTemplateResolver = new ClassLoaderTemplateResolver();
        engine.setTemplateResolver(classLoaderTemplateResolver);
        return engine;
    }
}

```
入门小案例
```java
package com.example.themlef;


import com.example.utils.ThymeleafUtils;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.thymeleaf.context.Context;

import java.io.IOException;

/**
* @author Jackson Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/1/19 18:42:27
**/
@SuppressWarnings({"all"})
@WebServlet("/thymeleaf01")
public class Thymeleaf01 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Context context = new Context();
        context.setVariable("title","jackson");
        ThymeleafUtils.getEngine().process("index.html",context,resp.getWriter());
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
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
    <div th:text="${title}"></div>
  </body>
</html>
```
语法：
我们使用了th:text来为当前标签指定内部文本，注意任何内容都会变成普通文本，即使传入了一个HTML代码，如果我希望向内部添加一个HTML文本呢？我们可以使用th:utext属性：
<div th:text="${参数}" > </div>

```html
<div th:text="${title.toUpperCase()}"></div>
<img width="300px" height="300px" th:src="${src}" th:alt="${alt}">
```

```html
<div th:text="${number*2}"></div>
```

```html
<div th:text="${number1 > 10 ? 'A' :'B'}"></div>
```

```html
<p th:text="${alt}+'+javsk'+${number}"></p>
```

## Thymeleaf流程控制语法

```html
<div th:if="${age}">if statement</div>
```

```html
<div th:switch="${score}">
    <div th:case="A">A</div>
    <div th:case="B">B</div>
    <div th:case="C">C</div>
    <div th:case="*">D</div>
</div>
```

```html
<ul>
    <li th:each="name:${items}" th:text="'name='+${name}"></li>
</ul>
```
```html
<ul>
    <li th:each="name,iterStat:${items}" th:text="'index='+${iterStat.index}+',name='+${name}"></li>
</ul>
```
我们还可以获取当前循环的迭代状态，只需要在最后添加`iterStat`即可，从中可以获取很多信息，比如当前的顺序
状态变量在`th:each`属性中定义，并包含以下数据：

- 当前_迭代索引_，以0开头。这是`index`属性。
- 当前_迭代索引_，以1开头。这是`count`属性。
- 迭代变量中的元素总量。这是`size`属性。
- 每个迭代的_迭代变量_。这是`current`属性。
- 当前迭代是偶数还是奇数。这些是`even/odd`布尔属性。
- 当前迭代是否是第一个迭代。这是`first`布尔属性。
- 当前迭代是否是最后一个迭代。这是`last`布尔属性。

## 解析URL地址
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse05.html#_1%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95)①基本语法
代码示例：
```html
<p th:text="@{/aaa/bbb/ccc}">标签体原始值</p>
```
经过解析后得到：
/view/aaa/bbb/ccc
所以@{}的作用是**在字符串前附加『上下文路径』**
这个语法的好处是：实际开发过程中，项目在不同环境部署时，Web应用的名字有可能发生变化。所以上下文路径不能写死。而通过@{}动态获取上下文路径后，不管怎么变都不怕啦！
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse05.html#_2%E9%A6%96%E9%A1%B5%E4%BD%BF%E7%94%A8url%E5%9C%B0%E5%9D%80%E8%A7%A3%E6%9E%90)②首页使用URL地址解析
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675415352062-de84aa8b-67de-4f0c-b855-0c85471e6b3e.png#averageHue=%23f5f5f5&clientId=ua4d1f647-afee-4&from=paste&id=u7b83a3d6&originHeight=268&originWidth=737&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=uad6f90d7-fd35-4f37-8515-3f6dd14107c&title=)
如果我们直接访问index.html本身，那么index.html是不需要通过Servlet，当然也不经过模板引擎，所以index.html上的Thymeleaf的任何表达式都不会被解析。
解决办法：通过Servlet访问index.html，这样就可以让模板引擎渲染页面了：
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675415351719-96a6eb3f-28cd-4a95-9a25-c5c9ccf6e9a9.png#averageHue=%23f3f3f3&clientId=ua4d1f647-afee-4&from=paste&id=u8dca1578&originHeight=268&originWidth=737&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=ub94582f4-072a-4835-9a95-a930a2d670d&title=)
进一步的好处：
通过上面的例子我们看到，所有和业务功能相关的请求都能够确保它们通过Servlet来处理，这样就方便我们统一对这些请求进行特定规则的限定。
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse05.html#_3%E7%BB%99url%E5%9C%B0%E5%9D%80%E5%90%8E%E9%9D%A2%E9%99%84%E5%8A%A0%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0)③给URL地址后面附加请求参数
参照官方文档说明：
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675415352113-857964e2-618a-4338-9090-524cbc9188fa.png#averageHue=%23f5f2f0&clientId=ua4d1f647-afee-4&from=paste&id=u4b552fd6&originHeight=99&originWidth=850&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u771a8fb5-78d0-452f-acca-21372307f37&title=)
## Thymeleaf模板布局
在某些网页中，我们会发现，整个网站的页面，除了中间部分的内容会随着我们的页面跳转而变化外，有些部分是一直保持一个状态的，比如打开小破站，我们翻动评论或是切换视频分P的时候，变化的仅仅是对应区域的内容，实际上，其他地方的内容会无论内部页面如何跳转，都不会改变。

Thymeleaf就可以轻松实现这样的操作，我们只需要将不会改变的地方设定为模板布局，并在不同的页面中插入这些模板布局，
```html
<div th:fragment="head">
  <div>this is title...</div>
</div>
```
```html
<div th:include="index1.html::head"></div>
```

个性模板
```html
<div class="head" th:fragment="head-title(sub)">
    <div>
        <h1>我是标题内容，每个页面都有</h1>
        <h2 th:text="${sub}"></h2>
    </div>
    <hr>
</div>
```
```html
<div th:include="head.html::head-title('这个是第1个页面的二级标题')"></div>
<div class="body">
    <ul>
        <li th:each="title, iterStat : ${list}" th:text="${iterStat.index}+'.《'+${title}+'》'"></li>
    </ul>
</div>
```

### 包含其他模板文件
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse11.html#_1%E3%80%81%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)1、应用场景
抽取各个页面的公共部分：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675415526974-8a447c6e-38d6-4717-81c5-bd896f3843b6.png#averageHue=%23f7f7f7&clientId=ua4d1f647-afee-4&from=paste&id=u4ca895f6&name=image.png&originHeight=395&originWidth=451&originalType=url&ratio=1&rotation=0&showTitle=false&size=7068&status=done&style=shadow&taskId=u208f97c8-5819-4517-87ee-51fbc5dbf22&title=)
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse11.html#_2%E3%80%81%E5%88%9B%E5%BB%BA%E9%A1%B5%E9%9D%A2%E7%9A%84%E4%BB%A3%E7%A0%81%E7%89%87%E6%AE%B5)2、创建页面的代码片段
使用th:fragment来给这个片段命名：
```html
<div th:fragment="header">
    <p>被抽取出来的头部内容</p>
</div>
```
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter08/verse11.html#_3%E3%80%81%E5%8C%85%E5%90%AB%E5%88%B0%E6%9C%89%E9%9C%80%E8%A6%81%E7%9A%84%E9%A1%B5%E9%9D%A2)3、包含到有需要的页面
| 语法 | 效果 |
| --- | --- |
| th:insert | 把目标的代码片段整个插入到当前标签内部 |
| th:replace | 用目标的代码替换当前标签 |
| th:include | 把目标的代码片段去除最外层标签，然后再插入到当前标签内部 |

页面代码举例：
```html
<!-- 代码片段所在页面的逻辑视图 :: 代码片段的名称 -->
<div id="badBoy" th:insert="segment :: header">
    div标签的原始内容
</div>

<div id="worseBoy" th:replace="segment :: header">
    div标签的原始内容
</div>

<div id="worstBoy" th:include="segment :: header">
    div标签的原始内容
</div>
```
