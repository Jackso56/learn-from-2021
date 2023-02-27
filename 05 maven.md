# maven概述
## 为什么要学maven
### maven作为依赖管理工具
#### jar 包的规模 
随着我们使用越来越多的框架，或者框架封装程度越来越高，项目中使用的jar包也越来越多。项目中， 一个模块里面用到上百个jar包是非常正常的。 
比如下面的例子，我们只用到 SpringBoot、SpringCloud 框架中的三个功能： 
Nacos 服务注册发现 
Web 框架环境 
图模板技术 Thymeleaf 
最终却导入了 106 个 jar 包

而如果使用 Maven 来引入这些 jar 包只需要配置三个『依赖』：
```xml
<!-- Nacos 服务注册发现启动器 -->
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<!-- web启动器依赖 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- 视图模板技术 thymeleaf -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId
```

#### jar包来源

   2. 这个jar包所属技术的官网。官网通常是英文界面，网站的结构又不尽相同，甚至找到下载链接还发现需要通过特殊的工具下载。 
   3. 第三方网站提供下载。问题是不规范，在使用过程中会出现各种问题。 
      1. jar包的名称 
      2. jar包的版本 
      3. jar包内的具体细节 
   4. 而使用 Maven 后，依赖对应的 jar 包能够自动下载，方便、快捷又规范。

#### jar包之间的依赖关系

   3. 框架中使用的 jar 包，不仅数量庞大，而且彼此之间存在错综复杂的依赖关系。依赖关系的复杂程度， 已经上升到了完全不能靠人力手动解决的程度。另外，jar 包之间有可能产生冲突。进一步增加了我们在 jar 包使用过程中的难度。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675560573699-7523a041-8142-48b7-8c25-f0ad20d5a53b.png#averageHue=%23fefefe&clientId=ud783fea5-6ac4-4&from=paste&height=1312&id=ubb0fab8e&name=image.png&originHeight=2623&originWidth=1916&originalType=binary&ratio=1&rotation=0&showTitle=false&size=498935&status=done&style=shadow&taskId=u4b79cf87-695c-491b-85a1-1995d18c216&title=&width=958)
而实际上 jar 包之间的依赖关系是普遍存在的，如果要由程序员手动梳理无疑会增加极高的学习成本， 
而这些工作又对实现业务功能毫无帮助。而使用 Maven 则几乎不需要管理这些关系，极个别的地方调整一下即可，极大的减轻了我们的工作量。

### Maven 作为构建管理工具
#### 你没有注意过的构建 
你可以不使用 Maven，但是构建必须要做。当我们使用 IDEA 进行开发时，构建是 IDEA 替我们做的。 
#### 脱离 IDE 环境仍需构建 
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675560912168-e1e9dccc-fe89-4534-b1d4-901128123b5d.png#averageHue=%23ebeb00&clientId=ud783fea5-6ac4-4&from=paste&height=233&id=ue46839c1&name=image.png&originHeight=466&originWidth=911&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67522&status=done&style=none&taskId=u066d5cda-f93d-44eb-adf6-6363fd67d90&title=&width=455.5)
### 结论 

1. 管理规模庞大的 jar 包，需要专门工具。 
2. 脱离 IDE 环境执行构建操作，需要专门工具。

## 什么是maven
> Maven 是 Apache 软件基金会组织维护的一款专门为 Java 项目提供**构建**和**依赖**管理支持的工具。 


### 构建
Java 项目开发过程中，构建指的是使用**『原材料生产产品』**的过程。 
原材料：
Java 源代码 
基于 HTML 的 Thymeleaf 文件 
图片 
配置文件 
**…… **
产品：一个可以在服务器上运行的项目 构建过程包含的主要的环节： 
清理：删除上一次构建的结果，为下一次构建做好准备编译：
Java 源程序编译成 *.class 字节码文件 
测试：运行提前准备好的测试程序 
报告：针对刚才测试的结果生成一个全面的信息 
打包：
Java工程：jar包 
Web工程：war包 
安装：把一个 Maven 工程经过打包操作生成的 jar 包或 war 包存入 Maven 仓库 
部署： 
部署 jar 包：把一个 jar 包部署到 Nexus 私服服务器上 
部署 war 包：借助相关 Maven 插件（例如 cargo），将 war 包部署到 Tomcat 服务器上

### 依赖
如果 A 工程里面用到了 B 工程的类、接口、配置文件等等这样的资源，那么我们就可以说 A 依赖 B。
例如： 
junit-4.12 依赖 hamcrest-core-1.3 
thymeleaf-3.0.12.RELEASE 依赖 ognl-3.1.26 
ognl-3.1.26 依赖 javassist-3.20.0-GA 
thymeleaf-3.0.12.RELEASE 依赖 attoparser-2.0.5.RELEASE 
thymeleaf-3.0.12.RELEASE 依赖 unbescape-1.1.6.RELEASE 
thymeleaf-3.0.12.RELEASE 依赖 slf4j-api-1.7.26 
依赖管理中要解决的具体问题： 
jar 包的下载：使用 Maven 之后，jar 包会从规范的远程仓库下载到本地 
jar 包之间的依赖：通过依赖的传递性自动完成 
jar 包之间的冲突：通过对依赖的配置进行调整，让某些jar包不会被导入

### maven工作机制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675561613105-d21e1f3e-1e16-4416-9493-ab976d773071.png#averageHue=%237db05b&clientId=ua50f1cef-11f4-4&from=paste&height=417&id=u06bab977&name=image.png&originHeight=834&originWidth=1211&originalType=binary&ratio=1&rotation=0&showTitle=false&size=221451&status=done&style=none&taskId=ub664840b-1611-4903-9bac-703d3e55a69&title=&width=605.5)

# Maven核心程序解压与配置
## maven压缩和配置
### maven下载：
> maven首页：[https://maven.apache.org/](https://maven.apache.org/)
> maven下载：[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675562179962-63213025-9bca-4258-9ff4-da995cb37f2c.png#averageHue=%23faf9f9&clientId=ua50f1cef-11f4-4&from=paste&height=338&id=u8211da3c&name=image.png&originHeight=675&originWidth=2313&originalType=binary&ratio=1&rotation=0&showTitle=false&size=189107&status=done&style=shadow&taskId=ua8fcce2c-ea0b-4056-ac55-162d7385c2f&title=&width=1156.5)

### 解压maven
核心程序压缩包：apache-maven-3.8.4-bin.zip，解压到非中文、没有空格的目录。例如：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675562376545-d5a5a95d-efd6-4e41-87a6-1dcecb08a69d.png#averageHue=%23fcfcfb&clientId=ua50f1cef-11f4-4&from=paste&height=351&id=ufb88dd35&name=image.png&originHeight=702&originWidth=1747&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83988&status=done&style=shadow&taskId=ua22df02c-8b71-4638-9ebd-c8c359f5e6f&title=&width=873.5)
在解压目录中，我们需要着重关注 Maven 的核心配置文件：conf/settings.xml

### 指定本地仓库
本地仓库默认值：用户家目录/.m2/repository。由于本地仓库的默认位置是在用户的家目录下，而家目
录往往是在 C 盘，也就是系统盘。将来 Maven 仓库中 jar 包越来越多，仓库体积越来越大，可能会拖慢
C 盘运行速度，影响系统性能。所以建议将 Maven 的本地仓库放在其他盘符下。配置方式如下：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675562611592-fa7e01ac-d265-46dd-a498-df5debc3a10f.png#averageHue=%23eeedec&clientId=ua50f1cef-11f4-4&from=paste&height=232&id=u330daf5d&name=image.png&originHeight=464&originWidth=2633&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101080&status=done&style=shadow&taskId=u61279f0b-e800-4273-b463-5526e03c0e8&title=&width=1316.5)
本地仓库这个目录，我们手动创建一个空的目录即可。 
记住：一定要把 localRepository 标签从注释中拿出来。 
注意：本地仓库本身也需要使用一个非中文、没有空格的目录。

### 配置阿里云提供的镜像仓库
> Maven 下载 jar 包默认访问境外的中央仓库，而国外网站速度很慢。改成阿里云提供的镜像仓库，访问
> 国内网站，可以让 Maven 下载 jar 包的时候速度更快。配置的方式是：


1. 将原有的例子配置注释掉

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675562800842-0ba6deff-b324-4f7f-8da7-a7b966ff449a.png#averageHue=%23eeedec&clientId=ua50f1cef-11f4-4&from=paste&height=258&id=ue88d8e46&name=image.png&originHeight=516&originWidth=2123&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86223&status=done&style=shadow&taskId=ub531dac2-2030-4007-8360-27caccd865d&title=&width=1061.5)

2. 加入阿里云的配置

将下面 mirror 标签整体复制到 settings.xml 文件的 mirrors 标签的内部。
```xml
<mirror>
  <id>nexus-aliyun</id>
  <mirrorOf>central</mirrorOf>
  <name>Nexusaliyun</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```
### 配置 Maven 工程的基础 JDK 版本
> 如果按照默认配置运行，Java 工程使用的默认 JDK 版本是 1.5，而我们熟悉和常用的是 JDK 1.8 版本。
> 修改配置的方式是：将 profile 标签整个复制到 settings.xml 文件的 profiles 标签内。


```xml
<profile>
 <id>jdk-1.8</id>
 <activation>
  <activeByDefault>true</activeByDefault>
  <jdk>1.8</jdk>
 </activation>
 <properties>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
  <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion> </properties>
</profile>
```

## 配置环境变量
### 检查 JAVAHOME 配置是否正确
> Maven 是一个用 Java 语言开发的程序，它必须基于 JDK 来运行，需要通过 JAVA_HOME 来找到 JDK 的安装位置。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675563340025-8abd71d5-344d-4fd6-a445-f37d8d4d866d.png#averageHue=%23f0efee&clientId=u5e904457-607c-4&from=paste&height=602&id=u9e043567&name=image.png&originHeight=1203&originWidth=1236&originalType=binary&ratio=1&rotation=0&showTitle=false&size=146859&status=done&style=shadow&taskId=ud83d0939-e2cd-4827-b344-4d7b3fd80dc&title=&width=618)

可以使用下面的命令验证：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675563382225-4d14f4bb-0389-47f4-a94a-437f8e643e07.png#averageHue=%23e9e8e7&clientId=u5e904457-607c-4&from=paste&height=163&id=uafb32e23&name=image.png&originHeight=325&originWidth=1433&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45725&status=done&style=shadow&taskId=u94ab2353-bb3a-46f6-852d-b74283c2e69&title=&width=716.5)

### 配置 MAVENHOME
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675563931960-46adc331-505f-4797-965e-399d9fcd95cf.png#averageHue=%23bca169&clientId=u5e904457-607c-4&from=paste&height=630&id=ue2bd8b84&name=image.png&originHeight=1260&originWidth=2618&originalType=binary&ratio=1&rotation=0&showTitle=false&size=359626&status=done&style=shadow&taskId=ud42b511d-afed-4c99-aa7f-7109afff59f&title=&width=1309)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675564004435-5f53b5aa-ecf9-4dc6-b4d3-2fc3a90b58a6.png#averageHue=%23eeedec&clientId=u5e904457-607c-4&from=paste&height=208&id=ue6b0714d&name=image.png&originHeight=183&originWidth=653&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9875&status=done&style=shadow&taskId=u2e6c975a-e355-4b9c-a79b-bc4fa28a4d5&title=&width=742.5)
> 配置环境变量的规律： 
> XXX_HOME 通常指向的是 bin 目录的上一级
> PATH 指向的是 bin 目录


### 配置PATH
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675564027793-1e1a4c12-18a9-45fe-bcca-baffa60d13c8.png#averageHue=%23dcb472&clientId=u5e904457-607c-4&from=paste&height=320&id=u8abd8c57&name=image.png&originHeight=228&originWidth=527&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14340&status=done&style=shadow&taskId=uc3a15b5a-7583-4bcf-9ef0-ea2d98a69bb&title=&width=740.5)

### 验证
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675564114227-7e700d18-3c73-4d28-baae-fb255d645c45.png#averageHue=%23e8e7e6&clientId=u5e904457-607c-4&from=paste&height=600&id=u27e8436e&name=image.png&originHeight=1200&originWidth=2580&originalType=binary&ratio=1&rotation=0&showTitle=false&size=299272&status=done&style=shadow&taskId=u5a35d5a4-029f-4746-9010-22e0a1cf172&title=&width=1290)

# maven基础
## jar包配置
> 官网：[https://mvnrepository.com/](https://mvnrepository.com/)

## maven核心概念：坐标
### 向量说明
使用三个『向量』在『Maven的仓库』中唯一的定位到一个『jar』包。 
groupId：公司或组织的 id 
artifactId：一个项目或者是项目中的一个模块的 id 
version：版本号
### 三个向量的取值方式 
groupId：公司或组织域名的倒序，通常也会加上项目名称 
例如：com.atguigu.maven 
artifactId：模块的名称，将来作为 Maven 工程的工程名 
version：模块的版本号，根据自己的需要设定 
例如：SNAPSHOT 表示快照版本，正在迭代过程中，不稳定的版本 
例如：RELEASE 表示正式版本

### 坐标和仓库中 jar 包的存储路径之间的对应关系
坐标：
```xml
<groupId>javax.servlet</groupId>
<artifactId>servlet-api</artifactId>
<version>2.5</version>
```
上面坐标对应的 jar 包在 Maven 本地仓库中的位置：
```xml
Maven本地仓库根目录\javax\servlet\servlet-api\2.5\servlet-api-2.5.jar 1
```

## maven命令
### 清理操作 
mvn clean 
效果：删除 target 目录 
### 编译操作 
主程序编译：mvn compile 
测试程序编译：mvn test-compile 
主体程序编译结果存放的目录：target/classes 
测试程序编译结果存放的目录：target/test-classes 
### 测试操作 
mvn test 
测试的报告存放的目录：target/surefire-reports 
### 打包操作 
mvn package 
打包的结果——jar 包，存放的目录：target

### 安装操作
mvn -install
安装的效果是将本地构建过程中生成的 jar 包存入 Maven 本地仓库。这个 jar 包在 Maven 仓库中的路 
径是根据它的坐标生成的。

坐标信息如下:
```xml
<groupId>com.atguigu.maven</groupId>
<artifactId>pro01-maven-java</artifactId>
<version>1.0-SNAPSHOT</version>
```
在 Maven 仓库中生成的路径如下：
D:\maven-rep1026\com\atguigu\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-
java-1.0-SNAPSHOT.jar

另外，安装操作还会将 pom.xml 文件转换为 XXX.pom 文件一起存入本地仓库。所以我们在 Maven 的 
本地仓库中想看一个 jar 包原始的 pom.xml 文件时，查看对应 XXX.pom 文件即可，它们是名字发生了 
改变，本质上是同一个文件。

## maven目录结构
![](https://cdn.nlark.com/yuque/0/2023/jpeg/34904523/1673937860860-3f7a3797-1374-4c1d-a09e-fef90bb81c12.jpeg)
## 依赖的范围

标签的位置：dependencies/dependency/**scope **
标签的可选值：**compile**/**test**/**provided**/system/runtime/**import**
### compile 和 test 对比 
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675565553732-83608216-36eb-44c2-baad-b1c0608811d1.png#averageHue=%23f2f2f1&clientId=u67e374f2-f0a1-4&from=paste&height=164&id=u2b2c90a4&name=image.png&originHeight=327&originWidth=1612&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54254&status=done&style=shadow&taskId=u0badd92a-20b9-469c-818c-9153c51bc2e&title=&width=806)
### compile 和 provided 对比
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675565567442-8d7f4e2c-809b-403b-862f-f8f9509ecaa5.png#averageHue=%23f2f2f1&clientId=u67e374f2-f0a1-4&from=paste&height=165&id=u97c88b93&name=image.png&originHeight=329&originWidth=1602&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55725&status=done&style=shadow&taskId=ua00a4ff0-45b2-40a7-97c9-586e8af0221&title=&width=801)

### 结论 
compile：通常使用的第三方框架的 jar 包这样在项目实际运行时真正要用到的 jar 包都是以 compile 范 
围进行依赖的。比如 SSM 框架所需jar包。 

test：测试过程中使用的 jar 包，以 test 范围依赖进来。比如 junit。

provided：在开发过程中需要用到的“服务器上的 jar 包”通常以 provided 范围依赖进来。
比如 servlet-api、jsp-api。而这个范围的 jar 包之所以不参与部署、不放进 war 包，就是避免和服务器上已有的同类 jar 包产生冲突，同时减轻服务器的负担。说白了就是：“**服务器上已经有了，你就别带啦！**

## 依赖的传递性
### 概念
A 依赖 B，B 依赖 C，那么在 A 没有配置对 C 的依赖的情况下，A 里面能不能直接使用 C？
### 传递的原则
在 A 依赖 B，B 依赖 C 的前提下，C 是否能够传递到 A，取决于 B 依赖 C 时使用的依赖范围。 
B 依赖 C 时使用 compile 范围：可以传递 
B 依赖 C 时使用 test 或 provided 范围：不能传递，所以需要这样的 jar 包时，就必须在需要的地 
方明确配置依赖才可以。

## 依赖的排除
### 概念
当 A 依赖 B，B 依赖 C 而且 C 可以传递到 A 的时候，A 不想要 C，需要在 A 里面把 C 排除掉。而往往这 
种情况都是为了避免 jar 包之间的冲突。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675566279292-b46c8117-38f5-490e-9f31-6b034cdd1b3c.png#averageHue=%23f0f0f0&clientId=u67e374f2-f0a1-4&from=paste&height=211&id=uab09567e&name=image.png&originHeight=422&originWidth=1602&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95628&status=done&style=shadow&taskId=u2c43b173-9454-4081-81c1-38b91ab6d1c&title=&width=801)
所以配置依赖的排除其实就是阻止某些 jar 包的传递。因为这样的 jar 包传递过来会和其他 jar 包冲突。

### 配置方式
```xml
<dependency>
  <groupId>com.atguigu.maven</groupId>
  <artifactId>pro01-maven-java</artifactId>
  <version>1.0-SNAPSHOT</version>
  <scope>compile</scope>
  <!-- 使用excludes标签配置依赖的排除 -->
  <exclusions>
    <!-- 在exclude标签中配置一个具体的排除 -->
    <exclusion>
      <!-- 指定要排除的依赖的坐标（不需要写version） -->
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

## 继承
### 概念
Maven工程之间，A 工程继承 B 工程 
B 工程：父工程 
A 工程：子工程 
本质上是 A 工程的 pom.xml 中的配置继承了 B 工程中 pom.xml 的配置。

### 作用
> 在父工程中统一管理项目中的依赖信息，具体来说是管理依赖信息的版本。 


它的背景是：
对一个比较大型的项目进行了模块拆分。 
一个 project 下面，创建了很多个 module。 
每一个 module 都需要配置自己的依赖信息。

它背后的需求是： 
在每一个 module 中各自维护各自的依赖信息很容易发生出入，不易统一管理。 
使用同一个框架内的不同 jar 包，它们应该是同一个版本，所以整个项目中使用的框架版本需要统		一。 
使用框架时所需要的 jar 包组合（或者说依赖信息组合）需要经过长期摸索和反复调试，最终确定 
一个可用组合。这个耗费很大精力总结出来的方案不应该在新的项目中重新摸索。

通过在父工程中为整个项目维护依赖信息的组合既**保证了整个项目使用规范、准确的 jar 包**；又能够将 
**以往的经验沉淀**下来，节约时间和精力。

### 父工程
> 只有打包方式为 pom 的 Maven 工程能够管理其他 Maven 工程。打包方式为 pom 的 Maven 工程中不写业务代码，它是专门管理其他 Maven 工程的工程。

```xml
<groupId>com.atguigu.maven</groupId> 
<artifactId>pro03-maven-parent</artifactId> 
<version>1.0-SNAPSHOT</version> 
<!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom --> 
<packaging>pom</packaging>
```

## 聚合
### 聚合本身的含义
> 部分组成整体


### maven中的聚合
使用一个“总工程”将各个“模块工程”汇集起来，作为一个整体对应完整的项目。 
项目：整体 
模块：部分

概念的对应关系： 
从继承关系角度来看： 
父工程 
子工程 
从聚合关系角度来看： 
总工程 
模块工程

### 好处
一键执行 Maven 命令：很多构建命令都可以在“总工程”中一键执行。

以 mvn install 命令为例：Maven 要求有父工程时先安装父工程；有依赖的工程时，先安装被依赖的 
工程。我们自己考虑这些规则会很麻烦。但是工程聚合之后，在总工程执行 mvn install 可以一键完 
成安装，而且会自动按照正确的顺序执行。

配置聚合之后，各个模块工程会在总工程中展示一个列表，让项目中的各个模块一目了然。

### 聚合的配置
在总工程中配置 modules 即可：
```xml
<modules>
  <module>pro04-maven-module</module>
  <module>pro05-maven-module</module>
  <module>pro06-maven-module</module>
</modules>
```
### 依赖循环问题
如果 A 工程依赖 B 工程，B 工程依赖 C 工程，C 工程又反过来依赖 A 工程，那么在执行构建操作时会报 
下面的错误： 
DANGER 
[ERROR] [ERROR] The projects in the reactor contain a cyclic reference: 
这个错误的含义是：循环引用。

# maven其他核心概念
## 生命周期
### 作用 
为了让构建过程自动化完成，Maven 设定了三个生命周期，生命周期中的每一个环节对应构建过程中的 
一个操作。
### 三个生命周期
| 生命周期名称 | 作用 | 各个环节 |
| --- | --- | --- |
| clean | 清理相关 | pre-clean clean post-clean |
| site | 生成站点相关 | pre-site site post-site deploy-site |
| default | 构建过程相关 | validate generate-sources process-sources generate-resources process
resources 复制并处理资源文件，至目标目录，准备打包。 compile 编译项目 
main 目录下的源代码。 process-classes generate-test-sources process-test
sources generate-test-resources process-test-resources 复制并处理资源文 
件，至目标测试目录。 test-compile 编译测试源代码。 process-test-classes 
test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。 
prepare-package package 接受编译好的代码，打包成可发布的格式，如JAR。 
pre-integration-test integration-test post-integration-test verify install将包 
安装至本地仓库，以让其它项目依赖。 deploy将最终的包复制到远程的仓库， 
以让其它开发人员共享；或者部署到服务器上运行（需借助插件，例如： 
cargo）。 |


### 特点

前面三个生命周期彼此是独立的。 
在任何一个生命周期内部，执行任何一个具体环节的操作，都是从本周期最初的位置开始执行，直 
到指定的地方。（本节记住这句话就行了，其他的都不需要记） 

Maven 之所以这么设计其实就是为了提高构建过程的自动化程度：让使用者只关心最终要干的即可，过 
程中的各个环节是自动执行的。

## 插件和目标	
### 插件 
Maven 的核心程序仅仅负责宏观调度，不做具体工作。具体工作都是由 Maven 插件完成的。例如：编 
译就是由 maven-compiler-plugin-3.1.jar 插件来执行的。 
### 目标 
一个插件可以对应多个目标，而每一个目标都和生命周期中的某一个环节对应。 
Default 生命周期中有 compile 和 test-compile 两个和编译相关的环节，这两个环节对应 compile 和 
test-compile 两个目标，而这两个目标都是由 maven-compiler-plugin-3.1.jar 插件来执行的。

## 仓库
本地仓库：在当前电脑上，为电脑上所有 Maven 工程服务 
远程仓库：需要联网 
局域网：我们自己搭建的 Maven 私服，例如使用 Nexus 技术。 
Internet 
中央仓库 
镜像仓库：内容和中央仓库保持一致，但是能够分担中央仓库的负载，同时让用户能够 
就近访问提高下载速度，例如：Nexus aliyun 

建议：不要中央仓库和阿里云镜像混用，否则 jar 包来源不纯，彼此冲突。 
专门搜索 Maven 依赖信息的网站：https://mvnrepository.com/
