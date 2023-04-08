# 快速入门
1. 创建父工程
```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.5.14</version>
</parent>
```

2. 添加web依赖
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

3. 编写代码
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
// 该注解可通过用户导入的jar包，自动配置spring
@EnableAutoConfiguration
public class MyApplication {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}


```

4. 启动项目
> 可直接运行main方法

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1678793277960-00d01774-59e0-4fa6-9da8-0c38a053e36e.png#averageHue=%23c8d195&clientId=u7adc74d7-2937-4&from=paste&height=350&id=udca43f8d&name=image.png&originHeight=700&originWidth=1685&originalType=binary&ratio=2&rotation=0&showTitle=false&size=328234&status=done&style=shadow&taskId=ue641de14-711e-42b7-a3b7-d2e660dc836&title=&width=842.5)

5. 添加打包依赖
```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```
# sprint boot 特点
## 依赖管理

- 父项目管理
```xml
<!-- 依赖管理     -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.9</version>
  </parent>

<!-- 他的父项目 -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.9</version>
  </parent>

<!-- 几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制 -->

```

- 开发导入starter场景启动器
```xml
1、见到很多 spring-boot-starter-* ： *就某种场景
2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
3、SpringBoot所有支持的场景
https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
5、所有场景启动器最底层的依赖
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>2.7.9</version>
  </dependency>
</dependencies>
```

- 无需关心版本号，会自动识别
```xml
1、引入依赖默认都可以不写版本
2、引入非版本仲裁的jar，要写版本号。
```

- 可以修改默认版本号
```xml
1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
2、在当前项目里面重写配置
<properties>
  <mysql.version>5.1.43</mysql.version>
</properties>
```
## 自动配置

- 自动配好Tomcat
   - 引入Tomcat依赖。
   - 配置Tomcat
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <version>2.7.9</version>
</dependency>
```

- 自动配好SpringMVC
   - 引入SpringMVC全套组件
   - 自动配好SpringMVC常用组件（功能）
- 自动配好Web常见功能，如：字符编码问题
   - SpringBoot帮我们配置好了所有web开发的常见场景
- 默认的包结构
   - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
   - 无需以前的包扫描配置
   - 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.linmu"**)

- 各种配置拥有默认值
   - 默认配置最终都是映射到某个类上，如：MultipartProperties
   - 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象
- 按需加载所有自动配置项
   - 非常多的starter
   - 引入了哪些场景这个场景的自动配置才会开启
   - SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面
# 容器功能
## 添加组件
### @Configuration
```java
package com.linmu.config;

import com.linmu.bean.Student;
import com.linmu.bean.Teacher;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/16 18:35:41
**/
// 表示配置类
@Configuration(proxyBeanMethods = false)
public class MyConfig {

    /**
    * @Bean： 向容器中添加组件
    * 方法名：为组件的ID
    * 返回类型：组件的类型
    * 返回值：组件在容器中的实例
    * @return
    */
    @Bean("s1") // student01的别名
    public Student student01(){
        return new Student("Jason Black",21);
    }

    @Bean()
    public Teacher teacher01(){
        return new Teacher("Jackson Black",30000);
    }
}

```
### @Bean、@Component、@Controller、@Service、@Repository
分别是：将组件添加到容器中、将组件添加到容器中、控制层组件、服务层组件、业务层组件
### @ComponentScan、@Import
```java
package com.linmu.main;

import com.linmu.bean.Student;
import com.linmu.bean.Teacher;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Import;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

// 给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
@Import({Student.class,Teacher.class})
@RestController
@SpringBootApplication(scanBasePackages = "com.linmu")
public class App {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) {
        // 返回IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(App.class, args);

        // 查看容器里面的组件
        String[] beanDefinitionNames = run.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }

        // 通过getbean()的方式获取指定组件
        // 注意配置类也是一个组件
        Student s1 = run.getBean("s1", Student.class);
        Student s2 = run.getBean("s1", Student.class);
        System.out.println(s1.equals(s2));
        System.out.println(run.getBean("teacher01", Teacher.class).
                equals(run.getBean("teacher01",Teacher.class)));

        System.out.println("=====");

        String[] st = run.getBeanNamesForType(Student.class);
        for (String s : st) {
            System.out.println(s);
        }
        String[] th = run.getBeanNamesForType(Teacher.class);
        for (String s : th) {
            System.out.println(s);
        }
    }
}
```
### @Conditional
条件装配：满足Conditional指定的条件，则进行组件注入
![image (2).png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679037058415-42b318cf-1157-4220-b757-044059a971a0.png#averageHue=%23faf9db&clientId=u7b239922-7e29-4&from=paste&height=673&id=u82e6b36e&name=image%20%282%29.png&originHeight=544&originWidth=599&originalType=binary&ratio=2&rotation=0&showTitle=false&size=81444&status=done&style=shadow&taskId=u90e4640f-1d37-4d7b-8ac9-8c03b86bc6e&title=&width=741.5)
> 满足条件时才会执行执行配置类中的代码
> @ConditionalOnMissBean注解可以标识类也可以标识方法
> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679037672512-181e8682-3b16-4c24-ad70-e12952556d04.png#averageHue=%23faf9f8&clientId=u7b239922-7e29-4&from=paste&height=209&id=u5d7393cc&name=image.png&originHeight=418&originWidth=1202&originalType=binary&ratio=2&rotation=0&showTitle=false&size=72330&status=done&style=shadow&taskId=ued639dfe-8e45-4bb7-aafe-e263c4b9a21&title=&width=601)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679037532049-09144c16-4a38-4a9d-aed3-99f527a756e5.png#averageHue=%23faf9f8&clientId=u7b239922-7e29-4&from=paste&height=95&id=u9c25f3dd&name=image.png&originHeight=190&originWidth=1124&originalType=binary&ratio=2&rotation=0&showTitle=false&size=36209&status=done&style=shadow&taskId=ud43cd4c1-b79f-40ab-af30-35a99df9089&title=&width=562)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679037543879-b7d1b556-e629-43dd-9722-f2695089ec0e.png#averageHue=%23fafaf9&clientId=u7b239922-7e29-4&from=paste&height=150&id=u245c3a5b&name=image.png&originHeight=299&originWidth=1334&originalType=binary&ratio=2&rotation=0&showTitle=false&size=46790&status=done&style=shadow&taskId=u38e20520-a037-4073-b755-3d98a2f0946&title=&width=667)
## 原生配置文件引入
### @ImportResource
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679038220980-c0268e6c-8403-4466-9972-a165ca0c1d9f.png#averageHue=%23fcf6f2&clientId=u7b239922-7e29-4&from=paste&height=191&id=ue3366874&name=image.png&originHeight=381&originWidth=1043&originalType=binary&ratio=2&rotation=0&showTitle=false&size=58714&status=done&style=shadow&taskId=ud81c4dbb-073f-44ea-b1df-403134576fb&title=&width=521.5)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  <bean id="student02" class="com.linmu.bean.Student">
    <property name="name" value="Jackson"></property>
    <property name="age" value="22"></property>
  </bean>
  <bean id="teacher02" class="com.linmu.bean.Teacher">
    <property name="name" value="Black"></property>
    <property name="salar" value="40000"></property>
  </bean>
</beans>
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679038314937-403e9996-60c9-48a2-b22e-444b02c7ba25.png#averageHue=%23f9f9f8&clientId=u7b239922-7e29-4&from=paste&height=112&id=u8b037596&name=image.png&originHeight=224&originWidth=1573&originalType=binary&ratio=2&rotation=0&showTitle=false&size=50963&status=done&style=shadow&taskId=u21bb9b76-2e78-43c1-a7f1-7a49ab86458&title=&width=786.5)
## 配置绑定
如何使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用；

默认绑定application.properties配置文件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679040201550-b2a6c432-6e03-45b8-9298-cd078cf7c32f.png#averageHue=%23fbf4f0&clientId=u7b239922-7e29-4&from=paste&height=199&id=u417a640c&name=image.png&originHeight=397&originWidth=1477&originalType=binary&ratio=2&rotation=0&showTitle=false&size=48207&status=done&style=shadow&taskId=uc2e6764b-cdbe-4ff8-b23c-7bf8582ad20&title=&width=738.5)

方式一：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679039751771-73fd0c9c-e76f-4f80-87b2-2f266849c719.png#averageHue=%23fce9e8&clientId=u7b239922-7e29-4&from=paste&height=240&id=uc8ee4616&name=image.png&originHeight=480&originWidth=1362&originalType=binary&ratio=2&rotation=0&showTitle=false&size=89441&status=done&style=shadow&taskId=u688b169f-0ecc-41ee-a5ed-9988920f1f4&title=&width=681)
方式二：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679039808844-ce919974-8d0a-4e61-a437-435301075434.png#averageHue=%23fcf3f2&clientId=u7b239922-7e29-4&from=paste&height=202&id=ub3bb1e16&name=image.png&originHeight=403&originWidth=1459&originalType=binary&ratio=2&rotation=0&showTitle=false&size=67689&status=done&style=shadow&taskId=ud8f587b1-43b3-46c6-90af-3d2ce1144c5&title=&width=729.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679040070598-780c6c62-3f3a-420f-9682-9868d9bfbaa2.png#averageHue=%23fcedec&clientId=u7b239922-7e29-4&from=paste&height=205&id=u7692c208&name=image.png&originHeight=409&originWidth=1497&originalType=binary&ratio=2&rotation=0&showTitle=false&size=79138&status=done&style=shadow&taskId=ub3a2179d-fb8b-49ec-a087-0dced929c03&title=&width=748.5)
```java

package com.linmu.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/17 15:38:16
**/
@SuppressWarnings({"all"})
// 只有将组件添加到容器中才能使用springboot中的强大功能
@Component  // 将组件添加到容器中
@ConfigurationProperties(prefix = "cat")
public class Cat {

    private String name;
    private int age;

    public Cat(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Cat() {
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

    @Override
    public String  toString() {
        return "Cat{" +
            "name='" + name + '\'' +
            ", age=" + age +
            '}';
    }
}






package com.linmu.contorller;

import com.linmu.bean.Cat;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/3/17 15:45:47
 **/
@SuppressWarnings({"all"})
@RestController // 标识处理请求的控制器
// @EnableConfigurationProperties(Cat.class)
public class Con {

    @Autowired
    Cat cat;

    @RequestMapping("/catMethod")
    public Cat catMethod(){
        return cat;
    }
}


```
## 自动配置原理入门
### 引导加载自动配置类
```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```
#### @SpringBootConfiguration
@Configuration。代表当前是一个配置类
#### @ComponentScan
指定扫描哪些，Spring注解；
#### @EnableAutoConfiguration
```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```
##### @AutoConfigurationPackage
自动配置包？指定了默认的包规则
```java
@Import(AutoConfigurationPackages.Registrar.class)  //给容器中导入一个组件
public @interface AutoConfigurationPackage {}

//利用Registrar给容器中导入一系列组件
//将指定的一个包下的所有组件导入进来？MainApplication 所在包下。

```
##### @Import(AutoConfigurationImportSelector.class)
```
1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
4、从META-INF/spring.factories位置来加载一个文件。
默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories

```
![image (3).png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679041527006-20caba27-5ac9-429b-8d98-85e6884982b0.png#averageHue=%23fbfbf5&clientId=u7b239922-7e29-4&from=paste&height=155&id=ue36a0cc3&name=image%20%283%29.png&originHeight=210&originWidth=1007&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31202&status=done&style=shadow&taskId=u7e11619d-8003-4139-8879-663548f208b&title=&width=745.5)
### 按需加载配置
虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration
按照条件装配规则（@Conditional），最终会按需配置。
## 默认配置
```java
@Bean
@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字 multipartResolver 的组件
public MultipartResolver multipartResolver(MultipartResolver resolver) {
//给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
//SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
// Detect if the user has created a MultipartResolver but named it incorrectly
    return resolver;
}
// 给容器中加入了文件上传解析器；

```

SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了以用户的优先

总结：

- SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration
- 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties里面拿。xxxProperties和配置文件进行了绑定
- 生效的配置类就会给容器中装配很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 定制化配置
   - 用户直接自己@Bean替换底层的组件
   - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件  ---> xxxxProperties里面拿值  ----> application.properties**

## 最佳实践

- 引入场景依赖
   - [https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/using.html#using.build-systems.starters](https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/using.html#using.build-systems.starters)
- 查看自动配置了哪些（选做）
   - 自己分析，引入场景对应的自动配置一般都生效了
   - 配置文件中debug=true开启自动配置报告。Negative（不生效）\Positive（生效）
- 是否需要修改
   - 参照文档修改配置项：[https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/application-properties.html#appendix.application-properties](https://docs.spring.io/spring-boot/docs/2.7.9/reference/html/application-properties.html#appendix.application-properties)
   - 自己分析。xxxxProperties绑定了配置文件的哪些。
- 自定义加入或者替换组件
   - @Bean、@Component。。。
- 自定义器  **XXXXXCustomizer**；
- ……

## 开发技巧
### lombok
> 可以简化开发，但团队合作时需要集体配备lombok


使用前提：

1. 添加依赖
```xml
<!-- 添加依赖 -->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>
<!-- 打包时排除依赖 -->
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <configuration>
        <excludes>
          <exclude>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
          </exclude>
        </excludes>
      </configuration>
    </plugin>
  </plugins>
</build>
```

2. 下载Lombok插件

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679204851530-1a0a5b86-4074-42d9-b276-8424459540be.png#averageHue=%23f6f1f1&clientId=u0b05b75c-63ce-4&from=paste&height=347&id=ua1fdcb05&name=image.png&originHeight=694&originWidth=1084&originalType=binary&ratio=2&rotation=0&showTitle=false&size=56512&status=done&style=shadow&taskId=u413e2580-12b1-4f30-961b-701755f390d&title=&width=542)

```java
package com.linmu.boot.bean;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Map;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/19 09:51:23
**/
@SuppressWarnings({"all"})
@Data
@AllArgsConstructor
@NoArgsConstructor
@Component
@ToString
@EqualsAndHashCode
@ConfigurationProperties(prefix = "student")
public class Student {
    private String name;
    private int age;
    private Map<String,Integer> course;
}

```
> 说明：

| @Data | 添加get、set、equal、hashCode、toString方法 |
| --- | --- |
| @AllArgsConstructor | 添加全参构造器 |
| @NoArgsConstructor | 添加无参构造器 |
| @ToString | 添加toString方法 |
| @EqualsAndHashCode | 添加equal、hashCode方法 |


### Spring Initailizr（项目初始化向导）

#### 创建Spring Initailizr项目
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679205554378-243f81de-b118-4518-b6a9-4da39935b68c.png#averageHue=%23f1eae9&clientId=u0b05b75c-63ce-4&from=paste&height=616&id=u536c5116&name=image.png&originHeight=1231&originWidth=1574&originalType=binary&ratio=2&rotation=0&showTitle=false&size=170684&status=done&style=shadow&taskId=uc368c519-6b92-45af-9acb-b67a0aa462d&title=&width=787)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679205718269-5b2c92c3-2b09-4825-b861-1a0c1500ddea.png#averageHue=%23f4eded&clientId=u0b05b75c-63ce-4&from=paste&height=616&id=ued33bbf3&name=image.png&originHeight=1231&originWidth=1574&originalType=binary&ratio=2&rotation=0&showTitle=false&size=142735&status=done&style=shadow&taskId=u3d0e6ba0-8839-4f93-b9c6-1ff61771174&title=&width=787)

#### 模块说明
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679205924393-6fa82f08-9145-4114-b695-5ebc205c798b.png#averageHue=%23dad8d7&clientId=u0b05b75c-63ce-4&from=paste&height=268&id=u2aa879cb&name=image.png&originHeight=536&originWidth=1318&originalType=binary&ratio=2&rotation=0&showTitle=false&size=66934&status=done&style=shadow&taskId=u3e26fb8e-e304-47b5-831a-d08600b8e67&title=&width=659)

# sprintboot核心技术
## 配置文件---yaml技术
### yaml简介
YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

非常适合用来做以数据为中心的配置文件，可以以yml、yaml结尾

### 基本语法

- key: value；kv之间有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

### 数据类型
字面量：单个的、不可再分的值。date、boolean、string、number、null
```java
键 : 空格 值
key: value
```

对象：键值对的集合。map、hash、set、object
```java
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```

数组：一组按次序排列的值。array、list、queue
```java
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```

案例
```yaml
teacher:
  age: 30
  hobby:
    - basketball
    - football
    - running
  teach:
    Math: 201
    English: 202
    Chinese: 203
  students:
    - name: Jason
      age: 18
      course:
        Math: 90
        English: 99
    - name: Jackson
      age: 17
      course:
        Math: 100
        English: 70
  name: Jason Black
student:
  name: Jackson Black
  age: 21
  course:
    Math: 100
    Chinese: 80
    English: 60
```

## Web
### SpringMVC自动配置概览

1. 绝大多数的组件springboot已经默认添加
2. 在默认基础上添加了一下属性
- 包含ContentNegotiatingViewResolver和BeanNameViewResolver bean。
- 支持提供静态资源，包括对WebJars的支持
- 自动注册Converter、GenericConverter和Formatter bean。
- 对HttpMessageConverters的支持
- MessageCodesResolver的自动注册
- 静态index.html支持。
- 自动使用ConfigurableWebBindingInitializer bean

### 简单功能分析
#### 静态资源访问
##### 静态资源目录

默认情况下：静态资源可以存放在 /static  /public  /resources  /META-INF/resources 目录下
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679221736652-c17489ab-90b2-4e95-9630-b2a33a053f17.png#averageHue=%23fafaf8&clientId=udc282e3e-5923-4&from=paste&height=309&id=ua65007b4&name=image.png&originHeight=557&originWidth=1204&originalType=binary&ratio=2&rotation=0&showTitle=false&size=82262&status=done&style=shadow&taskId=ufcd1c8c1-469d-4672-9e2d-e07cd0f6462&title=&width=667)

静态资源访问方式： 当前项目根路径/ + 静态资源名 
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679221833143-03dc26c9-c77f-4e9c-a552-99cf9a76e157.png#averageHue=%2372b467&clientId=udc282e3e-5923-4&from=paste&height=553&id=u69bd722e&name=image.png&originHeight=1106&originWidth=2741&originalType=binary&ratio=2&rotation=0&showTitle=false&size=397129&status=done&style=shadow&taskId=u40f6e076-f1ba-4bd8-963b-ca7988bbe01&title=&width=1370.5)

原理： 静态映射/**。
请求进来，先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面

##### 静态访问前缀
改变默认的静态资源路径
```yaml
spring:
  mvc:
    static-path-pattern: /sc/**  # 添加前缀
  web:
  resources:  # 改变静态资源访问路径
    static-locations: [classpath:/stic/]  
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679222378200-1edfa67b-00f9-46e4-a7dc-d6d674321cd3.png#averageHue=%23fbfaf8&clientId=udc282e3e-5923-4&from=paste&height=349&id=u5c955238&name=image.png&originHeight=697&originWidth=1991&originalType=binary&ratio=2&rotation=0&showTitle=false&size=131989&status=done&style=shadow&taskId=u015b044c-5e7b-4c39-a306-187cf070236&title=&width=995.5)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679222413226-e5f42980-7a1e-407e-a8ab-cee5c33bc8b8.png#averageHue=%23e0dfdc&clientId=udc282e3e-5923-4&from=paste&height=604&id=u03507073&name=image.png&originHeight=1207&originWidth=1917&originalType=binary&ratio=2&rotation=0&showTitle=false&size=207995&status=done&style=shadow&taskId=u9ba018a0-c144-4c72-86ea-3b2b98ef282&title=&width=958.5)

##### webjar
以  /webjars/** 方式访问jar包资源
```xml
<dependency>
  <groupId>org.webjars.npm</groupId>
  <artifactId>jquery</artifactId>
  <version>3.6.4</version>
</dependency>
```
js jar包官网：[https://www.webjars.org/](https://www.webjars.org/)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679223776363-7095b420-01a5-4546-9fc1-a523006a1110.png#averageHue=%23f7f6ed&clientId=udc282e3e-5923-4&from=paste&height=399&id=ua4cb13f8&name=image.png&originHeight=798&originWidth=2084&originalType=binary&ratio=2&rotation=0&showTitle=false&size=243436&status=done&style=shadow&taskId=u3ca253e1-7b62-4951-aab5-3f4f7a1a881&title=&width=1042)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679224038183-b86796ae-4170-41a3-a772-cc61b4a3a1da.png#averageHue=%23f9f9f9&clientId=udc282e3e-5923-4&from=paste&height=581&id=uf6bad5b1&name=image.png&originHeight=1161&originWidth=1752&originalType=binary&ratio=2&rotation=0&showTitle=false&size=348871&status=done&style=shadow&taskId=u3d37a5d4-2f28-42af-a98a-5126ed39b2d&title=&width=876)

#### 欢迎页面
Spring Boot supports both static and templated welcome pages. It first looks for an index.html file in the configured static content locations. If one is not found, it then looks for an index template. If either is found, it is automatically used as the welcome page of the application.
欢迎页面：index.html
```yaml
# spring:
#   mvc:   # 添加静态资源前缀
#    static-path-pattern: /sc/**
```
> **添加了静态资源的前缀会影响图标的显示**


#### 自定义 `Favicon`
favicon.ico 放在静态资源目录下即可。
网页图标的名称和后缀必须是favicon.ico
```yaml
# spring:
#   mvc:   # 添加静态资源前缀
#    static-path-pattern: /sc/**
```
> **添加了静态资源的前缀会影响图标的显示**


### 请求参数处理
#### 请求映射
##### restful使用与原理

- @XXXMapping
- Restful风格支持（使用http请求方式动词来表示对资源的操作）
   - 以前：
      - /getUser   获取用户  
      - /deleteUser 删除用户   
      - /editUser  修改用户   
      - /saveUser 保存用户
   - 现在： /user 
      - GET-获取用户 
      - DELETE-删除用户 
      - PUT-修改用户 
      - POST-保存用户
   - 核心Filter；HiddenHttpMethodFilter
      - 用法： 表单method=post，隐藏域 name=_method
      - SpringBoot中手动开启
      - 扩展：可以把_method 这个名字换成我们自己喜欢的。（）
```yaml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true  # 开启restful风格
```

```java
package com.jason.controller;

import org.springframework.web.bind.annotation.*;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/19 15:38:22
**/
@SuppressWarnings({"all"})
@RestController
public class CRUD {

    @GetMapping("/user")
    public String getUser(){
        return "get Jason Black...";
    }

    @DeleteMapping("/user")
    public String deleteUser(){
        return "delete Jason Black...";
    }

    @PutMapping("/user")
    public String putUser(){
        return "put Jason Black...";
    }

    @PostMapping("/user")
    public String postUser(){
        return "post Jason Black...";
    }
}

```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>welcome to index..</h1>
    <form action="/user" method="get">
      <input type="submit" value="get Jason Black...">
    </form>
    <br>
    <form action="/user" method="post">
      <input type="submit" value="post Jason Black...">
    </form><br>
    <form action="/user" method="post">
      <input name="_method" type="hidden" value="delete">
      <input type="submit" value="delete Jason Black...">
    </form><br>
    <form action="/user" method="post">
      <input name="_method" type="hidden" value="PUT">
      <input type="submit" value="put Jason Black...">
    </form>
  </body>
</html>
```

#### 普通参数与注解

- @PathVariable
- @RequestHeader
- @ModelAttribute
- @RequestParam
- @MatrixVariable
- @CookieValue
- @RequestBody

```java
package com.jason.controller;


import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.List;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/19 21:34:18
**/
@SuppressWarnings({"all"})
@RestController
public class TestAnnotation {


    @RequestMapping("/annotation/{name}/{age}")
    public HashMap testAnnotation01(@PathVariable("name") String name,
                                    @PathVariable("age") Integer age,
                                    @RequestHeader("Sec-Fetch-Mode") String SecFetchMode,
                                    @CookieValue("_xsrf") String _xsrf,
                                    @RequestParam("hobby") List<String> hobby) {
        HashMap<String, Object> hashMap = new HashMap<String, Object>();
        hashMap.put("name",name);
        hashMap.put("age",age);
        hashMap.put("Sec-Fetch-Mode",SecFetchMode);
        hashMap.put("_xsrf",_xsrf);
        hashMap.put("hobby",hobby);
        return hashMap;
    }

    @RequestMapping("/testRequestBody")
    public HashMap testAnnotation(@RequestBody String param){
        HashMap<String, Object> hashMap = new HashMap<String, Object>();
        hashMap.put("param", param);
        return hashMap;
    }

    //1、语法： 请求路径：/cars/sell;low=34;brand=byd,audi,yd
    //2、SpringBoot默认是禁用了矩阵变量的功能
    //      手动开启：原理。对于路径的处理。UrlPathHelper进行解析。
    //              removeSemicolonContent（移除分号内容）支持矩阵变量的
    //3、矩阵变量必须有url路径变量才能被解析
    @RequestMapping("/testMatrix/{path}")// 需要添加路径占位符
    public HashMap testMatrixVariable(@MatrixVariable("name") String name,
                                      @MatrixVariable("age") Integer age){
        HashMap<String, Object> hashMap = new HashMap<String, Object>();
        hashMap.put("name",name);
        hashMap.put("age",age);
        return hashMap;
    }
}

```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>welcome to index..</h1>
    <a href="/annotation/Jason/21?hobby=basketball&hobby=running">test @PathVariable...</a><br>
    <a href="/testMatrix/pa;name=Jason;age=33">test @MatrixVariable...</a><br>
    <form action="/testRequestBody" method="post">
      <label>name</label>
      <input type="text" name="name"><br>
      <label>age</label>
      <input type="text" name="age">
      <input type="submit" value="submit">
    </form>
    <form action="/user" method="get">
      <input type="submit" value="get Jason Black...">
    </form>
    <br>
    <form action="/user" method="post">
      <input type="submit" value="post Jason Black...">
    </form><br>
    <form action="/user" method="post">
      <input name="_method" type="hidden" value="DELETE">
      <input type="submit" value="delete Jason Black...">
    </form><br>
    <form action="/user" method="post">
      <input name="_method" type="hidden" value="PUT">
      <input type="submit" value="put Jason Black...">
    </form>
  </body>
</html>
```

```java
package com.jason.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.PathMatchConfigurer;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.util.UrlPathHelper;

/**
* @author Jason Black
* @version The past cannot be redeemed, the future can be changed.
* @CreateTime 2023/3/19 22:15:35
**/
@SuppressWarnings({"all"})
@Configuration
public class UrlPath implements WebMvcConfigurer {
    
    /**
    * 方式一：implements WebMvcConfigurer，重写configurePathMatch方法
    * 方式二：匿名内部类，重写重写configurePathMatch方法
    */

    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        UrlPathHelper urlPathHelper = new UrlPathHelper();
        // 开启分号，是矩阵变量功能生效
        urlPathHelper.setRemoveSemicolonContent(false);
        configurer.setUrlPathHelper(urlPathHelper);
    }

    @Bean
    public WebMvcConfigurer setUrlPathHelper(){
        WebMvcConfigurer webMvcConfigurer = new WebMvcConfigurer() {
            @Override
            public void configurePathMatch(PathMatchConfigurer configurer) {
                UrlPathHelper urlPathHelper = new UrlPathHelper();
                // 开启分号，是矩阵变量功能生效
                urlPathHelper.setRemoveSemicolonContent(false);
                configurer.setUrlPathHelper(urlPathHelper);
            }
        };
        return webMvcConfigurer;
    }
}

```
#### 响应JSON数据
jackson.jar+@ResponseBody可响应JSON数据
```xml
<!-- json数据解析 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-json</artifactId>
</dependency>
```
#### 自定义组件
```java
@Bean
public WebMvcConfigurer webMvcConfigurer(){
    // 可以添加自定义组件，系统底层会优先执行自定义组件，再执行系统组件
}
```

### theyleaf解析
> 基本语法

| 表达式名字 | 语法 | 用途 |
| --- | --- | --- |
| 变量取值 | ${...}  | 获取请求域、session域、对象等值 |
| 选择变量 | *{...} | 获取上下文对象值 |
| 消息 | #{...} | 获取国际化等值 |
| 链接 | @{...} | 生成链接 |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |

所有h5兼容的标签写法：[https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes)

自动配好的策略
●1、所有thymeleaf的配置值都在 ThymeleafProperties
●2、配置好了 **SpringTemplateEngine **
**●3、配好了 ThymeleafViewResolver **
●4、我们只需要直接开发页面
```java
	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

> 拦截器

```java
/**
 * 登录检查
 * 1、配置好拦截器要拦截哪些请求
 * 2、把这些配置放在容器中
 */
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {

    /**
     * 目标方法执行之前
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        String requestURI = request.getRequestURI();
        log.info("preHandle拦截的请求路径是{}",requestURI);

        //登录检查逻辑
        HttpSession session = request.getSession();

        Object loginUser = session.getAttribute("loginUser");

        if(loginUser != null){
            //放行
            return true;
        }

        //拦截住。未登录。跳转到登录页
        request.setAttribute("msg","请先登录");
//        re.sendRedirect("/");
        request.getRequestDispatcher("/").forward(request,response);
        return false;
    }

    /**
     * 目标方法执行完成以后
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandle执行{}",modelAndView);
    }

    /**
     * 页面渲染以后
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.info("afterCompletion执行异常{}",ex);
    }
}
```

### 文件上传
>  页面表单

```java
<form method="post" action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"><br>
    <input type="submit" value="提交">
</form>
```
> 文件上传

```java
    /**
     * MultipartFile 自动封装上传过来的文件
     * @param email
     * @param username
     * @param headerImg
     * @param photos
     * @return
     */
    @PostMapping("/upload")
    public String upload(@RequestParam("email") String email,
                         @RequestParam("username") String username,
                         @RequestPart("headerImg") MultipartFile headerImg,
                         @RequestPart("photos") MultipartFile[] photos) throws IOException {

        log.info("上传的信息：email={}，username={}，headerImg={}，photos={}",
                email,username,headerImg.getSize(),photos.length);

        if(!headerImg.isEmpty()){
            //保存到文件服务器，OSS服务器
            String originalFilename = headerImg.getOriginalFilename();
            headerImg.transferTo(new File("H:\\cache\\"+originalFilename));
        }

        if(photos.length > 0){
            for (MultipartFile photo : photos) {
                if(!photo.isEmpty()){
                    String originalFilename = photo.getOriginalFilename();
                    photo.transferTo(new File("H:\\cache\\"+originalFilename));
                }
            }
        }


        return "main";
    }

```

### Web原生组件注入（Servlet、Filter、Listener）
1、使用Servlet API
@ServletComponentScan(basePackages = **"com.atguigu.admin"**) :指定原生Servlet组件都放在那里
@WebServlet(urlPatterns = **"/my"**)：效果：直接响应，**没有经过Spring的拦截器？**
@WebFilter(urlPatterns={**"/css/*"**,**"/images/*"**})
@WebListener
2，使用RegistrationBean
`ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean`
```java
@Configuration
public class MyRegistConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();

        return new ServletRegistrationBean(myServlet,"/my","/my02");
    }


    @Bean
    public FilterRegistrationBean myFilter(){

        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}
```

### 定制化原理
> 定制化常见方式

- 修改配置文件；
- **xxxxxCustomizer；**
- **编写自定义的配置类   xxxConfiguration；+ @Bean替换、增加容器中默认组件；视图解析器 **
- **Web应用 编写一个配置类实现 WebMvcConfigurer 即可定制化web功能；+ @Bean给容器中再扩展一些组件**
```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer
```



## 数据访问

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679797672739-c9c32fbc-b158-44cb-90f3-1adafa837939.png#averageHue=%23eeefdd&clientId=u0ebdc4ae-763d-4&from=paste&height=131&id=yEQ3p&name=image.png&originHeight=261&originWidth=1442&originalType=binary&ratio=2&rotation=0&showTitle=false&size=170444&status=done&style=shadow&taskId=ub7188fa4-9c76-49a0-9f91-1a5b54f7536&title=&width=721)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679797681179-6872f141-47a9-493e-9f82-d786e66a6664.png#averageHue=%23f1ebca&clientId=u0ebdc4ae-763d-4&from=paste&height=266&id=O5WFQ&name=image.png&originHeight=532&originWidth=1410&originalType=binary&ratio=2&rotation=0&showTitle=false&size=224623&status=done&style=shadow&taskId=u2a93ab7a-5ee5-490b-a2f6-b3384776c12&title=&width=705)![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1679797690651-dfe751fd-28eb-4cee-bec8-54d18d7da0e1.png#averageHue=%23fdfdfa&clientId=u0ebdc4ae-763d-4&from=paste&height=268&id=SaS6B&name=image.png&originHeight=535&originWidth=1763&originalType=binary&ratio=2&rotation=0&showTitle=false&size=358274&status=done&style=shadow&taskId=u67c69d81-b147-4167-90cb-04429cf6aef&title=&width=881.5)
### 整合druid
druid官方github地址：[https://github.com/alibaba/druid](https://github.com/alibaba/druid)
```java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

    druid:
      aop-patterns: com.atguigu.admin.*  #监控SpringBean
      filters: stat,wall     # 底层开启功能，stat（sql监控），wall（防火墙）

      stat-view-servlet:   # 配置监控页功能
        enabled: true
        login-username: admin
        login-password: admin
        resetEnable: false

      web-stat-filter:  # 监控web
        enabled: true
        urlPattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'


      filter:
        stat:    # 对上面filters里面的stat的详细配置
          slow-sql-millis: 1000
          logSlowSql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false

```


SpringBoot配置示例
[https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter](https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter)
配置项列表[https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8](https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8)


### 整合MyBatis操作
> 官网地址：[https://github.com/mybatis](https://github.com/mybatis)


```java
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
```

```java
# 配置mybatis规则
mybatis:
#  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
    
 可以不写全局；配置文件，所有全局配置文件的配置都放在configuration配置项中即可
```

- 导入mybatis官方starter
- 编写mapper接口。标准@Mapper注解
- 编写sql映射文件并绑定mapper接口
- 在application.yaml中指定Mapper配置文件的位置，以及指定全局配置文件的信息 （建议；**配置在mybatis.configuration**）

> 注解模式

```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);

    public void insert(City city);

}

```

> 混合模式

```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);

    public void insert(City city);

}

```
