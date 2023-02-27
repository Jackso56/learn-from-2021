# 准备工作
## 创建maven工程
> 导入依赖

```xml
<!-- 创建自定义标签，用于控制jar包的版本 -->
<properties>
  <spring.version>5.3.1</spring.version>
</properties>

<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <!--springmvc-->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${spring.version}</version>
  </dependency>
  <!-- Mybatis核心 -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
  </dependency>
  <!--mybatis和spring的整合包-->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
  </dependency>
  <!-- 连接池 -->
  <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.9</version>
  </dependency>
  <!-- junit测试 -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
  <!-- MySQL驱动 -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
  </dependency>
  <!-- log4j日志 -->
  <dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
  </dependency>
  <dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.0</version>
  </dependency>
  <!-- 日志 -->
  <dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
  </dependency>
  <!-- ServletAPI -->
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
  </dependency>
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
  </dependency>
  <dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
  </dependency>
  <!-- Spring5和Thymeleaf整合包 -->
  <dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
    <version>3.0.12.RELEASE</version>
  </dependency>
</dependencies>
```
## 创建数据表
```sql
create table if not exists `users`
(
  `user_id` varchar(256) not null auto_increment comment '用户ID' primary key,
  `user_name` varchar(256) not null comment '用户名',
  `user_age` varchar(256) not null comment '年龄',
  `user_sex` varchar(256) not null comment '性别'
) comment '`users`';
```
# 配置web.xml
```xml

<!--    编码过滤器-->
<filter>
  <filter-name>characterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
  <init-param>
    <param-name>forceEncoding</param-name>
    <param-value>true</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>characterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

<!--        处理请求方式过滤器-->
<filter>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

<!--    配置前端控制器-->
<servlet>
  <servlet-name>dispatcherServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:mvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>dispatcherServlet</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>

<!--    配置sping监听器，在服务器启动时加载spring配置文件-->
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<!--    配置spring配置文件的自定义位置-->
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:spring.xml</param-value>
</context-param>
```
# 搭建springmvc的配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

  <!--    配置扫描组件-->
  <context:component-scan base-package="com.linmu.controller"></context:component-scan>

  <!-- 配置Thymeleaf视图解析器 -->
  <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
      <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver">
          <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
            <!-- 视图前缀 -->
            <property name="prefix" value="/WEB-INF/static/"/>
            <!-- 视图后缀 -->
            <property name="suffix" value=".html"/>
            <property name="templateMode" value="HTML5"/>
            <property name="characterEncoding" value="UTF-8" />
          </bean>
        </property>
      </bean>
    </property>
  </bean>

  <!--    配置默认servlet处理静态资源-->
  <mvc:default-servlet-handler></mvc:default-servlet-handler>

  <!--    开启mvc注解驱动-->
  <mvc:annotation-driven></mvc:annotation-driven>

  <!--    配置视图控制器-->
  <mvc:view-controller path="/" view-name="home_page"></mvc:view-controller>

  <!--    配置文件上传解析器-->
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>

</beans>
```
# 搭建mybatis环境
## 创建jdbc.properties配置文件
```xml
jdbc.driver=com.mysql.cj.jdbc.Driver
# ?url??mysql-8.0
jdbc.url=jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
jdbc.username=root
jdbc.password=000000
```
## 创建mybatis的核心配置文件mybatis-congfig.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--         mybatis核心配置文件标签的顺序-->
<!--        "(properties?,settings?,typeAliases?,typeHandlers?,-->
<!--        objectFactory?,objectWrapperFactory?,reflectorFactory?,-->
<!--        plugins?,environments?,databaseIdProvider?,mappers?)".-->
<configuration>

<!--   下划线转驼峰 -->
  <settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
  </settings>

<!--   分页插件 -->
  <plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
  </plugins>
</configuration>
```
## 创建Mapper接口和映射文件
> (两个文件的路径和名称要求一致)

```xml
package com.linmu.mapper;

import com.linmu.pojo.User;

import java.util.List;

public interface UserMapper {
    List<User> getAllUsers();
}

```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.linmu.mapper.UserMapper">

  <select id="getAllUsers" resultType="User">
    select * from users
  </select>

</mapper>
```
## 创建日志文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
  <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
    <param name="Encoding" value="UTF-8"/>
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}%m (%F:%L) \n"/>
    </layout>
  </appender>
  <logger name="java.sql">
    <level value="debug"/>
  </logger>
  <logger name="org.apache.ibatis">
    <level value="info"/>
  </logger>
  <root>
    <level value="debug"/>
    <appender-ref ref="STDOUT"/>
  </root>
</log4j:configuration>
```
# 创建spring的配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

  <!--    扫描组件-->
  <context:component-scan base-package="com.linmu">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>

  <!--    导入jdbc配置文件-->
  <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

  <!--    配置数据源-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${jdbc.driver}"></property>
    <property name="url" value="${jdbc.url}"></property>
    <property name="username" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
  </bean>

  <!--    配置事务管理器-->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
  </bean>

  <!--    开启事务注解驱动
  将使用注解@Transactional标识的方法或类中所有方法进行事务管理-->
  <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

  <!--    配置SqlSessionFactoryBean，可以直接在spring的IOC容器中直接获取SqlSessionFactory -->
  <bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <property name="dataSource" ref="dataSource"></property>
    <property name="typeAliasesPackage" value="com.linmu.pojo"></property>
  </bean>

  <!--    配置mapper接口的扫描，可以将指定包下的所有的mapper接口，
  通过SqlSession创建代理实现类对象，并将这些对象交给IOC容器管理-->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.linmu.mapper"></property>
  </bean>
</beans>
```
# 测试功能
## 创建组件
```java
package com.linmu.controller;

import com.github.pagehelper.PageInfo;
import com.linmu.pojo.User;
import com.linmu.service.dao.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.List;

/**
* @Author: Jackson Black
* @CreateTime: 2023-02-20  13:37:23
*/

@Controller
    public class UserController {
        @Autowired
        private UserService userService;

        @RequestMapping(
            value = "/getAllUsers",
            method = RequestMethod.GET
        )
        public String getAllUsers(Model model){
            List<User> users = userService.getAllUsers();
            model.addAttribute("users",users);
            return "show_data";
        }

        @RequestMapping(
            value = "/getAllUsers/page/{page}",
            method = RequestMethod.GET
        )
        public String getAllUsersByPage(@PathVariable("page") int page, Model model){
            PageInfo<User> pageNum = userService.getAllUsersByPage(page);
            model.addAttribute("page",pageNum);
            return "show_data";
        }
    }

```
## 创建页面
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  </head>
  <body>
    <h1>home_page successfully...</h1>
    <p><a th:href="@{/getAllUsers/page/1}">select * from users...</a></p>
  </body>
</html>

```
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <style>
      body{
        background-color: lightsteelblue;
      }
      table{
        background-color: lightgray;
        margin: 0 auto;
      }
      th,td{
        line-height: 30px;
        width: 100px;
        background-color: lightgoldenrodyellow;
        text-align: center;
      }
      div{
        margin: 30px auto;
        text-align: center;
      }
      a{
        text-decoration: none;
        color: darkblue;
      }

    </style>
  </head>
  <body>

    <table>
      <tr>
        <th colspan="6">UserInfo</th>
      </tr>
      <tr>
        <th>index</th>
        <th>user_id</th>
        <th>user_name</th>
        <th>user_age</th>
        <th>user_sex</th>
        <th>operation</th>
      </tr>
      <tr th:each="user,status:${page.list}">
        <td th:text="${status.count}"></td>
        <td th:text="${user.userId}"></td>
        <td th:text="${user.userName}"></td>
        <td th:text="${user.userAge}"></td>
        <td th:text="${user.userSex}"></td>
        <td>
          <a href="">add</a>
          <a href="">delete</a>
        </td>
      </tr>
    </table>
    <div>
      <a th:if="${page.hasPreviousPage}" th:href="@{/getAllUsers/page/1}">首页</a>
      <a th:if="${page.hasPreviousPage}" th:href="@{'/getAllUsers/page/'+${page.prePage}}">上一页</a>
      <span th:each="num : ${page.navigatepageNums}">
        <a th:if="${page.pageNum == num}" style="color: saddlebrown" th:href="@{'/getAllUsers/page/'+${num}}" th:text="'['+${num}+']'"></a>
        <a th:if="${page.pageNum != num}" th:href="@{'/getAllUsers/page/'+${num}}" th:text="${num}"></a>
      </span>
      <a th:if="${page.hasNextPage}" th:href="@{'/getAllUsers/page/'+${page.nextPage}}">下一页</a>
      <a th:if="${page.hasNextPage}" th:href="@{'/getAllUsers/page/'+${page.pages}}">末页</a>
    </div>
  </body>
</html>
```


