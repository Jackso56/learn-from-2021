# spring-简介
1. Spring 是轻量级的开源的 JavaEE 框架 
2. Spring 可以解决企业应用开发的复杂性 
3. Spring 有两个核心部分：IOC 和 Aop 
   1. IOC：控制反转，把创建对象过程交给 Spring 进行管理 
   2. Aop：面向切面，不修改源代码进行功能增强 	
4. Spring 特点 
   1. 方便解耦，简化开发 
   2. Aop 编程支持 
   3. 方便程序测试 
   4. 方便和其他框架进行整合 
   5. 方便进行事务操作 
   6. 降低 API 开发难度
   7. 想要提高java编程能力的圣经（spring源码）
5. spring核心模块

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674536036139-3ad3ef25-bffb-4769-8cbe-a8ea9781678d.png#averageHue=%235fbb0f&clientId=u373dc9cc-7f27-4&from=paste&height=400&id=u2d76aba2&name=image.png&originHeight=799&originWidth=1222&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123988&status=done&style=shadow&taskId=u3f77f11e-4d96-477c-ae4a-ab081fa1d81&title=&width=611)
# 
# spring-IOC（控制反转）
## IOC概念和原理

1. 什么是 IOC 
   1. 控制反转，把对象创建和对象之间的调用过程，交给 Spring 进行管理 
   2. 使用 IOC 目的：为了代码耦合度降低 
2. IOC 底层原理 
   1. xml 解析、工厂模式、反射
3. 图解IOC原理

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674536327735-30f44727-c06a-419f-9443-82654fac0224.png#averageHue=%23fefefe&clientId=u373dc9cc-7f27-4&from=paste&height=207&id=u49d68c73&name=image.png&originHeight=414&originWidth=1016&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25779&status=done&style=shadow&taskId=u46ae5581-ea90-4281-81b1-7239ad6c6ad&title=&width=508)
## IOC-BeanFactory接口

1. IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂 
2. Spring 提供 IOC 容器实现两种方式：（两个接口） 
   1. BeanFactory：IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用 加载配置文件时候不会创建对象，在获取对象（使用）才去创建对象 
   2. ApplicationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人 员进行使用 加载配置文件时候就会把在配置文件对象进行创建 
3. ApplicationContext 接口有实现类

![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674536786860-8d23d0db-b88c-4d5d-bfbb-5cca0de96965.png?x-oss-process=image%2Fresize%2Cw_1500%2Climit_0#averageHue=%23f9f9f8&from=url&id=TkKE6&originHeight=882&originWidth=1500&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)
## IOC操作-bean管理
### 概念

1. 什么是 **Bean 管理**：Bean 管理指的是两个操作 
   1. **Spring 创建对象 **
   2. **Spirng 注入属性**
2. Bean 管理操作有两种方式 
   1. 基于 xml 配置文件方式实现 
   2. 基于注解方式实现
### bean管理-基于xml配置文件方式管理
#### 创建对象
> 通过xml配置文件方式创建对象

```xml
<dependencies>
  <!--
  这个jar 文件包含Spring 框架基本的核心工具类。Spring 其它组件要都要使用到这个包里的类，是其它组件的基本核心，当然你也可以在自己的应用系统中使用这些工具类。
  外部依赖Commons Logging， (Log4J)。
  -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.2.1.RELEASE</version>
  </dependency>
  <!--
  这个jar 文件是所有应用都要用到的，它包含访问配置文件、创建和管理bean 以及进行Inversion ofControl / Dependency Injection（IoC/DI）操作相关的所有类。如果应用只需基本的IoC/DI 支持，引入spring-core.jar 及spring-beans.jar 文件就可以了。
  外部依赖spring-core，(CGLIB)。
  -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.2.1.RELEASE</version>
  </dependency>
  <!--
  这个jar 文件为Spring 核心提供了大量扩展。可以找到使用Spring ApplicationContext特性时所需的全部类，JDNI 所需的全部类，instrumentation组件以及校验Validation 方面的相关类。
  外部依赖spring-beans, (spring-aop)。
  -->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.1.RELEASE</version>
  </dependency>
<!--   测试 -->
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
  <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
  </dependency>
</dependencies>
```
```java
package com.linmu.entity;

/**
 * @author 2574550346
 */
public class Student {
    private String name;
    private int age;
    private String sex;

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Student() {
    }

    public Student(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
}

```
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    
    （1）在 spring 配置文件中，使用 bean 标签，标签里面添加对应属性，就可以实现对象创建
    （2）在 bean 标签有很多属性，介绍常用的属性
        * id 属性：唯一标识
        * class 属性：类全路径（包类路径）
    （3）创建对象时候，默认也是执行无参数构造方法完成对象创建
-->
    <bean id="student" class="com.linmu.entity.Student"></bean>
</beans>
```
```java
@Test
public void test01(){
    // 解析配置文件
    ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("spring.xml");
    // 根据id值获取bean对象，Student.class表示类加载
    Student student = classPathXmlApplicationContext.getBean("student", Student.class);
    System.out.println(student);
}
```
#### 依赖注入（DI）
> DI：即属性注入

##### DI-有参构造
```xml
<!--    the first method：DI by name -->
<bean id="studentByCon" class="com.linmu.entity.Student">
  <constructor-arg name="name" value="jackosn"></constructor-arg>
  <constructor-arg name="age" value="19" ></constructor-arg>
  <constructor-arg name="sex" value="male"></constructor-arg>
</bean>

<!--    the second method：DI by index-->
<bean id="studentByInd" class="com.linmu.entity.Student">
  <constructor-arg index="0" value="jackson-index"></constructor-arg>
  <constructor-arg index="1" value="20"></constructor-arg>
  <constructor-arg index="2" value="male"></constructor-arg>
</bean>
```

##### DI-set方法
```xml
<!--    DI by set-->
    <bean id="studentBySet" class="com.linmu.entity.Student">
        <property name="name" value="jackson-set"></property>
        <property name="age" value="21"></property>
        <property name="sex" value="male"></property>
    </bean>

```
##### 
##### DI-p
```xml
<!--    DI by p-->
<bean id="studentByp" class="com.linmu.entity.Student" p:name="jackson-p" p:age="21" p:sex="male"></bean>
```

##### DI-其实属性注入
###### 空值注入
```xml
<!--    DI-null-->
    <bean id="studentForNull" class="com.linmu.entity.Student">
        <property name="name" value="jackson"></property>
        <property name="age" value="19"></property>
        <property name="sex">
            <null></null>
        </property>
    </bean>
```

###### 特殊符号注入
```xml
<!--    DI-Special Symbol-->
    <bean id="studentForSpecialSymbol" class="com.linmu.entity.Student">
        <property name="name" >
            <value><![CDATA[#jackson-special symbol#]]></value>
        </property>
        <property name="age" value="19"></property>
        <property name="sex" value="male"></property>
    </bean>
```

###### 外部bean（级联注入）
```xml
<!--    outer bean-->
    <bean id="outerEmp" class="com.linmu.entity.Emp">
        <property name="name" value="jackson"></property>
        <property name="age" value="19"></property>
        <property name="dep" ref="outerDep"></property>
    </bean>
    <bean id="outerDep" class="com.linmu.entity.Dep">
        <property name="name" value="IT department"></property>
    </bean>
```

###### 内部bean
```xml
<!--    inner bean-->
    <bean id="innerBean" class="com.linmu.entity.Emp">
        <property name="name" value="jackson"></property>
        <property name="age" value="20"></property>
        <property name="dep">
            <bean id="innerDep" class="com.linmu.entity.Dep">
                <property name="name" value="IT department by inner"></property>
            </bean>
        </property>
    </bean>
```

###### 集合注入
```xml
<!--    DI-datas-->
    <bean id="data" class="com.linmu.entity.Datas">
        <property name="names">
            <array>
                <value>jackosn-array</value>
                <value>jack-array</value>
                <value>john-array</value>
            </array>
        </property>
        <property name="set">
            <set>
                <value>jackso-set</value>
                <value>jack-set</value>
                <value>john-set</value>
            </set>
        </property>
        <property name="list">
            <list>
                <value>jackson-list</value>
                <value>jack-list</value>
                <value>john-list</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="name" value="jackson"></entry>
                <entry key="job" value="IT"></entry>
                <entry key="sex" value="male"></entry>
            </map>
        </property>
    </bean>
```

###### util代码复用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675825185526-8571f33c-8953-47f2-9922-19cf2f0806e6.png#averageHue=%23282c34&clientId=ud741340a-f2ac-4&from=paste&height=194&id=u137bbcf3&name=image.png&originHeight=388&originWidth=2220&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107434&status=done&style=shadow&taskId=ucbe00e3b-059f-4306-a782-ec612d0983e&title=&width=1110)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675825209754-f1cf2094-5595-4f77-a2cb-5a8810bf342c.png#averageHue=%23282e36&clientId=ud741340a-f2ac-4&from=paste&height=146&id=ubc62c8a3&name=image.png&originHeight=292&originWidth=1010&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65711&status=done&style=shadow&taskId=uc572e944-785e-4e95-a3bd-a06b281c8c0&title=&width=505)
```xml
<!--    util-->
    <util:list id="listByUtil">
        <value>jackson-util</value>
        <value>jack-util</value>
        <value>mike-util</value>
    </util:list>
    <bean id="dataByUtil" class="com.linmu.entity.Datas">
        <property name="list" ref="listByUtil"></property>
    </bean>
```

###### 集合传入对象
```xml
<!--    set-objeat-->
    <bean id="dataForObject" class="com.linmu.entity.DataO">
        <property name="list">
            <list>
                <ref bean="studentByInd"></ref>
                <ref bean="studentBySet"></ref>
                <ref bean="studentByCon"></ref>
            </list>
        </property>
    </bean>
```
#### 
#### bean的种类
> Spring 有两种类型 bean，一种普通 bean，另外一种工厂 bean（FactoryBean） 

1. 普通 bean：在配置文件中定义 bean 类型就是返回类型 
2. 工厂 bean：在配置文件定义 bean 类型可以和返回类型不一样
   1. 第一步 创建类，让这个类作为工厂 bean，实现接口 FactoryBean 
   2. 第二步 实现接口里面的方法，在实现的方法中定义返回的 bean 类型

#### bean的作用域

1. 单例：每次在调用getbean方法获取对象都是为同一个对象；
2. 多例：每次在调用getbean方法获取对象都是一个新的对象。

注意：在spring默认的情况下，bean的作用域就是为单例 节约服务器内存。
单例：在同一个jvm中，该bean对象只会创建一次。
多例：在同一个jvm中，该bean对象可以被创建多次
```xml
<!--    
bean的作用域:
singleton表示单例
prototype表示多例
-->
<bean id="student_10" class="com.linmu.entity.Student" scope="singleton"></bean>
<bean id="student_11" class="com.linmu.entity.Student" scope="prototype"></bean>
```

#### bean的生命周期
> 生命周期概念：对象的创建与销毁的过程，类似与servlet生命的周期过程。

生命周期的原理：

1. 通过构造函数创建bean对象（默认执行无参构造函数 底层基于反射实现）
2. 为bean的属性设置 （使用反射调用set方法）
3. 将bean传递给后置处理器 调用初始化方法之前执行
4. 调用bean的初始化的方法（需要单独在类中配置初始化的方法）
5. 将bean传递给后置处理器 调用初始化方法之后执行
6. 正常使用bean对象
7. Spring容器关闭，调用该类的销毁回调的方法（需要单独在类中配置销毁的方法）
```java

package com.linmu.entity;

public class BeanObject {
    private String name;

    public BeanObject() {
        System.out.println("1.调用无参构造器创建对象...");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        System.out.println("2.调用set方法给属性赋值...");
        this.name = name;
    }

    public void init(){
        System.out.println("3.调动init方法初始化对象...");
    }

    public void destroy(){
        System.out.println("5.调用destroy方法销毁对象...");
    }

    @Override
    public String toString() {
        return "BeanObject{" +
            "name='" + name + '\'' +
            '}';
    }
}

```
```java
package com.linmu.entity;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.core.Ordered;

// 后置处理器的创建需要实现BeanPostProcessor接口
public class Processor_1 implements BeanPostProcessor, Ordered {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("[01]init方法之前调用后置处理器...");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("[01]init方法之后调用后置处理器...");
        return bean;
    }

    /**
* 实现Ordered接口，重写getOrder方法，
* 来实现后置处理器调用的先后顺序，
* 返回值越小优先级越高
* @return
*/
    @Override
    public int getOrder() {
        return 1;
    }
}
```
```java
package com.linmu.entity;

import com.sun.org.apache.xpath.internal.operations.Or;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.core.Ordered;

// 后置处理器的创建需要实现BeanPostProcessor接口
public class Processor_2 implements BeanPostProcessor, Ordered {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("[02]init方法之前调用后置处理器...");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("[02]init方法之后调用后置处理器...");
        return bean;
    }

    /**
* 实现Ordered接口，重写getOrder方法，
* 来实现后置处理器调用的先后顺序，
* 返回值越小优先级越高
* @return
*/
    @Override
    public int getOrder() {
        return 1;
    }
}
```
```xml

<!--    bean的生命周期-->
<bean id="pro1" class="com.linmu.entity.Processor_1"></bean> <!--后置处理器-->
<bean id="pro2" class="com.linmu.entity.Processor_2"></bean> <!--后置处理器-->
<bean id="beanobject" class="com.linmu.entity.BeanObject" init-method="init" destroy-method="destroy">
  <property name="name" value="jackson"></property>
</bean>
```
```java
package spring_bean;

import com.linmu.entity.BeanObject;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test15 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("spring.xml");
        BeanObject beanobject = classPathXmlApplicationContext.getBean("beanobject", BeanObject.class);
        System.out.println("4.正常使用bean对象...");
        System.out.println(beanobject);
        classPathXmlApplicationContext.close();
    }
}
```

##### 外部属性文件
```
mysql.driver=com.mysql.cj.jdbc.Driver
# 该url用于mysql-8.0
mysql.url=jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
mysql.username=root
mysql.password=000000
```
```xml
<!--    External properties file-->
<!--    import configration file-->
<context:property-placeholder location="mysql.properties"></context:property-placeholder>
<bean id="connection" class="com.linmu.entity.Connection">
  <property name="driver" value="${mysql.driver}"></property>
  <property name="url" value="${mysql.url}"></property>
  <property name="username" value="${mysql.username}"></property>
  <property name="password" value="${mysql.password}"></property>
</bean>
```
```java
package com.linmu.entity;

public class Connection {
    private String driver;
    private String url;
    private String username;
    private String password;

    @Override
    public String toString() {
        return "Connection{" +
            "driver='" + driver + '\'' +
            ", url='" + url + '\'' +
            ", username='" + username + '\'' +
            ", password='" + password + '\'' +
            '}';
    }

    public String getDriver() {
        return driver;
    }

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Connection() {
    }

    public Connection(String driver, String url, String username, String password) {
        this.driver = driver;
        this.url = url;
        this.username = username;
        this.password = password;
    }
}

```
```java
package com.liumu.entity;

import com.linmu.entity.Connection;
import org.junit.jupiter.api.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestConnection {

    @Test
    public void connection(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Connection connection = classPathXmlApplicationContext.getBean("connection", Connection.class);
        System.out.println(connection);
    }
}


```

### bean管理-基于注解方式
#### 对象创建

1. 什么是注解 
   1. 注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值..) 
   2. 使用注解，注解作用在类上面，方法上面，属性上面 
   3. 使用注解目的：简化 xml 配置 
2. Spring 针对 Bean 管理中创建对象提供注解 
   1. @Component 
   2. @Service 
   3. @Controller 
   4. @Repository 

 上面四个注解功能是一样的，都可以用来创建 bean 实例

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>5.2.6.RELEASE</version>
</dependency>
```
```java
package com.linmu.entityByAnnotation;

import org.springframework.stereotype.Component;

//在注解里面 value 属性值可以省略不写，
//默认值是类名称，首字母小写
@Component // 创建对象
    public class Employee {
        private String name;
        private Integer age;
        private String sex;

        @Override
        public String toString() {
            return "Employee{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public Integer getAge() {
            return age;
        }

        public void setAge(Integer age) {
            this.age = age;
        }

        public String getSex() {
            return sex;
        }

        public void setSex(String sex) {
            this.sex = sex;
        }

        public Employee() {
        }

        public Employee(String name, Integer age, String sex) {
            this.name = name;
            this.age = age;
            this.sex = sex;
        }
    }

```
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

<!--    up component-scanner-->
    <!--    注解开发,需要导入context约束-->
    <!--开启组件扫描
    1 如果扫描多个包，多个包使用逗号隔开
    2 扫描会扫描包下的所有类
    -->
    <context:component-scan base-package="com.linmu.entityByAnnotation"></context:component-scan>
</beans>
```
```java
package com.liumu.entityAnnotation;

import com.linmu.entityByAnnotation.Employee;
import org.junit.jupiter.api.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestEmployee {

    @Test
    public void testAnnotation(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("springAnno.xml");
        Employee employee = classPathXmlApplicationContext.getBean("employee", Employee.class);
        System.out.println(employee);
    }
}

```

#### 组件扫描（精准扫描,了解即可）
```java
<!--示例 1 
use-default-filters="false" 表示现在不使用默认 filter，自己配置 filter 
context:include-filter ，设置扫描哪些内容 
--> 
<context:component-scan base-package="com.atguigu" use-default-filters="false"> 
  <context:include-filter type="annotation" 
    expression="org.springframework.stereotype.Controller"/> 
</context:component-scan> 
<!--示例 2 
下面配置扫描包所有内容 
context:exclude-filter： 设置哪些内容不进行扫描 
--> 
<context:component-scan base-package="com.atguigu"> 
  <context:exclude-filter type="annotation" 
    expression="org.springframework.stereotype.Controller"/> 
</context:component-scan>
```

#### 属性注入

1. @Autowired：根据属性类型进行自动装配，不需要添加 set 方法 
   1. value：value 属性值可以省略不写，默认值是类名称，首字母小写
2. @Qualifier：根据名称进行注入 （以上两个主解是配套使用的）
   1. value：value 属性值写了表示根据名称进行注入，反之根据类型注入
3. @Resource ：默认根据类型进行注入 
   1. name：name写了属性值表示根据名称进行注入
4. @Value：注入普通属性
   1. value：表示属性值
> **前三个是对自定义类型的属性进行注入**，如Student。。。


```java

package com.linmu.entityByAnnotation;

import com.linmu.entity.Student;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

import javax.annotation.Resource;

//在注解里面 value 属性值可以省略不写，
//默认值是类名称，首字母小写
@Component // 创建对象
public class Employee {
//    @Autowired
//    @Qualifier(value = "String")
    @Value(value = "jackson")
    private String name;
//    @Resource
    @Value(value = "12")
    private Integer age;
    @Value(value = "male")
    private String sex;

    @Resource(name = "jackson")
    private Dog dog;

    @Autowired
    @Qualifier(value = "cat")
    private Cat cat;

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                ", dog=" + dog +
                ", cat=" + cat +
                '}';
    }

    public Employee(String name, Integer age, String sex, Dog dog, Cat cat) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.dog = dog;
        this.cat = cat;
    }

    public Employee(String name, Integer age, String sex, Dog dog) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Employee() {
    }

    public Employee(String name, Integer age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
}

```
```java

package com.linmu.entityByAnnotation;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Cat {
    @Value(value = "tom")
    private String name;

    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Cat() {
    }

    public Cat(String name) {
        this.name = name;
    }
}

```
```java
package com.linmu.entityByAnnotation;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component(value = "jackson")
public class Dog {
    @Value(value = "goff")
    private String name;

    public Dog() {
    }

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
```java

package com.liumu.entity;

import com.linmu.entity.Connection;
import org.junit.jupiter.api.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestConnection {

    @Test
    public void connection(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Connection connection = classPathXmlApplicationContext.getBean("connection", Connection.class);
        System.out.println(connection);
    }
}

```

### bean管理-完全注解开发
> 完全注解开发，使用注解类取代spring.xml配置文件
> 通过AnnotationConfigApplicationContext获取注解类
> **使用完全注解开发必须要声明无参构造，底层通过反射和无参构造创建对象**



```java
package com.linmu.annotation;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

@Configuration
@ComponentScan(basePackages = {"com.linmu.entityByFullAnnotation"})
public class AnnotationClass {

}

```
```java

package com.linmu.entityByFullAnnotation;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;

@Component
public class Animal {
    @Value("jackson")
    private String name;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;

    @Override
    public String toString() {
        return "Animal{" +
                "name='" + name + '\'' +
                ", dog=" + dog +
                '}';
    }

    public Animal(String name, Dog dog) {
        this.name = name;
        this.dog = dog;
    }

    public Animal() {
    }
}

```
```java

package com.linmu.entityByFullAnnotation;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Dog {
    @Value(value = "goff")
    private String name;

    public Dog(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dog() {
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                '}';
    }
}

```
```java

package com.liumu.fullAnnotationDevelopment;

import com.linmu.annotation.AnnotationClass;
import com.linmu.entityByFullAnnotation.Animal;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class TestFAD {

    @Test
    public void test(){
        AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext(AnnotationClass.class);
        Animal animal = annotationConfigApplicationContext.getBean("animal", Animal.class);
        System.out.println(animal);
    }
}

```

# spring-AOP
## AOP概念
> AOP即面向切面编程

1. 面向切面编程（方面），利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。 
2. 通俗描述：不通过修改源代码方式，在主干功能里面添加新功能

例子：使用登录例子说明 AOP
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674616868960-d9ebacd6-6214-45fc-9823-744d8d038ea3.png#averageHue=%23f8f8f8&from=url&id=TPH9k&originHeight=501&originWidth=941&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)

## AOP底层原理
> AOP底层使用动态代理，动态代理有两种情况

1. 有接口情况，使用JDK动态代理：创建接口实现类代理对象，增强类的方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674617118754-352c9dc5-19d3-46c4-8447-fed9366792e7.png#averageHue=%23eeeeee&clientId=u80783b88-ddbe-4&from=paste&height=185&id=u8913d2bb&name=image.png&originHeight=263&originWidth=1069&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19967&status=done&style=shadow&taskId=u1cde256d-8f09-4c2d-81e6-3f96709d9e3&title=&width=751.5)

2. 没有接口情况，使用 CGLIB 动态代理：创建子类的代理对象，增强类的方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674617212168-d60531cd-c9e9-4581-83b7-cb2c40def233.png#averageHue=%23f4f4f4&clientId=u80783b88-ddbe-4&from=paste&height=267&id=uc76cfd10&name=image.png&originHeight=357&originWidth=999&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21814&status=done&style=shadow&taskId=u9f55d039-f2c0-4c56-8e5b-665ec6220aa&title=&width=747.5)

## JDK动态代理

1. 使用 JDK 动态代理，使用 Proxy 类里面的方法创建代理对象

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674617434373-12d216f9-175f-4cff-b9a1-8c01b729a365.png#averageHue=%23fdfcfc&clientId=u80783b88-ddbe-4&from=paste&height=181&id=ue12fdb18&name=image.png&originHeight=361&originWidth=757&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28421&status=done&style=shadow&taskId=u4dde8f22-9f17-421d-b208-9bc8b59a286&title=&width=378.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674617461984-09467e12-63fa-448a-8b28-3fa0ed4954d3.png#averageHue=%237b9fbd&clientId=u80783b88-ddbe-4&from=paste&height=66&id=u560ec70e&name=image.png&originHeight=131&originWidth=1821&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37421&status=done&style=shadow&taskId=uc2528223-63df-4887-8843-48f6b2b20fd&title=&width=910.5)
方法有三个参数： 
第一参数，类加载器 
第二参数，增强方法所在的类，这个类实现的接口，支持多个接口 
第三参数，实现这个接口 InvocationHandler，创建代理对象，写增强的部分

2. 编写 JDK 动态代理代码
   1. 创建接口，定义方法
   2. 创建接口实现类，实现方法
   3. 使用 Proxy 类创建接口代理对象
```java
package com.linmu.aop.inter;

public interface Dao {
    void testAop();
}

```
```java
package com.linmu.aop.imp;

import com.linmu.aop.inter.Dao;

public class ImpDao implements Dao {
    @Override
    public void testAop() {
        System.out.println("call methods of TestAop()...");
    }
}

```
```java
package com.linmu.aop.proxy;

import com.linmu.aop.imp.ImpDao;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.util.Arrays;

public class DaoProxy implements InvocationHandler {

    // 需要代理的对象
    private Object object;

    public DaoProxy(Object object) {
        this.object = object;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // before method execute
        System.out.println("before method execute --> method name:" + method.getName() + ",args:" + Arrays.toString(args));
        // method execute
        Object invoke = method.invoke(new ImpDao());
        // after method execute
        System.out.println("after method execute --> " + object);
        return invoke;
    }
}

```
```java
package com.liumu.aop;

import com.linmu.aop.imp.ImpDao;
import com.linmu.aop.inter.Dao;
import com.linmu.aop.proxy.DaoProxy;
import org.junit.jupiter.api.Test;

import java.lang.reflect.Proxy;

public class TestAop {

    /**                                     类加载器
     * public static Object newProxyInstance(ClassLoader loader,
     *                                       需要代理的接口，数组的方式传输
     *                                       Class<?>[] interfaces,
     *                                       传入实现了InvocationHandler接口的对象，传入需要代理的接口
     *                                       InvocationHandler h)
     */
    @Test
    public void testAop(){
        Dao me =(Dao)Proxy.newProxyInstance(Dao.class.getClassLoader(),
                new Class[]{Dao.class}, new DaoProxy(new ImpDao()));
        me.testAop();
    }
}

```


## CGLIB 动态代理
### AOP术语

1. 连接点：类中可以被增强的方法，称为连接点
2. 切入点：实际上被增强的方法，称为切入点
3. 通知：实际增强部分称为通知
   1. 前置通知
   2. 后置通知
   3. 环绕通知
   4. 异常通知
   5. 最终通知
4. 切面：把通知应用到切入点的过程

### AOP操作前置工作

1. Spring 框架一般都是基于 AspectJ 实现 AOP 操作 
   1. AspectJ 不是 Spring 组成部分，独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使用，进行 AOP 操作 
   2. 基于 AspectJ 实现 AOP 操作 
2. AOP操作的两种方式
   1. 基于 xml 配置文件实现 
   2. 基于注解方式实现（常用）
3. 在项目工程里面引入 AOP 相关依赖
```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
<dependency>
  <groupId>com.mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <version>8.0.31</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.2.8</version>
</dependency>
```

4. **切入点表达式 **
   1. 切入点表达式作用：知道对哪个类里面的哪个方法进行增强 
   2. 语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]) ) 

举例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强 
execution(* com.atguigu.dao.BookDao.add(..)) 
举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强 
execution(* com.atguigu.dao.BookDao.* (..))
举例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强 
execution(* com.atguigu.dao.*.* (..))
:::tips
其中返回类型可以省略，（..）表示参数列表
:::

### AOP操作-注解方式
```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>5.2.1.RELEASE</version>
</dependency>
```

1. 创建类，在类里面定义方法
2. 创建增强类（编写增强逻辑） 
   1. 在增强类里面，创建方法，让不同方法代表不同通知类型
3. 进行通知的配置 
   1. 在 spring 配置文件中，开启注解扫描
   2. 使用注解创建 User 和 UserProxy 对象
   3. 在增强类上面添加注解 @Aspect
   4. 在 spring 配置文件中开启生成代理对象
4. 配置不同类型的通知 
   1. 在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置
5. 相同的切入点可以将excution表达式抽取，减少代码冗余
6. 有多个增强类多同一个方法进行增强，设置增强类优先级 
   1. 在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高
```java
package com.linmu.aop.entity;

import org.springframework.stereotype.Component;

@Component
    public class Student {
        public void aopByAnnotation(){
            System.out.println("Annotation-based AOP operation....");
        }
    }

```
```java
package com.linmu.aop.proxy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

@Component
// 表示代理类，增强类
@Aspect
//当有多个争强类时，可用于设置优先级
//@Order(value = 1)
public class StudentProxy {

    // 抽离相同的切入点表达式，减少代码冗余
    @Pointcut(value = "execution(* com.linmu.aop.entity.Student.aopByAnnotation (..))")
    public void printCut(){

    }

    @Before(value = "printCut()")
    public void before(){
        System.out.println("pre-notifaction...");
    }

    @After(value = "printCut()")
    public void after(){
        System.out.println("final notice...");
    }

    @AfterReturning(value = "printCut()")
    public void afterReturning(){
        System.out.println("post notifaction...");
    }

    @AfterThrowing(value = "printCut()")
    public void afterThrowing(){
        System.out.println("exception notifaction...");
    }

    @Around(value = "printCut()")
    public void around(ProceedingJoinPoint proceedingJoinPoint){
        System.out.println("surround notifaction...");
        try{
            // 不调用执行被增强的方法，前置通知不会执行
            proceedingJoinPoint.proceed();
        } catch (Throwable e) {
            e.printStackTrace();
        }
        System.out.println("surround notifaction...");
    }


}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--    开启扫描组件-->
  <context:component-scan base-package="com.linmu.aop"></context:component-scan>
  <!--    开启Aspect，生成代理对象-->
  <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```
```java
    @Test
    public void testAnnotationAop(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("aop.xml");
        Student student = classPathXmlApplicationContext.getBean("student", Student.class);
        student.aopByAnnotation();
    }
```

7. 完全使用注解开发：需要创建配置类，不需要创建 xml 配置文件 
```java
package com.linmu.aop;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

/**
 * 表示注解类
 * @Configuration
 * 开启注解扫描
 * @ComponentScan(basePackages = {"com.linmu.aop"})
 * 自动生成代理对象，默认值为false
 * @EnableAspectJAutoProxy(proxyTargetClass = true)
 */
@Configuration
@ComponentScan(basePackages = {"com.linmu.aop"})
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class Annotation {

}
```
```java
   @Test
    public void testAopByAnnotation(){
        AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext(Annotation.class);
        Student student = annotationConfigApplicationContext.getBean("student", Student.class);
        student.aopByAnnotation();
    }
```

> **需要通过proceedingJoinPoint.proceed();执行方法**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675910605132-c860ad62-3ece-4378-b922-4b1dccf9b4bb.png#averageHue=%232d323b&clientId=u23cc27f9-a181-4&from=paste&height=212&id=ub4ab1d7d&name=image.png&originHeight=424&originWidth=1462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72885&status=done&style=shadow&taskId=u1cfc0778-f50f-44c2-9818-a26f6209e76&title=&width=731)

### AOP操作-配置文件方式
```java
package com.linmu.aop.config;

public class Student {
    public void config(){
        System.out.println("aop operation by config...");
    }
}

```
```java
package com.linmu.aop.config;

public class Student {
    public void config(){
        System.out.println("aop operation by config...");
    }
}

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

  <!--    bean注入-->
  <bean id="student" class="com.linmu.aop.config.Student"></bean>
  <bean id="studentProxy" class="com.linmu.aop.config.StudentProxy"></bean>
  <!--    切面、切入点配置-->
  <aop:config>
    <!--        切入点配置-->
    <aop:pointcut id="preNotifacationMethod" expression="execution(* com.linmu.aop.config.Student.config(..))"/>
    <!--        切面配置-->
    <aop:aspect ref="studentProxy">
      <aop:before method="preNotification" pointcut-ref="preNotifacationMethod"></aop:before>
    </aop:aspect>
  </aop:config>
</beans>

```
```java
@Test
    public void testAopByConfig(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("aopByCon.xml");
        com.linmu.aop.config.Student student = classPathXmlApplicationContext.getBean("student", com.linmu.aop.config.Student.class);
        student.config();
    }
```
# spring-JdbcTemplate-操作数据库
## JdbcTemplate概述与使用配置
> 概念：Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

准备工作：

1. 导入jar包
2. 在 spring 配置文件配置数据库连接池
3. 配置 JdbcTemplate 对象，注入 DataSource
4. 创建 service 类，创建 dao 类，在 dao 注入 jdbcTemplate 对象
```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.0.31</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.21</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>5.3.20</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.3.24</version>
</dependency>
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

<!--    开启组件扫描-->
    <context:component-scan base-package="com.linmu.operateDatabase"></context:component-scan>
<!--    导入数据库配置-->
    <context:property-placeholder location="mysql.properties"></context:property-placeholder>
<!--    连接数据库-->
<!--    destroy-method="close" 表示关闭连接的方法-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
        <property name="driverClassName" value="${mysql.driver}"></property>
        <property name="url" value="${mysql.url}"></property>
        <property name="username" value="${mysql.username}"></property>
        <property name="password" value="${mysql.password}"></property>
    </bean>
<!--    bean注入-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>


</beans>
```
```java
package com.linmu.operateDatabase.dao;

public interface Dao {
}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;
}

```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class ServiceDao {

    @Autowired
    private ImpDao impDao;
}

```

## JdbcTemplate操作数据库(插入数据测试)

1. 对应数据库创建实体类
2. 编写 service 和 dao 
   1. 在 dao 进行数据库添加操作 
   2. 调用 JdbcTemplate 对象里面 update 方法实现添加操作
3. 测试（intsert、delete、update）
```sql
create table if not exists users_test(
  user_id int auto_increment not null primary key ,
  user_name varchar(256) not null ,
  user_age int not null ,
  user_sex varchar(256) not null
)
```
```java
package com.linmu.operateDatabase.entity;

public class User {
    private Integer user_id;
    private String user_name;
    private Integer user_age;
    private String user_sex;

    public User() {
    }

    @Override
    public String toString() {
        return "User{" +
            "user_id=" + user_id +
            ", user_name='" + user_name + '\'' +
            ", user_age=" + user_age +
            ", user_sex='" + user_sex + '\'' +
            '}';
    }

    public Integer getUser_id() {
        return user_id;
    }

    public void setUser_id(Integer user_id) {
        this.user_id = user_id;
    }

    public String getUser_name() {
        return user_name;
    }

    public void setUser_name(String user_name) {
        this.user_name = user_name;
    }

    public Integer getUser_age() {
        return user_age;
    }

    public void setUser_age(Integer user_age) {
        this.user_age = user_age;
    }

    public String getUser_sex() {
        return user_sex;
    }

    public void setUser_sex(String user_sex) {
        this.user_sex = user_sex;
    }

    public User(String user_name, Integer user_age, String user_sex) {
        this.user_name = user_name;
        this.user_age = user_age;
        this.user_sex = user_sex;
    }

    public User(Integer user_id, String user_name, Integer user_age, String user_sex) {
        this.user_id = user_id;
        this.user_name = user_name;
        this.user_age = user_age;
        this.user_sex = user_sex;
    }
}

```
```java
package com.linmu.operateDatabase.dao;

import com.linmu.operateDatabase.entity.User;

public interface Dao {
    int add(User user);
}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import com.linmu.operateDatabase.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int add(User user) {
        String sql = "insert into users values(?,?,?,?)";
        Object[] params = {user.getUser_id(),user.getUser_name(),user.getUser_age(),user.getUser_sex()};
        int update = jdbcTemplate.update(sql, params);
        return update;
    }
}

```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.entity.User;
import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class ServiceDao {

    @Autowired
    private ImpDao impDao;

    public int add(User user){
        return impDao.add(user);
    }
}

```
```java

package com.liumu.database;

import com.linmu.operateDatabase.entity.User;
import com.linmu.operateDatabase.service.ServiceDao;
import org.junit.jupiter.api.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class jdbcTemplate {

    @Test
    public void testInsert(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        User user = new User(null, "jackson", 21, "male");
        int add = serviceDao.add(user);
        System.out.println(add);
    }
}

```
## JdbcTemplate-updata and delete
```java
package com.linmu.operateDatabase.dao;

import com.linmu.operateDatabase.entity.User;

public interface Dao {
    int add(User user);
    int update(String name);
    int delete(String name);
}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import com.linmu.operateDatabase.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int add(User user) {
        String sql = "insert into users values(?,?,?,?)";
        Object[] params = {user.getUser_id(),user.getUser_name(),user.getUser_age(),user.getUser_sex()};
        int influenceRow = jdbcTemplate.update(sql, params);
        return influenceRow;
    }

    @Override
    public int update(String name) {
        String sql = "update users set user_age=30 where user_name=?";
        int influenceRow = jdbcTemplate.update(sql,name);
        return influenceRow;
    }

    @Override
    public int delete(String name) {
        String sql = "delete from users where user_name=?";
        int inFuleneceRow = jdbcTemplate.update(sql,name);
        return inFuleneceRow;
    }
}

```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.entity.User;
import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class ServiceDao {

    @Autowired
    private ImpDao impDao;

    public int add(User user){
        return impDao.add(user);
    }

    public int update(String name){
        return impDao.update(name);
    }

    public int dalete(String name){
        return impDao.delete(name);
    }
}

```
```java
    @Test
    public void testUpdate(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        int influenceRow = serviceDao.update("jackson");
        System.out.println(influenceRow);
    }

    @Test
    public void testDelete(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        int influenceRow = serviceDao.dalete("jackson");
        System.out.println(influenceRow);
    }
```

## JdbcTemplate-select
```java
package com.linmu.operateDatabase.dao;

import com.linmu.operateDatabase.entity.User;

import java.util.List;

public interface Dao {
    int selectId(String name);

    User selectByName(String name);

    List<User> selectAllUsers();

}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import com.linmu.operateDatabase.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;
import java.util.List;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int selectId(String name) {
        String sql = "select user_id from users where user_name=?";
        Integer result = jdbcTemplate.queryForObject(sql, Integer.class, name);
        return result;
    }

    @Override
    public User selectByName(String name) {
        String sql = "select * from users where user_name=?";
        User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), name);
        return user;
    }

    @Override
    public List<User> selectAllUsers() {
        String sql = "select * from users";
        // 放返回集合使用query，其他使用queryForObject
        List<User> users = jdbcTemplate.query(sql, new BeanPropertyRowMapper<User>(User.class));
        return users;
    }
}


```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.entity.User;
import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.util.List;

@Service
public class ServiceDao {

    @Autowired
    private ImpDao impDao;

    public Integer selectId(String name){
        return impDao.selectId(name);
    }

    public User selectByName(String name){
        return impDao.selectByName(name);
    }

    public List<User> selectAllUsers(){
        return impDao.selectAllUsers();
    }
}

```
```java
    @Test
    public void selectId(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        Integer result = serviceDao.selectId("jackson");
        System.out.println(result);
    }

    @Test
    public void selectName(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        User result = serviceDao.selectByName("jackson");
        System.out.println(result);
    }

    @Test
    public void selectAllUsers(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        List<User> result = serviceDao.selectAllUsers();
        result.forEach(user -> System.out.println(user));
    }
```

#### 批量操作

1. 批量操作：操作表里面多条记录 
2. JdbcTemplate 实现批量添加操作 有两个参数 

第一个参数：sql 语句 
第二个参数：List 集合，添加多条记录数据
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674644307531-9e192135-5b47-4629-bdab-308044e01933.png#averageHue=%23fafaf9&clientId=u999dde3d-3e89-4&from=paste&height=84&id=u29f130b0&name=image.png&originHeight=168&originWidth=1539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35651&status=done&style=shadow&taskId=ucfccd883-abf4-4c8d-afed-ec53f83d181&title=&width=769.5)
```java
package com.linmu.jdbcTemplate;


import java.util.List;

public interface Dao {
    void batchInsert(List<Object[]> list);
    void batchDelete(List<Object[]> list);
    void batchUpdate(List<Object[]> list);
}

```
```java

package com.linmu.jdbcTemplate;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.List;

@Service
public class DaoImp implements Dao{

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void batchInsert(List<Object[]> list) {
        String sql = "insert into userinfo values(?,?,?)";
        int[] ints = jdbcTemplate.batchUpdate(sql, list);
        System.out.println(Arrays.toString(ints));
    }

    @Override
    public void batchDelete(List<Object[]> list) {
        String sql = "delete from userinfo where id=?";
        int[] ints = jdbcTemplate.batchUpdate(sql, list);
        System.out.println(Arrays.toString(ints));
    }

    @Override
    public void batchUpdate(List<Object[]> list) {
        String sql = "update userinfo set age=? where id=?";
        int[] ints = jdbcTemplate.batchUpdate(sql, list);
        System.out.println(Arrays.toString(ints));
    }
}

```
```java
package com.linmu.jdbcTemplate;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DaoService {

    @Autowired
    private DaoImp daoImp;

    public void batchInsert(List<Object[]> list){
        daoImp.batchInsert(list);
    }

    public void batchDelete(List<Object[]> list){
        daoImp.batchDelete(list);
    }

    public void batchUpdate(List<Object[]> list){
        daoImp.batchUpdate(list);
    }
}

```
```java
package com.linmu.jdbcTemplate;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.ArrayList;
import java.util.List;

public class Test06 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("sql.xml");
        DaoService daoService = classPathXmlApplicationContext.getBean("daoService", DaoService.class);
        List<Object[]> objects = new ArrayList<>();
        Object[] obj1 = {"31","jack","19"};
        Object[] obj2 = {"32","limi","39"};
        Object[] obj3 = {"33","milan","29"};
        objects.add(obj1);
        objects.add(obj2);
        objects.add(obj3);
        daoService.batchInsert(objects);
    }
}





package com.linmu.jdbcTemplate;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.ArrayList;
import java.util.List;

public class Test07 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("sql.xml");
        DaoService daoService = classPathXmlApplicationContext.getBean("daoService", DaoService.class);
        List<Object[]> list = new ArrayList<>();
        Object[]  o1 = {"1"};
        Object[]  o2 = {"2"};
        Object[]  o3 = {"3"};
        list.add(o1);
        list.add(o3);
        list.add(o2);
        daoService.batchDelete(list);
    }
}





package com.linmu.jdbcTemplate;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.ArrayList;

public class Test08 {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("sql.xml");
        DaoService daoService = classPathXmlApplicationContext.getBean("daoService", DaoService.class);
        ArrayList<Object[]> objects = new ArrayList<>();
        Object[] o1 = {"30","4"};
        Object[] o2 = {"36","5"};
        Object[] o3 = {"33","6"};
        objects.add(o1);
        objects.add(o2);
        objects.add(o3);
        daoService.batchUpdate(objects);
    }
}



```
# Spring 事务管理
## 事务管理简介

1. 事务添加到 JavaEE 三层结构里面 Service 层（业务逻辑层） 
2. 在 Spring 进行事务管理操作 
   1. 有两种方式：编程式事务管理和声明式事务管理（使用） 
3. 声明式事务管理 
   1. 基于注解方式（使用） 
   2. 基于 xml 配置文件方式 
4. 在 Spring 进行声明式事务管理，底层使用 AOP 原理
5. Spring 事务管理 API 
   1. 提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674649906572-4bcf9c2d-d46b-438c-af2e-076d3f7ad405.png#averageHue=%23f3f2f2&clientId=u999dde3d-3e89-4&from=paste&height=307&id=u0a42d93b&name=image.png&originHeight=614&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63194&status=done&style=shadow&taskId=ub1d44e93-7ba4-4ed2-897d-0bf2404f277&title=&width=545)
> 注意：添加spring事务，不能使用try-catch

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674650097750-b72df333-b53d-4fc9-84b2-8c4b444f8672.png#averageHue=%239aa5a4&clientId=u999dde3d-3e89-4&from=paste&height=559&id=u4ef03676&name=image.png&originHeight=1117&originWidth=1960&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191314&status=done&style=shadow&taskId=ub856d4a9-0d54-4c86-94e4-e947de69f11&title=&width=980)

## 事务操作-注解声明式事务管理

1. 在 spring 配置文件配置事务管理器
2. 在 spring 配置文件，开启事务注解 
   1. 在 spring 配置文件引入名称空间 tx
   2. 开启事务注解
3. 在 service 类上面（或者 service 类里面方法上面）添加事务注解 
   1. @Transactional，这个注解添加到类上面，也可以添加方法上面 
   2. 如果把这个注解添加类上面，这个类里面所有的方法都添加事务 
   3. 如果把这个注解添加方法上面，为这个方法添加事务
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

  <!--    开启组件扫描-->
  <context:component-scan base-package="com.linmu.operateDatabase"></context:component-scan>
  <!--    导入数据库配置-->
  <context:property-placeholder location="mysql.properties"></context:property-placeholder>
  <!--    连接数据库-->
  <!--    destroy-method="close" 表示关闭连接的方法-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="driverClassName" value="${mysql.driver}"></property>
    <property name="url" value="${mysql.url}"></property>
    <property name="username" value="${mysql.username}"></property>
    <property name="password" value="${mysql.password}"></property>
  </bean>
  <!--    bean注入-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--创建事务管理器-->
  <bean id="transactionManager"
    class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--开启事务注解,需要导入tx约束-->
  <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>


</beans>

```
```java
package com.linmu.operateDatabase.dao;

import com.linmu.operateDatabase.entity.User;

import java.util.List;

public interface Dao {

    int reduce(Integer id);

    int increase(Integer id);
}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;
import java.util.List;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int increase(Integer id) {
        String sql = "update counter set counter_money=counter_money+100 where counter_id=?";
        int result = jdbcTemplate.update(sql,id);
        return result;
    }
}

```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * @Service
 * bean注入
 * @Transactional
 * 开启事务
 */
@Service
@Transactional
public class ServiceDao {

    @Autowired
    private ImpDao impDao;

    public int changeMoney(int id1,int id2){
        int result = 0;
        result += impDao.reduce(id1);
        int a = 1/0;
        result += impDao.increase(id2);
        return result;
    }
}


```
```java
    @Test
    public void transcationManager(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        int result = serviceDao.changeMoney(1, 2);
        System.out.println(result);
    }
```

## 事务操作-声明式事务管理参数配置
> 在 service 类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652894045-32d99b3c-7354-444a-b90d-a4977931adf3.png#averageHue=%23fafaf8&clientId=u999dde3d-3e89-4&from=paste&height=111&id=u4a03558a&name=image.png&originHeight=221&originWidth=1293&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39167&status=done&style=shadow&taskId=u0e11fe33-5a98-4f84-a186-7e09e5aa40a&title=&width=646.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652274716-41d0a574-3822-4eb8-bd9b-cb9828ad2636.png#averageHue=%23fdfcfb&clientId=u999dde3d-3e89-4&from=paste&height=594&id=u7cec6d5c&name=image.png&originHeight=1188&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=159063&status=done&style=shadow&taskId=u09909b96-44f5-43fa-bb9f-177b877a5a3&title=&width=545)

2. propagation：事务传播行为 
   1. 多事务方法直接进行调用，这个过程中事务 是如何进行管理的

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652342838-f4b47f45-b905-4d9a-b15b-848d03fbad95.png#averageHue=%23fbfbfb&clientId=u999dde3d-3e89-4&from=paste&height=216&id=ua4cca98f&name=image.png&originHeight=432&originWidth=941&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36790&status=done&style=shadow&taskId=u0af1b147-f0ae-40a3-94b5-21900d0ead5&title=&width=470.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652363319-a54be7e8-65ea-4679-b729-076dc0a8b4ed.png#averageHue=%23f7f8f4&clientId=u999dde3d-3e89-4&from=paste&height=318&id=u668e189a&name=image.png&originHeight=457&originWidth=677&originalType=binary&ratio=1&rotation=0&showTitle=false&size=322148&status=done&style=shadow&taskId=u24a49daf-7e50-4dd5-b3f3-217e02d17b2&title=&width=470.5)

3. ioslation：事务隔离级别 
   1. 事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑隔离性产生很多问题 
   2. 有三个读问题：脏读、不可重复读、虚（幻）读
      1. 脏读：一个未提交事务读取到另一个未提交事务的数据

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652478223-9c44b70c-8ae4-44f0-8550-bca278e3aa57.png#averageHue=%23fefefe&clientId=u999dde3d-3e89-4&from=paste&height=152&id=u09375978&name=image.png&originHeight=304&originWidth=795&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12555&status=done&style=shadow&taskId=u7103a9af-423b-4d40-bff8-dffd3e82253&title=&width=397.5)

      2. 不可重复读：一个未提交事务读取到另一提交事务修改数据

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652541418-0e00b559-db7b-45ba-ae90-de52b0556878.png#averageHue=%23fffefe&clientId=u999dde3d-3e89-4&from=paste&height=158&id=ue864fcbf&name=image.png&originHeight=316&originWidth=895&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14092&status=done&style=shadow&taskId=u5f304d25-ea31-4a55-89d8-4b3c51b6cc1&title=&width=447.5)

      3. 虚读：一个未提交事务读取到另一提交事务添加数据
   3. 解决：通过设置事务隔离级别，解决读问题	

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674652584373-43286170-afd0-429f-98e6-f7942e31c7bf.png#averageHue=%23f0eeec&clientId=u999dde3d-3e89-4&from=paste&height=134&id=uc31da804&name=image.png&originHeight=267&originWidth=583&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18886&status=done&style=shadow&taskId=u17556ec0-4112-4cc0-a4aa-f994b124a53&title=&width=291.5)

4. timeout：超时时间 
   1. 事务需要在一定时间内进行提交，如果不提交进行回滚 
   2. 默认值是 -1 ，设置时间以秒单位进行计算 
5. readOnly：是否只读 
   1. 读：查询操作，写：添加修改删除操作 
   2. readOnly 默认值 false，表示可以查询，可以添加修改删除操作 
   3. 设置 readOnly 值是 true，设置成 true 之后，只能查询 
6. rollbackFor：回滚 
   1. 设置出现哪些异常进行事务回滚 
7. noRollbackFor：不回滚 
   1. 设置出现哪些异常不进行事务回滚

## 事务操作-xml配置文件声明式事务管理(取消注解事务)

1. 在 spring 配置文件中进行配置 

第一步 配置事务管理器 
第二步 配置通知
第三步 配置切入点和切面
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

  <!--    开启组件扫描-->
  <context:component-scan base-package="com.linmu.operateDatabase"></context:component-scan>
  <!--    导入数据库配置-->
  <context:property-placeholder location="mysql.properties"></context:property-placeholder>
  <!--    连接数据库-->
  <!--    destroy-method="close" 表示关闭连接的方法-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="driverClassName" value="${mysql.driver}"></property>
    <property name="url" value="${mysql.url}"></property>
    <property name="username" value="${mysql.username}"></property>
    <property name="password" value="${mysql.password}"></property>
  </bean>
  <!--    bean注入-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--创建事务管理器-->
  <bean id="transactionManager"
    class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--注入数据源-->
    <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--开启事务注解,需要导入tx约束-->
  <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
	<!--    配置通知-->
    <tx:advice id="notification">
        <tx:attributes>
<!--            支持通配符，可配置事务参数-->
            <tx:method name="changeMoney" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
<!--    aop配置-->
    <aop:config>
<!--        配置切入点-->
        <aop:pointcut id="pc" expression="execution(* com.linmu.operateDatabase.service.ServiceDao.changeMoney(..))"/>
<!--        切面配置-->
        <aop:advisor advice-ref="notification" pointcut-ref="pc"></aop:advisor>
    </aop:config>

</beans>

```
```java
package com.linmu.operateDatabase.dao;

import com.linmu.operateDatabase.entity.User;

import java.util.List;

public interface Dao {

    int reduce(Integer id);

    int increase(Integer id);
}

```
```java
package com.linmu.operateDatabase.imp;

import com.linmu.operateDatabase.dao.Dao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;
import java.util.List;

@Component
public class ImpDao implements Dao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int increase(Integer id) {
        String sql = "update counter set counter_money=counter_money+100 where counter_id=?";
        int result = jdbcTemplate.update(sql,id);
        return result;
    }
}

```
```java
package com.linmu.operateDatabase.service;

import com.linmu.operateDatabase.imp.ImpDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * @Service
 * bean注入
 * @Transactional
 * 开启事务
 */
@Service
// @Transactional  配置文件取代该注解
public class ServiceDao {

    @Autowired
    private ImpDao impDao;

    public int changeMoney(int id1,int id2){
        int result = 0;
        result += impDao.reduce(id1);
        int a = 1/0;
        result += impDao.increase(id2);
        return result;
    }
}


```
```java
    @Test
    public void transcationManager(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("database.xml");
        ServiceDao serviceDao = classPathXmlApplicationContext.getBean("serviceDao", ServiceDao.class);
        int result = serviceDao.changeMoney(1, 2);
        System.out.println(result);
    }
```

# spring5特性
## log4j-2日志整合

1. 整个 Spring5 框架的代码基于 Java8，运行时兼容 JDK9，许多不建议使用的类和方法在代码库中删除
2. Spring 5.0 框架自带了通用的日志封装 
   1. Spring5 已经移除 Log4jConfigListener，官方建议使用 Log4j2 
   2. Spring5 框架整合 Log4j2 

第一步 引入 jar 包 
第二步 创建 log4j2.xml 配置文件
```xml
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-core</artifactId>
  <version>2.17.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-api -->
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-api</artifactId>
  <version>2.17.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>2.0.3</version>
</dependency>
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE >
ALL -->
<!--Configuration 后面的 status 用于设置 log4j2 自身内部的信息输出，可以不设置，
当设置成 trace 时，可以看到 log4j2 内部各种详细输出-->
<configuration status="INFO">
    <!--先定义所有的 appender-->
    <appenders>
        <!--输出日志信息到控制台-->
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </console>
    </appenders>
    <!--然后定义 logger，只有定义 logger 并引入的 appender，appender 才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定 Logger，则会使用 root 作为
   默认的日志输出-->
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

## @Nullable注解与对象注册

1. Spring5 框架核心容器支持@Nullable 注解 
   1. @Nullable 注解可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以为空，参数值可以为空 
   2. 注解用在方法上面，方法返回值可以为空 
   3. 注解使用在方法参数里面，方法参数可以为空 
   4. 注解使用在属性上面，属性值可以为空

![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674696818672-80066a25-acd7-4434-b881-915dd24e2202.png#averageHue=%23fcfbf9&from=url&height=90&id=Gcc0h&originHeight=117&originWidth=623&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=&width=479)
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674696845854-39f8a0cb-b1d9-460d-8c3a-2bbe1884b114.png#averageHue=%23faf9f8&from=url&id=JjTOD&originHeight=142&originWidth=2026&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)
![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674696854734-0c06793e-6ecd-45f1-a7f4-befa63e6d9c6.png#averageHue=%23fdfcfb&from=url&id=TqGJp&originHeight=114&originWidth=1228&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)

2. Spring5 核心容器支持函数式风格 GenericApplicationContext
```java
//函数式风格创建对象，交给 spring 进行管理 
@Test 
    public void testGenericApplicationContext() { 
    //1 创建 GenericApplicationContext 对象 
    GenericApplicationContext context = new GenericApplicationContext(); 
    //2 调用 context 的方法对象注册 
    context.refresh(); 
    context.registerBean("user1",User.class,() -> new User()); 
    //3 获取在 spring 注册的对象 
    // User user = (User)context.getBean("com.atguigu.spring5.test.User"); 
    User user = (User)context.getBean("user1"); 
    System.out.println(user); 
}
```

## 测试框架整合

1. 整合 JUnit4 

第一步 引入 Spring 相关针对测试依赖
第二步 创建测试类，使用注解方式完成
```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.4</version>
</dependency>

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>

<!-- 二选一 -->

<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.4</version>
</dependency>

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>

```
```java
package com.linmu.transaction;


import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.annotation.Resource;

@RunWith(SpringJUnit4ClassRunner.class)//单元测试框架
@ContextConfiguration("classpath:transactionconf.xml")//加载配置文件
public class Test03 {

    // @Resource是JDK提供的，通过name名字查找；而Autowired是Spring提供的，通过type类型查找。
    @Resource
    private DaoService daoService;

    @Test
    public void test(){
        daoService.acountChange();
    }
}
```

2. Spring5 整合 JUnit5 

第一步 引入 JUnit5 的 jar 包
第二步 创建测试类，使用注解完成
```xml
<!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.7.0</version>
</dependency>

```
```java
package com.linmu.transaction;


import com.linmu.tancon.DaoService;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import javax.annotation.Resource;

@ExtendWith(SpringExtension.class)
@ContextConfiguration("classpath:transactionconf.xml")
public class Test04 {

    @Resource
    private DaoService daoService;

    @Test
    public void prin(){
        daoService.acountChange();
    }
}
```

3. 使用一个复合注解替代上面两个注解完成整合
```java
package com.linmu.transaction;

import com.linmu.tancon.DaoService;
import org.junit.jupiter.api.Test;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

import javax.annotation.Resource;

@SpringJUnitConfig(locations = "classpath:transaction.xml")
public class test05 {

    @Resource
    private DaoService daoService;

    @Test
    public void test1() {
        daoService.acountChange();
    }
}

```
## WebFlux
### SpringWebflux 介绍 （
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674706283472-c3f51810-61f2-4187-8005-afbdca64ff26.png#averageHue=%235fba10&clientId=u8a8b9022-1a5d-4&from=paste&height=400&id=u3df182d8&name=image.png&originHeight=800&originWidth=1232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=126514&status=done&style=shadow&taskId=ub545fc4d-5649-4b9a-862a-2a4a5332851&title=&width=616)
（1）是 Spring5 添加新的模块，用于 web 开发的，功能和 SpringMVC 类似的，Webflux 使用 
当前一种比较流程响应式编程出现的框架。
（2）使用传统 web 框架，比如 SpringMVC，这些基于 Servlet 容器，Webflux 是一种异步非阻 
塞的框架，异步非阻塞的框架在 Servlet3.1 以后才支持，核心是基于 Reactor 的相关 API 实现 
的。 
（3）解释什么是异步非阻塞 
* 异步和同步 
* 非阻塞和阻塞 
** 上面都是针对对象不一样 
** 异步和同步针对调用者，调用者发送请求，如果等着对方回应之后才去做其他事情就是同 
步，如果发送请求之后不等着对方回应就去做其他事情就是异步 
** 阻塞和非阻塞针对被调用者，被调用者受到请求之后，做完请求任务之后才给出反馈就是阻 
塞，受到请求之后马上给出反馈然后再去做事情就是非阻塞 
（4）Webflux 特点： 
第一 非阻塞式：在有限资源下，提高系统吞吐量和伸缩性，以 Reactor 为基础实现响应式编程 
第二 函数式编程：Spring5 框架基于 java8，Webflux 使用 Java8 函数式编程方式实现路由请求 
（5）比较 SpringMVC
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674706369980-55db66cc-2de9-4a02-8cd6-94a5d1822392.png#averageHue=%23d6e4ef&clientId=u8a8b9022-1a5d-4&from=paste&height=228&id=uaad9f9ee&name=image.png&originHeight=456&originWidth=811&originalType=binary&ratio=1&rotation=0&showTitle=false&size=291054&status=done&style=shadow&taskId=ue0936ea5-7769-4e4b-9a9f-7e9871649eb&title=&width=405.5)
第一 两个框架都可以使用注解方式，都运行在 Tomet 等容器中
第二 SpringMVC 采用命令式编程，Webflux 采用异步响应式编程

### 响应式编程（Java 实现） 

1. 什么是响应式编程 

响应式编程是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便 
地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。 
电子表格程序就是响应式编程的一个例子。单元格可以包含字面值或类似"=B1+C1"的公 
式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。 

2. Java8 及其之前版本 

* 提供的观察者模式两个类 Observer 和 Observable

```java
public class ObserverDemo extends Observable {
    public static void main(String[] args) {
        ObserverDemo observer = new ObserverDemo();
        //添加观察者
        observer.addObserver((o,arg)->{
            System.out.println("发生变化");
        });
        observer.addObserver((o,arg)->{
            System.out.println("手动被观察者通知，准备改变");
        });
        observer.setChanged(); //数据变化
        observer.notifyObservers(); //通知
    }
}
```

### 响应式编程（Reactor 实现） 
（1）响应式编程操作中，Reactor 是满足 Reactive 规范框架 
（2）Reactor 有两个核心类，Mono 和 Flux，这两个类实现接口 Publisher，提供丰富操作 
  符。Flux 对象实现发布者，返回 N 个元素；Mono 实现发布者，返回 0 或者 1 个元素 
（3）Flux 和 Mono 都是数据流的发布者，使用 Flux 和 Mono 都可以发出三种数据信号： 
元素值，错误信号，完成信号，错误信号和完成信号都代表终止信号，终止信号用于告诉 
订阅者数据流结束了，错误信号终止数据流同时把错误信息传递给订阅者

![](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674706554046-448f26cc-fc02-4338-b124-c18dca664bf8.png#averageHue=%23f6f6f6&from=url&id=A9WjX&originHeight=491&originWidth=845&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=shadow&title=)

(4) 代码演示 Flux 和 Mono
版本问题，有些地方需要修改
```xml
<dependency>
  <groupId>io.projectreactor</groupId>
  <artifactId>reactor-core</artifactId>
  <version>3.1.5.RELEASE</version>
</dependency>
```
```java
package com.example.demo;

import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class Demo {
    public static void main(String[] args) {
        //just 方法直接声明
        Flux.just(1,2,3);
        Mono.just(1);

        // 数组
        Integer[] array = {1,2,3};
        Flux.fromArray(array);

        // 集合
        List<Integer> list = Arrays.asList(array);
        Flux.fromIterable(list);

        // 数据流
        Stream<Integer> stream = list.stream();
        Flux.fromStream(stream);
    }
}

```

（5）三种信号特点 
* 错误信号和完成信号都是终止信号，不能共存的 
* 如果没有发送任何元素值，而是直接发送错误或者完成信号，表示是空数据流 
* 如果没有错误信号，没有完成信号，表示是无限数据流 
（6）调用 just 或者其他方法只是声明数据流，数据流并没有发出，只有进行订阅之后才会触 
发数据流，不订阅什么都不会发生的
```java
package com.example.demo;

import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class Demo {
    public static void main(String[] args) {
        //just 方法直接声明,订阅
        Flux.just(1,2,3).subscribe(System.out::print);
        Mono.just(1).subscribe(System.out::print);

        // 数组，订阅
        Integer[] array = {1,2,3};
        Flux.fromArray(array).subscribe(System.out::print);

        // 集合，订阅
        List<Integer> list = Arrays.asList(array);
        Flux.fromIterable(list).subscribe(System.out::print);

        // 数据流，订阅
        Stream<Integer> stream = list.stream();
        Flux.fromStream(stream).subscribe(System.out::print);
    }
}
```

（7）操作符 
* 对数据流进行一道道操作，成为操作符，比如工厂流水线 
第一 map 元素映射为新元素
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674708714507-bb16d2b9-16ce-4ff0-8ebe-c3a0cede4c8c.png#averageHue=%23f7f6f6&clientId=u8a8b9022-1a5d-4&from=paste&height=308&id=u5248d48e&name=image.png&originHeight=355&originWidth=606&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37180&status=done&style=shadow&taskId=u566569f2-64ba-4bf6-ab96-333d119fc3d&title=&width=525)
第二 flatMap 元素映射为流     把每个元素转换流，把转换之后多个流合并大的流
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1674708728709-d47a789f-4bcf-456c-94d9-cbffbf799a5e.png#averageHue=%23f3f3f3&clientId=u8a8b9022-1a5d-4&from=paste&height=334&id=u3e9c6bc7&name=image.png&originHeight=359&originWidth=573&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44169&status=done&style=shadow&taskId=u893ec068-f264-429f-8bfa-3ffbc95e1ce&title=&width=533.5)
### SpringWebflux 执行流程和核心 API 
SpringWebflux 基于 Reactor，默认使用容器是 Netty，Netty 是高性能的 NIO 框架，异步非阻塞的框架 
> netty编程：[https://www.bilibili.com/video/BV1DJ411m7NR/?spm_id_from=333.337.search-card.all.click&vd_source=862fa654e0bb61d4820baec598075496](https://www.bilibili.com/video/BV1DJ411m7NR/?spm_id_from=333.337.search-card.all.click&vd_source=862fa654e0bb61d4820baec598075496)

#### Netty
BIO
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675996761934-9eaa8cf9-1065-4cc5-b01d-9f63fc1e7801.png#averageHue=%23f1ece8&clientId=u58128aa1-a12e-4&from=paste&height=356&id=ud29b8859&name=image.png&originHeight=330&originWidth=628&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33345&status=done&style=shadow&taskId=u2e752119-40f2-477e-92c4-500e30ee23a&title=&width=677)
NIO
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675996782194-4192fb20-9a22-491f-ad04-3450b1aa45c4.png#averageHue=%23e4d2b3&clientId=u58128aa1-a12e-4&from=paste&height=432&id=u31bd5101&name=image.png&originHeight=490&originWidth=769&originalType=binary&ratio=2&rotation=0&showTitle=false&size=150802&status=done&style=shadow&taskId=ue5aa6111-e74f-4cf6-a477-f2a51af7c8e&title=&width=678.5)
#### SpringWebflux 执行过程和 SpringMVC 相似的 
* SpringWebflux 核心控制器 DispatchHandler，实现接口 WebHandler 
* 接口 WebHandler 有一个方法
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675996857505-70d05fc0-3676-4251-90ae-39aec980ae6c.png#averageHue=%23fbf9f8&clientId=u58128aa1-a12e-4&from=paste&height=133&id=u6e2acef4&name=image.png&originHeight=85&originWidth=435&originalType=binary&ratio=2&rotation=0&showTitle=false&size=5288&status=done&style=shadow&taskId=u17ae2bb2-e67c-4c67-8100-23c0d30f577&title=&width=678.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675996864763-7ed5099a-9854-4a46-9155-e65ea315fcc3.png#averageHue=%23fcf8f4&clientId=u58128aa1-a12e-4&from=paste&height=204&id=u5fe39886&name=image.png&originHeight=283&originWidth=942&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29344&status=done&style=shadow&taskId=u22dfbc58-5ece-474b-8f9a-9fce8b95146&title=&width=678)
#### SpringWebflux 里面 DispatcherHandler，负责请求的处理 
* HandlerMapping：请求查询到处理的方法 
* HandlerAdapter：真正负责请求处理 
* HandlerResultHandler：响应结果处理 
#### SpringWebflux 实现函数式编程，两个接口：RouterFunction（路由处理） 
和 HandlerFunction（处理函数） 
#### SpringWebflux（基于注解编程模型） 
SpringWebflux 实现方式有两种：注解编程模型和函数式编程模型 
使用注解编程模型方式，和之前 SpringMVC 使用相似的，只需要把相关依赖配置到项目中， 
SpringBoot 自动配置相关运行容器，默认情况下使用 Netty 服务器

第一步 创建 SpringBoot 工程，引入 Webflux 依赖
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675997137121-2efe468c-a012-47c0-a71f-dc771698d176.png#averageHue=%23faf9f8&clientId=u58128aa1-a12e-4&from=paste&height=75&id=u9cfdfff0&name=image.png&originHeight=150&originWidth=258&originalType=binary&ratio=2&rotation=0&showTitle=false&size=4867&status=done&style=shadow&taskId=u2088d535-45e2-4e5f-977a-0edabc3f513&title=&width=129)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675997143649-aa1bebc5-4dd0-4e60-b60f-b249d0ad097c.png#averageHue=%23f9f7f2&clientId=u58128aa1-a12e-4&from=paste&height=60&id=u35bde8a1&name=image.png&originHeight=120&originWidth=528&originalType=binary&ratio=2&rotation=0&showTitle=false&size=8579&status=done&style=shadow&taskId=u67cd324f-33f3-4dc8-adfc-b81790b5171&title=&width=264)
第二步 配置启动端口号
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675997112823-65da6c85-4dfa-4b11-8e30-1d30fd781453.png#averageHue=%23f4f1e6&clientId=u58128aa1-a12e-4&from=paste&height=75&id=ud0de53c7&name=image.png&originHeight=59&originWidth=294&originalType=binary&ratio=2&rotation=0&showTitle=false&size=2882&status=done&style=shadow&taskId=uff889b47-3632-4370-a5bc-42fa842f61a&title=&width=376)
第三步 创建包和相关类 
实体类
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675997093924-95b5825f-cf42-4375-b14d-1dc358a9fe5b.png#averageHue=%23fdfcfa&clientId=u58128aa1-a12e-4&from=paste&height=472&id=ubb9a3e69&name=image.png&originHeight=352&originWidth=555&originalType=binary&ratio=2&rotation=0&showTitle=false&size=18777&status=done&style=shadow&taskId=uea0925c8-4521-45e8-9fe5-d5721295f00&title=&width=743.5)
创建接口定义操作的方法
```java
//用户操作接口
public interface UserService {
 //根据 id 查询用户
 Mono<User> getUserById(int id);
 //查询所有用户
 Flux<User> getAllUser();
 //添加用户
 Mono<Void> saveUserInfo(Mono<User> user);
}
```
接口实现类
```java
public class UserServiceImpl implements UserService {
 //创建 map 集合存储数据
 private final Map<Integer,User> users = new HashMap<>();
 public UserServiceImpl() {
 this.users.put(1,new User("lucy","nan",20));
 this.users.put(2,new User("mary","nv",30));
 this.users.put(3,new User("jack","nv",50));
 }
 //根据 id 查询
 @Override
 public Mono<User> getUserById(int id) {
 return Mono.justOrEmpty(this.users.get(id));
 }
 //查询多个用户
 @Override
 public Flux<User> getAllUser() {
 return Flux.fromIterable(this.users.values());
 }
 //添加用户
 @Override
 public Mono<Void> saveUserInfo(Mono<User> userMono) {
 return userMono.doOnNext(person -> {
 //向 map 集合里面放值
 int id = users.size()+1;
 users.put(id,person);
 }).thenEmpty(Mono.empty());
 }
}
```
创建 controller
```java
@RestController
public class UserController {
 //注入 service
 @Autowired
 private UserService userService;
 //id 查询
 @GetMapping("/user/{id}")
 public Mono<User> geetUserId(@PathVariable int id) {
 return userService.getUserById(id);
 }
 //查询所有
 @GetMapping("/user")
 public Flux<User> getUsers() {
 return userService.getAllUser();
 }
 //添加
 @PostMapping("/saveuser")
 public Mono<Void> saveUser(@RequestBody User user) {
 Mono<User> userMono = Mono.just(user);
 return userService.saveUserInfo(userMono);
 }
}
```
**说明 **
**SpringMVC 方式实现，同步阻塞的方式，基于 SpringMVC+Servlet+Tomcat **
**SpringWebflux 方式实现，异步非阻塞 方式，基于 SpringWebflux+Reactor+Netty**

#### SpringWebflux（基于函数式编程模型） 
（1）在使用函数式编程模型操作时候，需要自己初始化服务器 
（2）基于函数式编程模型时候，有两个核心接口：RouterFunction（实现路由功能，请求转发 
给对应的 handler）和 HandlerFunction（处理请求生成响应的函数）。核心任务定义两个函数 
式接口的实现并且启动需要的服务器。 
（ 3 ） SpringWebflux 请 求 和 响 应 不 再 是 ServletRequest 和 ServletResponse ，而是 
ServerRequest 和 ServerResponse

第一步 把注解编程模型工程复制一份 ，保留 entity 和 service 内容 
第二步 创建 Handler（具体实现方法）
```java
public class UserHandler {
    private final UserService userService;
    public UserHandler(UserService userService) {
        this.userService = userService;
    }
    //根据 id 查询
    public Mono<ServerResponse> getUserById(ServerRequest request) {
        //获取 id 值
        int userId = Integer.valueOf(request.pathVariable("id"));
        //空值处理
        Mono<ServerResponse> notFound = ServerResponse.notFound().build();
        //调用 service 方法得到数据
        Mono<User> userMono = this.userService.getUserById(userId);
        //把 userMono 进行转换返回
        //使用 Reactor 操作符 flatMap
        return 
            userMono
            .flatMap(person -> 
                     ServerResponse.ok().contentType(MediaType.APPLICATION_JSON)
                     .body(fromObject(person)))
            .switchIfEmpty(notFound);
    }
    //查询所有
    public Mono<ServerResponse> getAllUsers() {
        //调用 service 得到结果
        Flux<User> users = this.userService.getAllUser();
        return 
            ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body(users,User.cl
                                                                             ass);
    }
    //添加
    public Mono<ServerResponse> saveUser(ServerRequest request) {
        //得到 user 对象
        Mono<User> userMono = request.bodyToMono(User.class);
        return 
            ServerResponse.ok().build(this.userService.saveUserInfo(userMono));
    }
}
```

第三步 初始化服务器，编写 Router 
创建路由的方法
```java
1 创建 Router 路由 
    public RouterFunction<ServerResponse> routingFunction() { 
        //创建 hanler 对象 
        UserService userService = new UserServiceImpl(); 
        UserHandler handler = new UserHandler(userService); 
        //设置路由 
        return RouterFunctions.route( 
            GET("/users/{id}").and(accept(APPLICATION_JSON)),handler::getUserById) 
            .andRoute(GET("/users").and(accept(APPLICATION_JSON)),handler::get 
                      AllUsers); 
    }
```
 创建服务器完成适配 
```java
//2 创建服务器完成适配 
public void createReactorServer() { 
//路由和 handler 适配 
RouterFunction<ServerResponse> route = routingFunction(); 
 HttpHandler httpHandler = toHttpHandler(route); 
 ReactorHttpHandlerAdapter adapter = new 
ReactorHttpHandlerAdapter(httpHandler); 
//创建服务器 
HttpServer httpServer = HttpServer.create(); 
 httpServer.handle(adapter).bindNow(); 
} 
```
最终调用 
```java
public static void main(String[] args) throws Exception{ 
 Server server = new Server(); 
 server.createReactorServer(); 
 System.out.println("enter to exit"); 
 System.in.read(); 
} 
```
（4）使用 WebClient 调用 
```java
ublic class Client { 
    public static void main(String[] args) { 
        //调用服务器地址 
        WebClient webClient = WebClient.create("http://127.0.0.1:5794"); 
        //根据 id 查询 
        String id = "1"; 
        User userresult = webClient.get().uri("/users/{id}", id) 
            .accept(MediaType.APPLICATION_JSON).retrieve().bodyToMono(User 
                                                                      .class) 
            .block(); 
        System.out.println(userresult.getName()); 
        //查询所有 
        Flux<User> results = webClient.get().uri("/users") 
            .accept(MediaType.APPLICATION_JSON).retrieve().bodyToFlux(User 
                                                                      .class); 
        results.map(stu -> stu.getName()) 
            .buffer().doOnNext(System.out::println).blockFirst(); 
    } 
}
```

# 小结
1、Spring 框架概述 
（1）轻量级开源 JavaEE 框架，为了解决企业复杂性，两个核心组成：IOC 和 AOP 
（2）Spring5.2.6 版本 
2、IOC 容器
（1）IOC 底层原理（工厂、反射等） 
（2）IOC 接口（BeanFactory） 
（3）IOC 操作 Bean 管理（基于 xml） 
（4）IOC 操作 Bean 管理（基于注解） 
3、Aop 
（1）AOP 底层原理：动态代理，有接口（JDK 动态代理），没有接口（CGLIB 动态代理） 
（2）术语：切入点、增强（通知）、切面 
（3）基于 AspectJ 实现 AOP 操作 
4、JdbcTemplate 
（1）使用 JdbcTemplate 实现数据库 curd 操作 
（2）使用 JdbcTemplate 实现数据库批量操作 
5、事务管理 
（1）事务概念 
（2）重要概念（传播行为和隔离级别） 
（3）基于注解实现声明式事务管理 
（4）完全注解方式实现声明式事务管理 
6、Spring5 新功能 
（1）整合日志框架 
（2）@Nullable 注解 
（3）函数式注册对象 
（4）整合 JUnit5 单元测试框架 
（5）SpringWebflux 使用
