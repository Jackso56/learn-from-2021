# 会话技术

产生原因
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675415737076-193e2ba6-d917-4256-a2de-fb01d3b0bda9.png#averageHue=%23f0f0f0&clientId=uf9f05580-74c9-4&from=paste&id=u9154def5&originHeight=363&originWidth=439&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u5c21c1d5-e0ee-4176-951e-c29df45834d&title=)
保持用户登录状态，背后的底层逻辑是：服务器在接收到用户请求的时候，有办法判断这个请求来自于之前的某一个用户。所以保持登录状态，本质上是保持** 会话状态**

1. 用户打开同一个浏览器，访问到我们的web服务器的资源建立会话，对方断开连接该会话才会结束，在一次会话中可以包含多次请求和响应。通俗易懂的理解：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于用一个浏览器，以便于在同一次会话的多次请求间共享数据。
2. 这是因为http协议是无状态的，每次浏览器向服务器请求时，没有绑定会话信息，服务器都会认为该请求是为新请求，没有任何记忆功能，所以我们需要会话跟踪技术实现会话内数据共享。

会话技术实现方式（数据共享）

1. 客户端会话跟踪技术：Cookie （相关信息存放在客户端）     
2. 服务端会话跟踪技术：Session（相关信息存放在服务器端）
3. token或者jwt----新的
## cookie
> Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问


### 创建cookie

1. 创建Cookie对象：Cookie cookie = new Cookie("key","value");
2. 使用response响应Cookie给客户端（浏览器）：response.addCookie(cookie);
```java
package com.linmu.cookie;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 18:38:03
 **/
@SuppressWarnings({"all"})
@WebServlet("/cookie01")
public class Cookie01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter("name");
        String password = req.getParameter("password");
        resp.addCookie(new Cookie("name",name));
        resp.addCookie(new Cookie("password",password));
    }
}

```

### 获取cookie
> 获取客户端携带的所有Cookie，使用request对象

Cookie[] cookies = request.getCookies()
cookies.getName()
cookies.getValue()
```java
package com.linmu.cookie;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 18:51:09
 **/
@SuppressWarnings({"all"})
@WebServlet("/cookie02")
public class Cookie02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        Cookie[] cookies = req.getCookies();
        for (int i = 0; i < cookies.length; i++) {
            writer.println(cookies[i].getName() + "=" + cookies[i].getValue());
        }
    }
}

```

### cookie原理
Cookie实现是基于HTTP协议的

1. 响应头：set—cookie

客户端（浏览器端）发送请求达到服务器端，服务器端会创建cookie，会将该cookie数据
返回给客户端，在响应头中设置  set-cookie：value cookie数据。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673782058940-4c32d6ed-3135-46bb-819b-ec14225b4439.png#averageHue=%23d5dff1&clientId=u96270324-22e4-4&from=paste&height=166&id=uf9426f28&name=image.png&originHeight=332&originWidth=760&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33967&status=done&style=shadow&taskId=u7950e67b-8406-4d52-ba21-1115e308b7b&title=&width=380)

2. 请求头：cookie

同一个浏览器发送请求时，在请求中设置该cookie数据 存放在请求头中，cookie：value
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673782006594-ce8fa8f7-cb6e-47d0-a365-2c83efeed685.png#averageHue=%23fefefe&clientId=u96270324-22e4-4&from=paste&height=230&id=u87bfe89a&name=image.png&originHeight=460&originWidth=1168&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71739&status=done&style=shadow&taskId=uca1fe4ed-7fe9-40b0-ad48-99dc34ab1f6&title=&width=584)

### cookie过期时间
#### Cookie时效性

- 会话级Cookie
   - 服务器端并没有明确指定Cookie的存在时间
   - 在浏览器端，Cookie数据存在于内存中
   - 只要浏览器还开着，Cookie数据就一直都在
   - 浏览器关闭，内存中的Cookie数据就会被释放
- 持久化Cookie
   - 服务器端明确设置了Cookie的存在时间
   - 在浏览器端，Cookie数据会被保存到硬盘上
   - Cookie在硬盘上存在的时间根据服务器端限定的时间来管控，不受浏览器关闭的影响
   - 持久化Cookie到达了预设的时间会被释放

setMaxAge(int seconds):设置Cookie存活时间 

1. 正数：将Cookie写入浏览器所在的电脑硬盘，持久化存储，到期自动删除 
2. 负数：默认值，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁。
3. 零：删除对应Cookie

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673782344637-89a6e4a7-9af8-40ef-af8c-dd778974f407.png#averageHue=%23fefdfd&clientId=u96270324-22e4-4&from=paste&height=495&id=ue7d3a559&name=image.png&originHeight=989&originWidth=899&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85923&status=done&style=shadow&taskId=u5b3bcee6-aa1a-4ea4-a4b8-64088ff0928&title=&width=449.5)
```java
package com.linmu.cookie;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/15 19:33:18
 **/
@SuppressWarnings({"all"})
@WebServlet("/cookie03")
public class cookie03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie name = new Cookie("name", "jackson");
        Cookie password = new Cookie("password", "000000");
        name.setMaxAge(60);
        password.setMaxAge(60);
        resp.addCookie(name);
        resp.addCookie(password);
    }
}

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673783032306-fba4ba02-f4fc-4a49-89f3-6990547f5cb4.png#averageHue=%23fefdfc&clientId=u96270324-22e4-4&from=paste&height=498&id=u8179426d&name=image.png&originHeight=996&originWidth=895&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88946&status=done&style=shadow&taskId=u3e405e79-fd32-4aa3-8bb4-add7367b40f&title=&width=447.5)






## session
### session常见API的使用

1. 服务器端会话跟踪技术：将数据保存在服务器端，底层基于cookie实现封装的
2. 常用的API： 
| void session.setAttribute(k,v) | 增加session |
| --- | --- |
| Object session.getAttribute(k)  | 获取到session中的值 |
| void removeAttribute(k)  | 删除session |

```java
package com.linmu.cookie;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author Jackson Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/1/16 08:56:18
 **/
@SuppressWarnings({"all"})
@WebServlet("/sessionDemo")
public class SessionDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        HttpSession session = req.getSession();
        session.setAttribute("name","jackson");
        session.setAttribute("password","000000");
        String name = (String)session.getAttribute("name");
        String password = (String)session.getAttribute("password");
        writer.println(name);
        writer.println(password);
        writer.close();
    }
}
```

### session原理

1. 当我们客户端发送请求达到服务器端时创建session，会得到一个sessionid，在将该sessionid  响应在响应头<sessionid  > 
2. 客户端（浏览器）接受响应头中的sessionid  ，会将该sessionid的值 存放在浏览器中。session本质上就是基于cookie实现封装的。
3. 使用同一个浏览器发送请求时，访问通一个服务器端，会在请求头中设定该sessionid  的值，服务器端就会从请求头中获取到该sessionid  查找对应session。
> cookie与session的区别：session 数据存放在服务器端 cookie将数据存放在本地。

![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1673831956154-4bf4c196-c807-4283-a992-453adeba8363.jpeg)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1673833115812-ef86d7a2-002a-43ac-a5f1-3b3d1c1a3c11.png#averageHue=%23fefefe&clientId=ue4908878-20b6-4&from=paste&height=271&id=ub891d92a&name=image.png&originHeight=542&originWidth=880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70599&status=done&style=shadow&taskId=u6eca03e2-c37c-44c3-83a2-587f27d4cdd&title=&width=440)

### session使用细节

1. 当客户端关闭后，服务器不关闭的话，获取到的session是否是同一个？
   1. 在默认情况下，不是同一个。因为客户端关闭后，cookie对象被销毁，客户端请求服务器会创建新的session。
   2. 如果需要两个Session相同，则可以创建一个Cookie对象，key为：JSESSIONID，设置一下最大存活时间，让Cookie持久化保存Session的ID，就可以实现客户端关闭，两次获取Session就是同一个。
```
Cookie c = new Cookie("JSESSIONID",session.getId());
	    c.setMaxAge(60*60); //1个小时有效期
	    response.addCookie(c);
```

2. 客户端不关闭，服务器关闭后的话，两次获取的Session是同一个吗？
   1. 不是同一个，但是为了确保数据不丢失，因为同样服务器关闭后session对象会被销毁 ，如果想确保数据不丢失，可以使session钝化，即在服务器正常关闭之前，将session对象序列化到硬盘上。下次在服务器启动后，将session文件反序列化转化为内存中的session对象即可。
   2. tomcat自动完成以下工作：
      1. session的钝化：在服务器正常关闭之前，将session对象系列化到硬盘上。
      2. session的活化： 在服务器启动后，将session文件转化为内存中的session对象即可。
3. session什么时候被销毁？
   1. 服务器关闭；
   2. session对象调用invalidate() ；
   3. session默认失效时间到期；

###  session与cookie区别

1. session特点：
   1. session用于存储一次会话的多次请求的数据，存在服务器端；
   2. session可以存储任意类型，任意大小的数据。
2. cookie特点：
   1. cookie用于存储一次会话的多次请求需共享的数据，存在客户端；
3. 区别：
   1. session存储数据在服务器端，Cookie在客户端；
   2. session没有数据大小限制，Cookie有数据大小限制；
   3. session数据安全，Cookie相对于不安全。

## 用户登录注册案例

# 过滤器（filter）
## 过滤器的三要素
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter10/verse01.html#_1%E6%8B%A6%E6%88%AA)①拦截
过滤器之所以能够对请求进行预处理，关键是对请求进行拦截，把请求拦截下来才能够做后续的操作。而且对于一个具体的过滤器，它必须明确它要拦截的请求，而不是所有请求都拦截。
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter10/verse01.html#_2%E8%BF%87%E6%BB%A4)②过滤
根据业务功能实际的需求，看看在把请求拦截到之后，需要做什么检查或什么操作，写对应的代码即可。
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter10/verse01.html#_3%E6%94%BE%E8%A1%8C)③放行
过滤器完成自己的任务或者是检测到当前请求符合过滤规则，那么可以将请求放行。所谓放行，就是让请求继续去访问它原本要访问的资源。
友情提示：将来学习SpringMVC时，会学习SpringMVC中的『拦截器』，同样具备三要素。
## 过滤器应用场景
过滤器是处于客户端与服务器资源文件之间的一道过滤网，在访问资源文件之前，通过一系列的过滤器对请求进行修改、判断等，把不符合规则的请求在中途拦截或修改。也可以对响应进行过滤，拦截或修改响应.
应用场景： 判断用户是否登录、过滤器请求记录日志、身份验证、权限控制等。
什么是过滤器  拦截过滤请求
过滤器可以 减少代码冗余性问题

## 过滤器配置与使用

1. 所有资源拦截：@WebFilter("/*") ===>这是指访问所有资源的时候都会经过过滤器
2. 具体资源路径拦截：@WebFilter("/index.jsp") ===>这是指访问index.jsp的时候会经过过滤器
3. 具体目录拦截：@WebFilter("/mayik/*")  ===>这是指访问mayikt目录下的所有资源时会经过过滤器
4. 具体后缀名拦截：@WebFilter("*.jsp")  ===>这时指访问后缀名为.jsp的资源时会经过过滤器

## 过滤器链
> 注解配置的Filter，优先级按照过滤器类名(字符串)的自然排序

![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1673833926090-463c67fd-8b48-4a50-95cf-745a0aa78bda.jpeg)

# 监听器（Listener）
## 观察者模式
二十三种设计模式之一：
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416117497-58e08125-f3d9-4f56-afd2-e9e6548ee146.png#averageHue=%23e1e1e1&clientId=uf9f05580-74c9-4&from=paste&id=u77355ec6&originHeight=297&originWidth=459&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u6711fcb0-c1de-4e6e-b242-469167072f4&title=)

- 观察者：监控『被观察者』的行为，一旦发现『被观察者』触发了事件，就会调用事先准备好的方法执行操作。
- 被观察者：『被观察者』一旦触发了被监控的事件，就会被『观察者』发现。

### 概念
监听器：专门用于对其他对象身上发生的事件或状态改变进行监听和相应处理的对象，当被监视的对象发生情况时，立即采取相应的行动。 **Servlet监听器**：Servlet规范中定义的一种特殊类，它用于监听Web应用程序中的ServletContext，HttpSession 和HttpServletRequest等域对象的创建与销毁事件，以及监听这些域对象中的属性发生修改的事件。

### 分类
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416293453-3cf87b8e-9b4f-44d2-bcc5-a9c4db2963fb.png#averageHue=%23f2f2f2&clientId=uf9f05580-74c9-4&from=paste&id=u0d73e999&originHeight=442&originWidth=471&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u42f3e91c-bcd3-4ff1-a3d0-04cad26676f&title=)

- 域对象监听器
- 域对象的属性域监听器
- Session域中数据的监听器

1. **监听器Listener就是在application,session,request三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件。**
2.  Listener是Servlet的监听器，可以监听客户端的请求，服务端的操作等。
3.  Listener实现了javax.servlet.ServletContextListener 接口的服务器端程序，它也是随web应用的启动而启动，只初始化一次，随web应用的停止而销毁。主要作用是：做一些初始化的内容添加工作、设置一些基本的内容、比如一些参数或者是一些固定的对象等.

1. ServletContext监听

ServletContextListener：用于对Servlet整个上下文进行监听；
ServletContextAttributeListener：对Servlet上下文属性的监听。

2. Session监听

HttpSessionListener接口：对Session的整体状态的监听；
HttpSessionAttributeListener接口：对session的属性监听。

3. Request监听

ServletRequestListener：用于对Request请求进行监听；
ServletRequestAttributeListener：对Request属性的监听。

### 监听器列表
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_1servletcontextlistener)①ServletContextListener
作用：监听ServletContext对象的创建与销毁

| 方法名 | 作用 |
| --- | --- |
| contextInitialized(ServletContextEvent sce) | ServletContext创建时调用 |
| contextDestroyed(ServletContextEvent sce) | ServletContext销毁时调用 |

ServletContextEvent对象代表从ServletContext对象身上捕获到的事件，通过这个事件对象我们可以获取到ServletContext对象。
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_2httpsessionlistener)②HttpSessionListener
作用：监听HttpSession对象的创建与销毁

| 方法名 | 作用 |
| --- | --- |
| sessionCreated(HttpSessionEvent hse) | HttpSession对象创建时调用 |
| sessionDestroyed(HttpSessionEvent hse) | HttpSession对象销毁时调用 |

HttpSessionEvent对象代表从HttpSession对象身上捕获到的事件，通过这个事件对象我们可以获取到触发事件的HttpSession对象。
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_3servletrequestlistener)③ServletRequestListener
作用：监听ServletRequest对象的创建与销毁

| 方法名 | 作用 |
| --- | --- |
| requestInitialized(ServletRequestEvent sre) | ServletRequest对象创建时调用 |
| requestDestroyed(ServletRequestEvent sre) | ServletRequest对象销毁时调用 |

ServletRequestEvent对象代表从HttpServletRequest对象身上捕获到的事件，通过这个事件对象我们可以获取到触发事件的HttpServletRequest对象。另外还有一个方法可以获取到当前Web应用的ServletContext对象。
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_4servletcontextattributelistener)④ServletContextAttributeListener
作用：监听ServletContext中属性的创建、修改和销毁

| 方法名 | 作用 |
| --- | --- |
| attributeAdded(ServletContextAttributeEvent scab) | 向ServletContext中添加属性时调用 |
| attributeRemoved(ServletContextAttributeEvent scab) | 从ServletContext中移除属性时调用 |
| attributeReplaced(ServletContextAttributeEvent scab) | 当ServletContext中的属性被修改时调用 |

ServletContextAttributeEvent对象代表属性变化事件，它包含的方法如下：

| 方法名 | 作用 |
| --- | --- |
| getName() | 获取修改或添加的属性名 |
| getValue() | 获取被修改或添加的属性值 |
| getServletContext() | 获取ServletContext对象 |

#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_5httpsessionattributelistener)⑤HttpSessionAttributeListener
作用：监听HttpSession中属性的创建、修改和销毁

| 方法名 | 作用 |
| --- | --- |
| attributeAdded(HttpSessionBindingEvent se) | 向HttpSession中添加属性时调用 |
| attributeRemoved(HttpSessionBindingEvent se) | 从HttpSession中移除属性时调用 |
| attributeReplaced(HttpSessionBindingEvent se) | 当HttpSession中的属性被修改时调用 |

HttpSessionBindingEvent对象代表属性变化事件，它包含的方法如下：

| 方法名 | 作用 |
| --- | --- |
| getName() | 获取修改或添加的属性名 |
| getValue() | 获取被修改或添加的属性值 |
| getSession() | 获取触发事件的HttpSession对象 |

#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_6servletrequestattributelistener)⑥ServletRequestAttributeListener
作用：监听ServletRequest中属性的创建、修改和销毁

| 方法名 | 作用 |
| --- | --- |
| attributeAdded(ServletRequestAttributeEvent srae) | 向ServletRequest中添加属性时调用 |
| attributeRemoved(ServletRequestAttributeEvent srae) | 从ServletRequest中移除属性时调用 |
| attributeReplaced(ServletRequestAttributeEvent srae) | 当ServletRequest中的属性被修改时调用 |

ServletRequestAttributeEvent对象代表属性变化事件，它包含的方法如下：

| 方法名 | 作用 |
| --- | --- |
| getName() | 获取修改或添加的属性名 |
| getValue() | 获取被修改或添加的属性值 |
| getServletRequest () | 获取触发事件的ServletRequest对象 |

#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_7httpsessionbindinglistener)⑦HttpSessionBindingListener
作用：监听某个对象在Session域中的创建与移除

| 方法名 | 作用 |
| --- | --- |
| valueBound(HttpSessionBindingEvent event) | 该类的实例被放到Session域中时调用 |
| valueUnbound(HttpSessionBindingEvent event) | 该类的实例从Session中移除时调用 |

HttpSessionBindingEvent对象代表属性变化事件，它包含的方法如下：

| 方法名 | 作用 |
| --- | --- |
| getName() | 获取当前事件涉及的属性名 |
| getValue() | 获取当前事件涉及的属性值 |
| getSession() | 获取触发事件的HttpSession对象 |

#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter11/verse02.html#_8httpsessionactivationlistener)⑧HttpSessionActivationListener
作用：监听某个对象在Session中的序列化与反序列化。

| 方法名 | 作用 |
| --- | --- |
| sessionWillPassivate(HttpSessionEvent se) | 该类实例和Session一起钝化到硬盘时调用 |
| sessionDidActivate(HttpSessionEvent se) | 该类实例和Session一起活化到内存时调用 |

HttpSessionEvent对象代表事件对象，通过getSession()方法获取事件涉及的HttpSession对象。
# ajax（axios）
## ajax概述

Ajax即Asynchronous Javascript And XML（异步JavaScript和XML）在 2005年被Jesse James Garrett提出的新术语，用来描述一种使用现有技术集合的‘新’方法，包括: HTML 或 XHTML, CSS, JavaScript, DOM, XML, XSLT, 以及最重要的XMLHttpRequest。 [3] **使用Ajax技术网页应用能够快速地将增量更新呈现在用户界面上，而不需要重载（刷新）整个页面，这使得程序能够更快地回应用户的操作**。

参考 原生ajax：[https://www.runoob.com/php/php-ajax-intro.html](https://www.runoob.com/php/php-ajax-intro.html)
** **
## axios 介绍

1. axios 是一个专注于网络请求的库，核心负责发请求和拿数据
在后面的vue react都会用axios来请求数据（类似于后端 httpclient）
2. Axios，是一个基于promise [5] 的网络请求库，作用于[node.js](https://baike.baidu.com/item/node.js/7567977)和[浏览器](https://baike.baidu.com/item/%E6%B5%8F%E8%A7%88%E5%99%A8/213911)中，它是 isomorphic 的(即同一套代码可以运行在浏览器和node.js中)。在[服务端](https://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E7%AB%AF/6492316)它使用原生node.js [http](https://baike.baidu.com/item/http/243074)模块, 而在客户端 (浏览端) 则使用XMLHttpRequest。
3. [https://www.axios-http.cn/](https://www.axios-http.cn/)

### 1、服务器端渲染
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416591680-95968b18-d242-4767-b72a-35f98e8c47ed.png#averageHue=%23e6e6e6&clientId=uf9f05580-74c9-4&from=paste&id=u12f73134&originHeight=598&originWidth=991&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u69e06bd8-c2d3-4c26-9695-4e86a527a77&title=)
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_2%E3%80%81ajax%E6%B8%B2%E6%9F%93-%E5%B1%80%E9%83%A8%E6%9B%B4%E6%96%B0)2、Ajax渲染（局部更新）
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416591660-14bf2f87-e9b8-463f-9eff-940958099064.png#averageHue=%23e2e1e1&clientId=uf9f05580-74c9-4&from=paste&id=u0926d6ef&originHeight=470&originWidth=931&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=ud0c4830a-7204-440b-81fd-48499365a39&title=)
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_3%E3%80%81%E5%89%8D%E5%90%8E%E7%AB%AF%E5%88%86%E7%A6%BB)3、前后端分离
彻底舍弃服务器端渲染，数据全部通过Ajax方式以JSON格式来传递。
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_4%E3%80%81%E5%90%8C%E6%AD%A5%E4%B8%8E%E5%BC%82%E6%AD%A5)4、同步与异步
Ajax本身就是Asynchronous JavaScript And XML的缩写，直译为：异步的JavaScript和XML。在实际应用中Ajax指的是：**不刷新浏览器窗口**，**不做页面跳转**，**局部更新页面内容**的技术。
**『同步』**和**『异步』**是一对相对的概念，那么什么是同步，什么是异步呢？
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_1%E5%90%8C%E6%AD%A5)①同步
多个操作**按顺序执行**，前面的操作没有完成，后面的操作就必须**等待**。所以同步操作通常是**串行**的。
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416591803-cab5f460-dd94-441a-acc4-229962ed5911.png#averageHue=%23f6f6f6&clientId=uf9f05580-74c9-4&from=paste&id=u46232bac&originHeight=422&originWidth=417&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=u4b588354-140f-4f49-b675-5d4fc26c4a4&title=)
#### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_2%E5%BC%82%E6%AD%A5)②异步
多个操作相继开始**并发执行**，即使开始的先后顺序不同，但是由于它们各自是**在自己独立的进程或线程中**完成，所以**互不干扰**，**谁也不用等谁**。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416591439-85f9b7f5-20dd-4a05-8bef-e8b648851c73.png#averageHue=%23f6f6f6&clientId=uf9f05580-74c9-4&from=paste&id=u5ffc76ce&name=image.png&originHeight=388&originWidth=278&originalType=url&ratio=1&rotation=0&showTitle=false&size=7694&status=done&style=shadow&taskId=u1504dd97-5841-42b4-891f-3c7dca299a6&title=)
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse01.html#_5%E3%80%81axios%E7%AE%80%E4%BB%8B)5、Axios简介
使用原生的JavaScript程序执行Ajax极其繁琐，所以一定要使用框架来完成。而Axios就是目前最流行的前端Ajax框架。
[Axios官网(opens new window)](http://www.axios-js.com/)
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416591629-3479f6e4-8723-441d-848a-8d1b35bdda0b.png#averageHue=%231a232a&clientId=uf9f05580-74c9-4&from=paste&id=u48c7e815&originHeight=444&originWidth=1178&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=ue1d353ad-1171-4d15-baf0-c387afb3bef&title=)
使用Axios和使用Vue一样，导入对应的*.js文件即可。官方提供的script标签引入方式为：
```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```
我们可以把这个axios.min.js文件下载下来保存到本地来使用。

## 发送请求体JSON
### [#](https://heavy_code_industry.gitee.io/code_heavy_industry/pro001-javaweb/lecture/chapter12/verse02.html#_1%E5%89%8D%E7%AB%AF%E4%BB%A3%E7%A0%81-2)①前端代码
> axios依赖：<script src="[https://unpkg.com/axios/dist/axios.min.js"></script>](https://unpkg.com/axios/dist/axios.min.js"></script>)

```html
<button @click="requestBodyJSON">请求体JSON</button>
```
```java
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

    ……
"methods":{
    "requestBodyJSON":function () {
        axios({
            "method":"post",
            "url":"/demo/AjaxServlet?method=requestBodyJSON",
            "data":{
                "stuId": 55,
                "stuName": "tom",
                "subjectList": [
                    {
                        "subjectName": "java",
                        "subjectScore": 50.55
                    },
                    {
                        "subjectName": "php",
                        "subjectScore": 30.26
                    }
                ],
                "teacherMap": {
                    "one": {
                        "teacherName":"tom",
                        "tearcherAge":23
                    },
                    "two": {
                        "teacherName":"jerry",
                        "tearcherAge":31
                    },
                },
                "school": {
                    "schoolId": 23,
                    "schoolName": "atguigu"
                }
            }
        }).then(function (response) {
            // 发送成功后进入
            console.log(response);
        }).catch(function (error) {
            // 发送异常后进入
            console.log(error);
        });
    }
}
……

```

### axios程序接收到的响应对象结构
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675416977458-26cbe9c4-ddfd-4612-85c4-ff15260e1a5d.png#averageHue=%23fcfcfc&clientId=uf9f05580-74c9-4&from=paste&id=u8439399d&originHeight=171&originWidth=573&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&taskId=ufca6bcd9-1e53-4386-8c63-4929981b5bd&title=)

| 属性名 | 作用 |
| --- | --- |
| config | 调用axios(config对象)方法时传入的JSON对象 |
| data | 服务器端返回的响应体数据 |
| headers | 响应消息头 |
| request | 原生JavaScript执行Ajax操作时使用的XMLHttpRequest |
| status | 响应状态码 |
| statusText | 响应状态码的说明文本 |


# vue
## 快速入门(vue2)
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>

	</head>
	<body>

	<div id="p1">
		<div id="p1"><p>{{message}}</p></div>
		<h1>{{ message }}</h1>
	</div>

	</body>
	<script>
		var vue =  new Vue({
			el:"#p1",
			data:{
				message:"jackson"
			}
		});
	</script>
</html>

```

## v-once：表示值仅仅改变一次    v-html：表示可以解析html标签
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="div">
			<h1 v-once>{{mes1}}</h1>
			<h1 v-html="mes2"></h1>
		</div>
		<script type="text/javascript">
			var v = new Vue({
				el:"#div",
				data:{
					mes1:"my name is jackson",
					mes2:"<p style='color:red'>my name is jackson....</p>"
				}
			});
		</script>
	</body>
</html>
```

## 支持函数的使用、三元运算
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="div">
			<h1 v-once>{{mes1.split('').reverse().join('')}}</h1>
			<h1 >{{mes3>7?"successfully":"failure"}}</h1>
		</div>
		<script type="text/javascript">
			var v = new Vue({
				el:"#div",
				data:{
					mes1:"my name is jackson",
					mes2:"<p style='color:red'>my name is jackson....</p>",
					mes3:5
				}
			});
		</script>
	</body>
</html>
```

## 支持条件判断（v-if），属性绑定（v-bind）
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
		
	</head>
	<body>
		<div id="test">
			<h1 v-if="flag">flag</h1>
			<a v-bind:href="url">baidu</a>
			<h2 @click="click">cllick to log...</h2>
		</div>
		
		<script type="text/javascript">
			var vu = new Vue({
				el:"#test",
				data:{
					flag:false,
					url:"https://www.baidu.com/"
				},
				methods:{
					click:function(){
						console.log("click to log...");
					}
				}
			});
		</script>
	</body>
</html>
```

## class和style的绑定，绑定时支持三元运算符
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<!-- 大括号 -->
			<h1 v-bind:class="{class1:mes1,class2:mes2}">{{mes3}}</h1>
			<!-- 中括号 -->
			<h1 v-bind:class="[mes1?'class1':'',mes4?'class2':'']">{{mes3}}</h1>
			<h1 v-bind:class="['mes5','mes6']">{{mes3}}</h1>
			
			<h1 :style="{color:color, fontFamily:fontFamily}">怪盗杰克</h1>
		</div>
		<script type="text/javascript">
			var vue = new Vue({
				el:"#test",
				data:{
					mes1:true,
					mes2:true,
					mes4:false,
					mes3:"怪盗杰克",
					color:"darkgreen",
					fontFamily:"楷体"
				}
			});
		</script>
		<style type="text/css">
			.class1{
				color: blue;
			}
			.class2{
				font-family: '楷体';
			}
			.mes5{
				color: blueviolet;
			}
			.mes6{
				font-size: 50px;
			}
		</style>
	</body>
</html>
```

## v-if和v-show
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<h1 v-show="flag2">jackson  v-show</h1>
			<h1 v-if="flag1">jackson ---1</h1>
			<h1 v-else>jackson ---2</h1>
			<h1 v-if="score<=60">D</h1>
			<h1 v-else-if="score<=70">C</h1>
			<h1 v-else-if="score<=80">B</h1>
			<h1 v-else-if="score<=90">A</h1>
			<h1 v-else>S</h1>
		</div>
		<script type="text/javascript">
			var v = new Vue({
				el:"#test",
				data:{
					flag1:true,
					score:77,
					flag2:false
				}
			});
		</script>
	</body>
</html>
```

## 列表渲染：v-for
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<ul>
				<li v-for="item in items">{{item}}</li>
				<hr>
				<li v-for="item,index in items">{{index}}-{{item}}</li>
				<hr>
				<li v-for="value in obj">{{value}}</li>
				<hr>
				<li v-for="key,value in obj">{{key}} : {{value}}</li>
				<hr>
				<li v-for="key,value,index in obj">{{index}}-{{key}} : {{value}}</li>
			</ul>
		</div>
		<script>
			var v = new Vue({
				el:"#test",
				data:{
					items:["jack","jackson","black","simthe"],
					obj:{
						name:"jackson",
						age:13,
						sex:"male",
						score:90
					}
				}
			});
		</script>
	</body>
</html>
```

## 事件处理
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<button v-on:click="met">number={{counter}}</button><br>
			<hr>
			<button v-on:dblclick="emt('jackson')">double click</button>
		</div>
	</body>
	<script type="text/javascript">
		var v = new Vue({
			el:"#test",
			data:{
				counter:0
			},
			methods:{
				met : function(){
					this.counter++;
				},
				emt : function(str){
					alert("hello" + str);
				},
			}
		});
	</script>
</html>
```

##  表单输入绑定
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<form action="#">
				<p>username：<input type="text" v-model="username"></p>
				<p>username：{{username}}</p>
				<hr>
				<p>sex：
					<input type="radio" name="sex" v-model="sex" value="male">male
					<input type="radio" name="sex" v-model="sex" value="female">female
				</p>
				<p>sex：{{sex}}</p>
				<hr>
				<p>hobby：
					<input type="checkbox" name="hobby" value="basketball" v-model="hobby">
					<input type="checkbox" name="hobby" value="football" v-model="hobby">
					<input type="checkbox" name="hobby" value="running" v-model="hobby">
					<input type="checkbox" name="hobby" value="swimming" v-model="hobby">
					<p>hobby：{{hobby}}</p>
				</p>
				<hr>
				<p>
					<input type="submit" value="submit" @click="submit()">
				</p>
			</form>
			<hr>
		</div>
		<script type="text/javascript">
			var v = new Vue({
				el:"#test",
				data:{   // 默认值，只需在此处添加值就行了
					username:"",
					sex:"",
					hobby:[]    ,// 复选框的值需要使用数组来接收
					obj:""
				},
				methods:{
					submit : function(){
						this.obj={
							username:this.username,
							sex:this.sex,
							hobby:this.hobby
						};

					}
				}
			});
		</script>
	</body>
</html>
```

## 组件基础
```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
	</head>
	<body>
		<div id="test">
			<!-- 组件可复用 -->
			<button-component jacks="title-->"><p>jacskon</p></button-component>
			<button-component jacks="title-->"></button-component>
		</div>
		
		<script type="text/javascript">
			Vue.component('button-component',{
				// 添加属性
				props:['jacks'],
				data:function(){
					return {
						counter:0
					}
				},
				// 组件必须要有唯一的根标签
				// <slot></slot> 声明组件插槽，可以插入其他标签
				template: '<div><p>my name is jackson</p><slot></slot><button v-on:click="clicknow()">{{jacks}}click on add one: {{counter}} time cliked</button></div>',
				methods:{
					clicknow : function(){
						this.counter++;
					}
				}
			})
			var v = new Vue({
				el:"#test",
				data:{
					
				}
			});
		</script>
	</body>
</html>
```

## 入门小案例（vue3）
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  </head>
  <body>
    <div id="test"></div>
    <script>
      // 创建根组件，vue3中，组件就是js对象
      const Root = {
        // data 方法中的属性可以在组件中使用，常用于解析json数据
        data() {
          return {
            message:'jackson'
          }
        },
        // template(用于组件化开发)
        template: "<p>my name is {{message}}...</p>"
      }
      // 创建实例
      const root = Vue.createApp(Root)
      // 将实例挂载到页面上
      root.mount("#test")

      // 与上面等效
      // Vue.createApp({template:"my name is jackson..."}).mount("#test")
    </script>
  </body>
</html>
```
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  </head>
  <body>
    <div></div>
    <script type="text/javascript">
      const component = {
        data(){
          return {
            count:0
          }
        },
        methods:{
          counter : function(){
            this.count++;
          }
        },
        template:"<button @click='counter'>click add one... cliked {{count}} times...</button>"
      }
      Vue.createApp(component).mount("div")
    </script>
  </body>
</html>
```

## template的使用
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>title</title>
        <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    </head>
    <body>
        <!--当使用了模板组件，根标签下的内容将全部会被替换为组件内容-->
        <div></div>
        <script type="text/javascript">
            const demo = {
                data(){
                    return {
                        message : "jackson"
                    }
                },
                // template定义方式：
                    // 1.使用template属性定义
                    // 2.直接定义在根标签中
                    // 3.使用render()方法直接渲染
                template : "<p>my name is {{message}}</p>"
            }
            Vue.createApp(demo).mount('div')
        </script>
    </body>
</html>
```
# json
## json基础语法
> JSON: JavaScript Object Notation(JavaScript 对象表示法)；

JSON 是存储和交换文本信息的语法，类似 XML；
JSON ---数据交换格式
客户端与服务器端之间通讯  ---数据交换格式
JSON  体积XML 更小减少带宽传输
JSON 比 XML 更小、更快，更易解析；

1. JSON是由键值对构成的；
2. 键要用引号（单双都行）引起，也可以不引；
3. 取值范围：
   1. 数字（整数或浮点数）
   2. 字符串（在双引号中）
   3. 逻辑值（true 或 false）
   4. 数组（在中括号中）
   5. 对象（在大括号中）
   6. null，不常用
```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jsonDemo</title>
    <script>
        // 1.定义json
        var json1 = {name: "mayikt", age: 22, isFlag: true};
        alert(json1.name);
        alert(json1.age);
        alert(json1.isFlag);
        // 2.定义json数组
        var jsonArr = {code: 200, data: [{name: "mayikt", age: 22}, {name: "meite", age: 22}]};
        alert(jsonArr.code);
        alert(jsonArr.data[0].name);
        var userArr = jsonArr.data;
        for (var i = 0; i < userArr.length; i++) {
            alert(userArr[i].name + "," + userArr[i].age);
        }
    </script>
</head>
<body>

</body>
</html>


```

##  Gson用法  
常见解析器：JsonLib，Gson，fastjson(阿里)，jackson（Spring MVC内置解析器）（常用Gson）
文档：javaweb开发相关资料下载.notea
链接：[http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7](http://note.youdao.com/noteshare?id=61e2cc939390acc9c7e5017907e98044&sub=DAABBA2F350445D2AC6879CCC3F46EA7)
```java
    // 2.创建Gson对象
    Gson gson = new Gson();

    // 3.将Java对象转换为JSON对象
    String json = gson.toJson(student);

    // 4.设置响应体的内容类型
    response.setContentType("application/json;charset=UTF-8");
    response.getWriter().write(json);
```
```java
rotected void requestBodyJSON(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.由于请求体数据有可能很大，所以Servlet标准在设计API的时候要求我们通过输入流来读取
    BufferedReader reader = request.getReader();

    // 2.创建StringBuilder对象来累加存储从请求体中读取到的每一行
    StringBuilder builder = new StringBuilder();

    // 3.声明临时变量
    String bufferStr = null;

    // 4.循环读取
    while((bufferStr = reader.readLine()) != null) {
        builder.append(bufferStr);
    }

    // 5.关闭流
    reader.close();

    // 6.累加的结果就是整个请求体
    String requestBody = builder.toString();

    // 7.创建Gson对象用于解析JSON字符串
    Gson gson = new Gson();

    // 8.将JSON字符串还原为Java对象
    Student student = gson.fromJson(requestBody, Student.class);
    System.out.println("student = " + student);

    System.out.println("requestBody = " + requestBody);

    response.setContentType("text/html;charset=UTF-8");
    response.getWriter().write("服务器端返回普通文本字符串作为响应");
}
```
