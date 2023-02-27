# mybatis-简介
> 官网：[https://mybatis.net.cn/index.html](https://mybatis.net.cn/index.html)


## MyBatis历史

- MyBatis最初是Apache的一个开源项目iBatis, 2010年6月这个项目由Apache Software Foundation迁移到了Google Code。随着开发团队转投Google Code旗下，iBatis3.x正式更名为MyBatis。代码于2013年11月迁移到Github
- iBatis一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。iBatis提供的持久层框架包括SQL Maps和Data Access Objects（DAO）

## MyBatis特性

1. MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架
2. MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集
3. MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录
4. MyBatis 是一个 半自动的ORM（Object Relation Mapping）框架
5. mybatis下载：[https://github.com/mybatis/mybatis-3](https://github.com/mybatis/mybatis-3)

> **orm**
> ORM是Object Relational Mapping 对象关系映射。简单来说，就是把数据库表和实体类及实体类的属性对应起来，让开发者操作实体类就实现操作数据库表，它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建连接等复杂过程。
> ORM:Object-Relation-Mapping，也就是对象关系映射，是一种程序设计思想，mybatis就是ORM的一种实现方式，简单来说就是将数据库中查询出的数据映射到对应的实体中。

## 和其它持久化层技术对比

- JDBC 
   - SQL 夹杂在Java代码中耦合度高，导致硬编码内伤
   - 维护不易且实际开发需求中 SQL 有变化，频繁修改的情况多见
   - 代码冗长，开发效率低
- Hibernate 和 JPA 
   - 操作简便，开发效率高
   - 程序中的长难复杂 SQL 需要绕过框架
   - 内部自动生产的 SQL，不容易做特殊优化
   - 基于全映射的全自动框架，大量字段的 POJO 进行部分映射时比较困难。
   - 反射操作太多，导致数据库性能下降
- MyBatis 
   - 轻量级，性能出色
   - SQL 和 Java 编码分开，功能边界清晰。Java代码专注业务、SQL语句专注数据
   - 开发效率稍逊于HIbernate，但是完全能够接受

# mybatis-环境	搭建
## 创建maven工程
**打包方式：jar包**
```xml
<dependencies>
	<!-- Mybatis核心 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
		<version>3.5.7</version>
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
      <version>8.0.28</version>
  </dependency>
</dependencies>
```

## 创建数据库（练习专用）
> SQL工具网：[https://www.sqlfather.com/](https://www.sqlfather.com/)

```sql
-- `users`
create table if not exists `users`
(
`user_id` int not null auto_increment comment '用户ID' primary key,
`user_name` varchar(256) not null comment '用户名',
`user_age` int not null comment '年龄',
`user_sex` varchar(256) not null comment '性别'
) comment '`users`';
```
```sql
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (1, '任涛', 35, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (2, '阎昊强', 35, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (3, '王鹏煊', 16, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (4, '韩熠彤', 38, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (5, '黎健柏', 20, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (6, '谢炫明', 33, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (7, '陈烨霖', 34, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (8, '覃语堂', 38, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (9, '杨越彬', 17, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (10, '曹烨磊', 30, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (11, '林语堂', 28, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (12, '汪弘文', 25, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (13, '方伟宸', 17, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (14, '沈明轩', 37, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (15, '李哲瀚', 22, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (16, '罗鹏', 27, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (17, '何嘉懿', 33, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (18, '覃伟祺', 28, '男');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (19, '崔子默', 37, '女');
insert into `users` (`user_id`, `user_name`, `user_age`, `user_sex`) values (20, '程伟祺', 17, '男');
```

## 创建实体类
> 使数据库中的每一条数据对应一个java对象，一张表对应一个java类
> **注意：创建时最好给定set和get方法和构造器**

```java
package com.linmu.pojo;

public class POJOUser {
    private int userId;
    private String userName;
    private int userAge;
    private String userSex;

    public POJOUser() {
    }

    public POJOUser(int userId, String userName, int userAge, String userSex) {
        this.userId = userId;
        this.userName = userName;
        this.userAge = userAge;
        this.userSex = userSex;
    }

    @Override
    public String toString() {
        return "POJOUser{" +
            "userId=" + userId +
            ", userName='" + userName + '\'' +
            ", userAge=" + userAge +
            ", userSex='" + userSex + '\'' +
            '}';
    }

    public int getUserId() {
        return userId;
    }

    public void setUserId(int userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public int getUserAge() {
        return userAge;
    }

    public void setUserAge(int userAge) {
        this.userAge = userAge;
    }

    public String getUserSex() {
        return userSex;
    }

    public void setUserSex(String userSex) {
        this.userSex = userSex;
    }
}

```

## 创建mapper接口

> MyBatis中的mapper接口相当于以前的dao。但是区别在于，mapper仅仅是接口，我们不需要提供实现类

```java
package com.linmu.mapper;

import com.linmu.pojo.POJOUser;

import java.util.List;

public interface MapperByUsers {
    List<POJOUser> selectAllUsersInfo();
}

```

## 创建MyBatis的核心配置文件
> 习惯上命名为`mybatis-config.xml`，这个文件名仅仅只是建议，并非强制要求。将来整合Spring之后，这个配置文件可以省略，所以大家操作时可以直接复制、粘贴。
核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息
核心配置文件存放的位置是src/main/resources目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--扫描数据库连接的配置文件-->
    <properties resource="jdbc.properties"></properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
      <!--扫描mapper.xml配置文件，获取SQL语句;可读取多个配置文件-->
        <mapper resource="mapper.xml"/> 
    </mappers>
</configuration>
```

> 配置文件方式连接数据库：在src/main/resources目录下创建一个properties文件

```
jdbc.driver=com.mysql.cj.jdbc.Driver
# 该url用于mysql-8.0
jdbc.url=jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
jdbc.username=root
jdbc.password=000000
```



## 创建MyBatis的映射文件

- 相关概念：ORM（Object Relationship Mapping）对象关系映射。 
   - 对象：Java的实体类对象
   - 关系：关系型数据库
   - 映射：二者之间的对应关系
| Java概念 | 数据库概念 |
| --- | --- |
| 类 | 表 |
| 属性 | 字段/列 |
| 对象 | 记录/行 |


- 映射文件的命名规则 
   - 表所对应的实体类的类名+Mapper.xml
   - 例如：表t_user，映射的实体类为User，所对应的映射文件为UserMapper.xml
   - 因此一个映射文件对应一个实体类，对应一张表的操作
   - MyBatis映射文件用于编写SQL，访问以及操作表中的数据
   - MyBatis映射文件存放的位置是src/main/resources/mappers目录下
- MyBatis中可以面向接口操作数据，要保证两个一致 
   - mapper接口的全类名和映射文件的命名空间（namespace）保持一致
   - mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace：映射接口对应的路径-->
<mapper namespace="com.linmu.mapper.MapperByUsers"> 
  <!--    创建映射结果集，使数据表中的字段与映射类中的属性关联
  id表示映射结果集的标识，供SQL语句调用
  type表示数据表需要关联的类    -->
  <resultMap id="userMapper" type="com.linmu.pojo.POJOUser">
    <id column="user_id" property="userId"></id>
    <result column="user_name" property="userName"></result>
    <result column="user_age" property="userAge"></result>
    <result column="user_sex" property="userSex"></result>
  </resultMap>
  <!-- 	编写SQL语句 -->
  <!--    id表示mapper接口中的方法，-->
  <!--    resultMap表示映射结果集-->
  <select id="selectAllUsersInfo" resultMap="userMapper">
    select * from users
  </select>
</mapper>
```

## 日志文件配置（可选配置）
```
#声明日志的输出级别及输出方式
log4j.rootLogger=DEBUG,stdout
# MyBatis logging configuration...
# MyBatis 日志配置
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
#声明日志的输出位置在控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
#定义日志的打印格式  %t 表示线程名称  %5p表示输出日志级别   %n表示换行
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```
```xml
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
```

## 查询数据
```java
package com.linmu;

import com.linmu.mapper.MapperByUsers;
import com.linmu.pojo.POJOUser;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class testmapper {

    @Test
    public void selectAllUsersInfo() throws IOException {
        //读取MyBatis的核心配置文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        //获取SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        //通过核心配置文件所对应的字节输入流创建工厂类SqlSessionFactory，生产SqlSession对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        //获取sqlSession，此时通过SqlSession对象所操作的sql都必须手动提交或回滚事务
        //SqlSession sqlSession = sqlSessionFactory.openSession();
        //创建SqlSession对象，此时通过SqlSession对象所操作的sql都会自动提交
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        //通过代理模式创建UserMapper接口的代理实现类对象
        MapperByUsers mapper = sqlSession.getMapper(MapperByUsers.class);
        //调用UserMapper接口中的方法，就可以根据UserMapper的全类名匹配元素文件，通过调用的方法名匹配映射文件中的SQL标签，并执行标签中的SQL语句
        List<POJOUser> pojoUsers = mapper.selectAllUsersInfo();
        //提交事务
        //sqlSession.commit();
        pojoUsers.forEach(item -> System.out.println(item));
    }
}

```

# mybatis-核心配置文件详解
> 详情见官网：[https://mybatis.net.cn/index.html](https://mybatis.net.cn/index.html)

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
  <!--    引入数据库连接的配置文件,以'${属性名}'的方式访问属性-->
  <properties resource="JDBC.properties"></properties>
  <!--   全局配置 -->
  <settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
    <!--mapUnderscoreToCamelCase：下划线命名转驼峰命名     默认false：未开启-->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
  	<!--开启延迟加载-->
    <setting name="lazyLoadingEnabled" value="true"/>
  </settings>
  <!--    设置映射类的别名-->
  <typeAliases>
    <!-- typeAlias:具体设置映射类的别名      
    			默认别名：即类名，且不区分大小写
    type：设置映射类的访问路径
    alias：设置映射类的访问路径的别名   若不设置，则使用默认别名-->
    <typeAlias type="org.example.Obj.UserInfo" ></typeAlias>
    <!--        package:以包为单位，路径内包下的类均设置默认类名
    name:映射类的路径-->
    <package name="org.example.Obj.UserInfo"/>
  </typeAliases>
  <!--
  environments
  default:id值   默认使用的数据库连接配置
  -->
  <environments default="development">
    <!--        environment   具体数据库连接配置    id：表示环境配置的唯一标识        -->
    <environment id="development">
      <!--         transactionManager  事务管理方式
      type: JDBC    表示JDBC原生事务管理方式,即事务的提交、回滚需要手动执行
      type: MANAGED  被管理,有其他框架管理-->
      <transactionManager type="JDBC"/>
      <!--        dataSource:配置数据源     type=“POOLED|UNPOOLED|JNDI”
      POOLED:使用数据库连接池缓存数据
      UNPOOLED: 不使用数据库连接池
      JNDI: 使用上下文中的数据源-->
      <dataSource type="POOLED">
        <!--                设置连接数据库的驱动-->
        <property name="driver" value="${JDBC.driver}"/>
        <!--                设置连接数据库的连接地址-->
        <property name="url" value="${JDBC.url}"/>
        <!--                设置连接数据库的用户名-->
        <property name="username" value="${JDBC.username}"/>
        <!--                设置连接数据库的密码-->
        <property name="password" value="${JDBC.password}"/>
      </dataSource>
    </environment>
  </environments>
  <!--    引入映射文件-->
  <mappers>
    <!--         mapper:引入单个映射文件
    resource:配置文件的具体路径（默认从resources目录中读取）-->
    <mapper resource="mapper.xml"/>
    <!--     package：以包为单位引入配置文件
    name：具体的包路径
    要求：
    1.映射接口与配置文件所在的包名要一致
    2.映射接接口与配置文件的名称要一致-->
    <!--        <package name=""/>-->
  </mappers>
</configuration>
```

## 标签顺序
> 注意：标签顺序不能乱，否则会报错；标签可以不写，但相对顺序不能乱
> properties?,settings?,typeAliases?,typeHandlers?,
> objectFactory?,objectWrapperFactory?,reflectorFactory?,
> plugins?,environments?,databaseIdProvider?,mappers?


## 具体标签含义
> configuration标签：引入数据库连接的配置文件


>   properties标签：引入*.properties配置文件
>   	resource：配置文件地址


> settings标签：全局配置
> setting标签：具体配置
> name：配置名称          value：是否开启
> mapUnderscoreToCamelCase：下划线命名映射驼峰命名，默认false
> lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载，默认false
> aggressiveLazyLoading：开启时，任一方法的调用都会加载该对象的所有延迟加载属性，默认false
> 



> typeAliases标签：设置映射类的别名
> typeAlias标签：具体设置映射类的别名      默认别名：即类名，且不区分大小写
>  type：设置映射类的访问路径
>                 alias：设置映射类的访问路径的别名   若不设置，则使用默认别名

> package标签：以包为单位，路径内包下的类均设置默认类名
>  name:映射类的路径


> environments标签：引入数据库连接配置
> default：id值       使用的数据库连接配置
> environment   具体数据库连接配置    
> id：表示环境配置的唯一标识
> transactionManager 标签：事务管理方式
> type: JDBC    表示JDBC原生事务管理方式,即事务的提交、回滚需要手动执行
> type: MANAGED  被管理,有其他框架管理
> dataSource标签：配置数据源     
> type=“POOLED|UNPOOLED|JNDI”
> POOLED：使用数据库连接池缓存数据
> UNPOOLED：不使用数据库连接池
> JNDI：使用上下文中的数据源
> property标签：具体设置数据库连接参数
> name：参数名称
> driver：表示设置连接数据库的驱动
> name：表示设置连接数据库的连接地址
> username：表示设置连接数据库的用户名
> password：表示设置连接数据库的密码
> value：参数的值


> mappers标签：引入映射文件
> mapper标签：引入单个映射文件
>  resource：配置文件的具体路径
> package标签：以包为单位引入配置文件
> name：具体的包路径
> package标签使用要求：
>             1.映射接口与配置文件所在的包名要一致
>             2.映射接接口与配置文件的名称要一致


# mybatis-CRUD
> create（插入），read（读），update（更新），delete（删除）
> **不同的SQL语句需要使用不同的标签**

## create
```xml
<!--    int insertData(POJOUser user);-->
    <insert id="insertData">
        insert into users values(null, "jackson", 17, "male")
    </insert>
```

## read
```xml
<!--    List<POJOUser> selectAllUsersInfo();-->
    <select id="selectAllUsersInfo" resultMap="userMapper">
        select * from users
    </select>
```

## update
```xml
<!--    int updateData(int userId);-->
    <update id="updateData">
        update users set user_age=20 where user_id=#{userId}
    </update>
```

## delete
```xml
<!--    int deleteData(int id);-->
    <delete id="deleteData">
        delete from users where user_id=#{id}
    </delete>
```

# mybatis-参数获取（重点）

- MyBatis获取参数值的两种方式：${}和#{}
- ${}的本质就是字符串拼接，#{}的本质就是占位符赋值
- ${}使用字符串拼接的方式拼接sql，**若为字符串类型或日期类型的字段进行赋值时，需要手动加单引号**；但是#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以自动添加单引号
- **往往使用#{}的方式，因为${}存在SQL注入的风险，但有些情况不得不使用${}**
## 单个字面量类型的参数

- 若mapper接口中的方法参数为单个的字面量类型，此时可以使用${}和#{}以任意的名称（最好见名识意）获取参数的值，注意${}需要手动加单引号

```xml
<!--    List<POJOUser> selectBySex(String sex);-->
    <select id="selectBySex" resultMap="userMapper">
        select * from users where user_sex=#{sex}
    </select>
```
```xml
<!--    POJOUser selectById(int id);-->
    <select id="selectByName" resultMap="userMapper">
        select * from users where user_name='${name}'
    </select>
```

## 多个字面量类型的参数

- 若mapper接口中的方法参数为多个时，此时MyBatis会自动将这些参数放在一个map集合中
   - 以arg0,arg1...为键，以参数为值；
   - 以param1,param2...为键，以参数为值；
- 因此只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号。
- 使用arg或者param都行，要注意的是，arg是从arg0开始的，param是从param1开始的

- 可以通过@Param注解标识mapper接口中的方法参数，此时，会将这些参数放在map集合中 
   - 以@Param注解的value属性值为键，以参数为值；
   - 以param1,param2...为键，以参数为值；
- 只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--    List<POJOUser> selectByAgeAndSex(int age, String sex);-->
    <select id="selectByAgeAndSex" resultMap="userMapper">
        select * from users where user_age>#{param1} and user_sex=#{param2}
    </select>
```

```xml
<!--    List<POJOUser> selectByIdAndSex(Integer id, String sex);-->
    <select id="selectByIdAndSex" resultMap="userMapper">
        select * from users where user_id>#{arg0} and user_sex=#{arg1}
    </select>
```

```xml
<!--    List<POJOUser> selectByIdAndAge(@Param("id") Integer id,@Param("age") Integer age);-->
    <select id="selectByIdAndAge" resultMap="userMapper">
        select * from users where user_id>#{id} and #{age}>user_age
    </select>
```

## map集合类型参数

- 若mapper接口中的方法需要的参数为多个时，此时可以手动创建map集合，将这些数据放在map中只需要通过${}和#{}访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--    POJOUser selectByNameAndId(Map<String, String> map);-->
<select id="selectByNameAndId" resultMap="userMapper">
  select * from users where user_name=#{name} and user_id=#{id}
</select>
```

```java
@Test
public void selectByNameAndId() throws IOException {
    Map<String, String> hashMap = new HashMap<>();
    hashMap.put("name","黎健柏");
    hashMap.put("id","5");
    InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
    SqlSession sqlSession = build.openSession(true);
    MapperByUsers mapper = sqlSession.getMapper(MapperByUsers.class);
    POJOUser pojoUser = mapper.selectByNameAndId(hashMap);
    System.out.println(pojoUser);
}
```

## 实体类类型参数

- 若mapper接口中的方法参数为实体类对象时此时可以使用${}和#{}，通过访问实体类对象中的属性名获取属性值，注意${}需要手动加单引号

```xml
<!--    List<POJOUser> selectByAge(POJOUser user);-->
    <select id="selectByAge" resultMap="userMapper">
        select * from users where user_age>#{userAge}
    </select>
```

```java
@Test
public void selectByAge() throws IOException{
    InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
    SqlSession sqlSession = build.openSession();
    MapperByUsers mapper = sqlSession.getMapper(MapperByUsers.class);
    List<POJOUser> users = mapper.selectByAge(new POJOUser(20));
    users.forEach(user-> System.out.println(user));
}
```

## 小结

- 建议分成两种情况进行处理 
   - 实体类类型的参数
   - 使用@Param标识参数

# mybatis-select

1. 如果查询出的数据只有一条，可以通过 
   1. 实体类对象接收
   2. List集合接收
   3. Map集合接收，结果`{password=123456, sex=男, id=1, age=23, username=admin}`
2. 如果查询出的数据有多条，一定不能用实体类对象接收，会抛异常TooManyResultsException，可以通过 
   1. 实体类类型的LIst集合接收
   2. Map类型的LIst集合接收
   3. 在mapper接口的方法上添加@MapKey注解

## 查询一个实体类对象
```xml
<!--    POJOUser selectById(Integer id);-->
    <select id="selectById" resultMap="userMapper">
        select * from users where user_id=#{id}
    </select>
```

## 查询一个List集合
```xml
<!--    List<POJOUser> selectBySex(String sex);-->
    <select id="selectBySex" resultMap="userMapper">
        select * from users where user_sex=#{sex}
    </select>
```

## 查询单个数据
```xml
<!--    int getCount();-->
<select id="getCount" resultType="integer">
  select count(*) from users
</select>
```

## 查询一条数据为map集合
```xml
<!--    Map<String, Object> selectByIdForMap(Integer id);-->
    <select id="selectByIdForMap" resultType="map">
        select * from users where user_id=#{id}
    </select>
```

## 查询多条数据为Map集合
### 方法一
```java
/**
 * 查询所有用户信息为map集合
 * @return
 * 将表中的数据以map集合的方式查询，一条数据对应一个map；若有多条数据，
 * 就会产生多个map集合，并且最终要以一个map的方式返回数据，
 * 此时需要通过@MapKey注解设置map集合的键，值是每条数据所对应的map集合
 */
@MapKey("userId")
Map<String,Object> selectUsersInfo();
```
```xml
<!--    Map<String,Object> selectUsersInfo();-->
    <select id="selectUsersInfo" resultMap="userMapper">
        select * from users
    </select>
```
```java
    @Test
    public void selectUsersInfo() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        MapperByUsers mapper = sqlSession.getMapper(MapperByUsers.class);
        Map<Integer, Object> integerObjectMap = mapper.selectUsersInfo();
        integerObjectMap.forEach((id, obj) -> System.out.println(id+"-"+obj));
    }
```

### 方法二
```xml
<!--    List<Map<String,Object>> selectUserInfoByList();-->
<select id="selectUserInfoByList" resultMap="userMapper">
  select * from users
</select>
```


# 类型转换异常
> 有时返回映射和返回类型不能同时出现：这取决于返回类型是属性、还是对象

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675734486845-f488d8c4-9a55-48e3-8611-c1b6470aba87.png#averageHue=%23f9f7f3&clientId=u0fa2fcc4-bc51-4&from=paste&height=390&id=ufe15ba53&name=image.png&originHeight=779&originWidth=1670&originalType=binary&ratio=1&rotation=0&showTitle=false&size=173085&status=done&style=shadow&taskId=u510e299c-1fc2-4781-a598-2ba01ec2aac&title=&width=835)
# mybatis-特殊SQL
## 模糊查询
> **select标签内不能写注释，否则会报错**

```xml
<!--    Map<String, Object> selectForLike(String name);
select * from users where user_name like "%"#{name}
select * from users where user_name like concat('%',#{name})-->
    <select id="selectForLike" resultType="map" >
        select * from users where user_name like '%${name}'
    </select>
```

## 批量删除

- 只能使用${}，如果使用#{}，则解析后的sql语句为`delete from t_user where id in ('1,2,3')`，这样是将`1,2,3`看做是一个整体，只有id为`1,2,3`的数据会被删除。正确的语句应该是`delete from t_user where id in (1,2,3)`，或者`delete from t_user where id in ('1','2','3')`
> **不同的SQL使用不同的标签**

```xml
<!--    int deleteDataMany(String ids);-->
    <delete id="deleteDataMany">
        delete from users where user_id in (${ids})
    </delete>
```

## 动态设置表名

- 只能使用${}，因为表名不能加单引号

```xml
<!--    List<POJOUser> selectForTable(String tableName);-->
    <select id="selectForTable" resultMap="userMapper">
        select * from ${tableName}
    </select>
```

## 添加功能获取自增的主键

- 使用场景 
   - t_clazz(clazz_id,clazz_name)
   - t_student(student_id,student_name,clazz_id)
   1. 添加班级信息
   2. 获取新添加的班级的id
   3. 为班级分配学生，即将某学的班级id修改为新添加的班级的id
- 在mapper.xml中设置两个属性 
   - useGeneratedKeys：设置使用自增的主键
   - keyProperty：因为增删改有统一的返回值是受影响的行数，因此只能将获取的自增的主键放在传输的参数user对象的某个属性中

```xml
<!--    int insertDataForPrimaryKey(POJOUser pojoUser);-->
    <insert id="insertDataForPrimaryKey" useGeneratedKeys="true" keyProperty="userId">
        insert into users values(null,#{userName},#{userAge},#{userSex})
    </insert>
```
```java
@Test
public void insertDataForPrimaryKey() throws IOException{
    InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
    SqlSession sqlSession = build.openSession(true);
    MapperByUsers mapper = sqlSession.getMapper(MapperByUsers.class);
    int i = mapper.insertDataForPrimaryKey(new POJOUser("jackson", 24, "男"));
    System.out.println(i);
}
```
> **若要将id也传入，需要将id的类型改变为包装类，包装类支持null，基本数据类型不支持**


# mybatis-resultMap
> 处理类属性与表字段的映射关系

## 一对一映射处理

- resultMap：设置自定义映射 
   - 属性： 
      - id：表示自定义映射的唯一标识，不能重复
      - type：查询的数据要映射的实体类的类型
   - 子标签： 
      - id：设置主键的映射关系
      - result：设置普通字段的映射关系
      - 子标签属性： 
         - property：设置映射关系中实体类中的属性名
         - column：设置映射关系中表中的字段名
- 若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射，即使字段名和属性名一致的属性也要映射，也就是全部属性都要列出来
```xml
<!--    创建映射结果集，使数据表中的字段与映射类中的属性关联
        id表示映射结果集的标识，供SQL语句调用
        type表示数据表需要关联的类    -->
    <resultMap id="userMapper" type="com.linmu.pojo.POJOUser">
        <id column="user_id" property="userId"></id>
        <result column="user_name" property="userName"></result>
        <result column="user_age" property="userAge"></result>
        <result column="user_sex" property="userSex"></result>
    </resultMap>
```

- 若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用_），实体类中的属性名符合Java的规则（使用驼峰）。此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系 
   1. 可以通过为字段起别名的方式，保证和实体类中的属性名保持一致
```xml
<!--List<Emp> getAllEmp();-->
<select id="getAllEmp" resultType="Emp">
  select eid,emp_name empName,age,sex,email from t_emp
</select>
```

   2. 可以在MyBatis的核心配置文件中的`setting`标签中，设置一个全局配置信息mapUnderscoreToCamelCase，可以在查询表中数据时，自动将_类型的字段名转换为驼峰，例如：字段名user_name，设置了mapUnderscoreToCamelCase，此时字段名就会转换为userName。
```xml
  <settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
    <!--mapUnderscoreToCamelCase：下划线命名转驼峰命名     默认false：未开启-->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
  	<!--开启延迟加载-->
    <setting name="lazyLoadingEnabled" value="true"/>
  </settings>
```

## 多对一映射处理
> 案例

```java
package com.linmu.pojo;

public class Emp {
    private int id;
    private String name;
    private int age;
    private String sex;
    private Dep dep;

    public Emp() {
    }

    public Emp(int id, String name, int age, String sex, Dep dep) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.dep = dep;
    }

    public Emp(String name, int age, String sex, Dep dep) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.dep = dep;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    public Dep getDep() {
        return dep;
    }

    public void setDep(Dep dep) {
        this.dep = dep;
    }
}

```
```java
package com.linmu.pojo;

public class Dep {
    private String name;
    private int id;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "Dep{" +
                "name='" + name + '\'' +
                ", id=" + id +
                '}';
    }

    public Dep(String name, int id) {
        this.name = name;
        this.id = id;
    }

    public Dep() {
    }
}

```

### 方式一：级联
```xml
<resultMap id="empMapper" type="com.linmu.pojo.Emp">
  <id column="emp_id" property="id"></id>
  <result column="emp_name" property="name"></result>
  <result column="emp_age" property="age"></result>
  <result column="emp_sex" property="sex"></result>
  <result column="dep_id" property="dep.id"></result>
  <result column="dep_name" property="dep.name"></result>
</resultMap>

```
```xml
<!--    List<Emp> selectEmp();-->
<select id="selectEmp" resultMap="empMapper">
  select * from emp join dep on emp.dep_id=dep.dep_id
</select>
```

### 方式二：association

- association：处理多对一的映射关系
- property：需要处理多对的映射关系的属性名
- javaType：该属性的类型

```xml
<resultMap id="empMapperByAss" type="com.linmu.pojo.Emp">
  <id column="emp_id" property="id"></id>
  <result column="emp_name" property="name"></result>
  <result column="emp_age" property="age"></result>
  <result column="emp_sex" property="sex"></result>
  <association property="dep" javaType="com.linmu.pojo.Dep">
    <id column="dep_id" property="id"></id>
    <result column="dep_name" property="name"></result>
  </association>
</resultMap>
```
```xml
<!--    List<Emp> selectEmp();-->
    <select id="selectEmp" resultMap="empMapperByAss">
        select * from emp join dep on emp.dep_id=dep.dep_id
    </select>
```

### 方式三：分步查询
#### 第一步：查员工信息

- select：设置分布查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）
- column：设置分步查询的条件
```xml
    <resultMap id="empDe" type="com.linmu.pojo.Emp">
        <id column="emp_id" property="id"></id>
        <result column="emp_name" property="name"></result>
        <result column="emp_age" property="age"></result>
        <result column="emp_sex" property="sex"></result>
        <association property="dep" javaType="com.linmu.pojo.Dep"
                select="com.linmu.mapper.DepMapper.selectByStep"
                column="dep_id"></association>
    </resultMap>
```
```xml
<!--    Emp selectByStep(Integer id);-->
    <select id="selectByStep" resultMap="empDe">
        select * from emp where emp_id=#{id}
    </select>
```

#### 第二步：查部门信息
```xml
    <resultMap id="depMapper" type="com.linmu.pojo.Dep">
        <id column="dep_id" property="id"></id>
        <result column="dep_name" property="name"></result>
    </resultMap>
```
```xml
<!--    Dep selectByStep(Integer id);-->
    <select id="selectByStep" resultMap="depMapper">
        select * from dep where dep_id=#{id}
    </select>
```

#### 测试
```java
public EmpMapper mapperUtil() {
    InputStream resourceAsStream = null;
    try{
        resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
    } catch (Exception e){
        e.printStackTrace();
    }
    SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
    SqlSession sqlSession = build.openSession(true);
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    return mapper;
}

@Test
public void selectEmp(){
    EmpMapper empMapper = mapperUtil();
    Emp emp = empMapper.selectByStep(2104);
    System.out.println(emp);
}
```

## 一对多映射处理
### 方式一：collection

- collection：用来处理一对多的映射关系
- ofType：表示该属性对饮的集合中存储的数据的类型
```xml
<resultMap id="DeptAndEmpResultMap" type="Dept">
  <id property="did" column="did"></id>
  <result property="deptName" column="dept_name"></result>
  <collection property="emps" ofType="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
  </collection>
</resultMap>
<!--Dept getDeptAndEmp(@Param("did") Integer did);-->
<select id="getDeptAndEmp" resultMap="DeptAndEmpResultMap">
  select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did = #{did}
</select>
```

### 方式二：分步查询
#### 第一步：查看部门信息
```java
/**
* 通过分步查询，查询部门及对应的所有员工信息
* 分步查询第一步：查询部门信息
* @param did 
* @return com.atguigu.mybatis.pojo.Dept
* @date 2022/2/27 22:04
*/
Dept getDeptAndEmpByStepOne(@Param("did") Integer did);
```
```xml
<resultMap id="DeptAndEmpByStepOneResultMap" type="Dept">
  <id property="did" column="did"></id>
  <result property="deptName" column="dept_name"></result>
  <collection property="emps"
    select="com.atguigu.mybatis.mapper.EmpMapper.getDeptAndEmpByStepTwo"
    column="did"></collection>
</resultMap>
<!--Dept getDeptAndEmpByStepOne(@Param("did") Integer did);-->
<select id="getDeptAndEmpByStepOne" resultMap="DeptAndEmpByStepOneResultMap">
  select * from t_dept where did = #{did}
</select>
```
#### 第二步：查员工信息
```java
/**
 * 通过分步查询，查询部门及对应的所有员工信息
 * 分步查询第二步：根据部门id查询部门中的所有员工
 * @param did
 * @return java.util.List<com.atguigu.mybatis.pojo.Emp>
 * @date 2022/2/27 22:10
 */
List<Emp> getDeptAndEmpByStepTwo(@Param("did") Integer did);
```
```java
<!--List<Emp> getDeptAndEmpByStepTwo(@Param("did") Integer did);-->
<select id="getDeptAndEmpByStepTwo" resultType="Emp">
	select * from t_emp where did = #{did}
</select>
```
## 延迟加载

- 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息： 
   - lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
   - aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载
- 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加载)|eager(立即加载)"
```java
<settings>
	<!--开启延迟加载-->
	<setting name="lazyLoadingEnabled" value="true"/>
</settings>
```
```java
@Test
public void getEmpAndDeptByStepOne() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	Emp emp = mapper.getEmpAndDeptByStepOne(1);
	System.out.println(emp.getEmpName());
}
```

- 关闭延迟加载，两条SQL语句都运行了
- 开启延迟加载，只运行获取emp的SQL语句

```java
@Test
public void getEmpAndDeptByStepOne() {
	SqlSession sqlSession = SqlSessionUtils.getSqlSession();
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	Emp emp = mapper.getEmpAndDeptByStepOne(1);
	System.out.println(emp.getEmpName());
	System.out.println("----------------");
	System.out.println(emp.getDept());
}
```

- 开启后，需要用到查询dept的时候才会调用相应的SQL语句
- fetchType：当开启了全局的延迟加载之后，可以通过该属性手动控制延迟加载的效果，fetchType="lazy(延迟加载)|eager(立即加载)"

```java
<resultMap id="empAndDeptByStepResultMap" type="Emp">
	<id property="eid" column="eid"></id>
	<result property="empName" column="emp_name"></result>
	<result property="age" column="age"></result>
	<result property="sex" column="sex"></result>
	<result property="email" column="email"></result>
	<association property="dept"
				 select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
				 column="did"
				 fetchType="lazy"></association>
</resultMap>
```

# 动态SQL

- Mybatis框架的动态SQL技术是一种根据特定条件动态拼装SQL语句的功能，它存在的意义是为了解决拼接SQL语句字符串时的痛点问题

## if标签

- if标签可通过test属性（即传递过来的数据）的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中的内容不会执行
- 在where后面添加一个恒成立条件`1=1` 
   - 这个恒成立条件并不会影响查询的结果
   - 这个`1=1`可以用来拼接`and`语句，例如：当empName为null时 
      - 如果不加上恒成立条件，则SQL语句为`select * from t_emp where and age = ? and sex = ? and email = ?`，此时`where`会与`and`连用，SQL语句会报错
      - 如果加上一个恒成立条件，则SQL语句为`select * from t_emp where 1= 1 and age = ? and sex = ? and email = ?`，此时不报错
```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
  select * from t_emp where 1=1
  <if test="empName != null and empName !=''">
    and emp_name = #{empName}
  </if>
  <if test="age != null and age !=''">
    and age = #{age}
  </if>
  <if test="sex != null and sex !=''">
    and sex = #{sex}
  </if>
  <if test="email != null and email !=''">
    and email = #{email}
  </if>
</select>
```

## where标签

- where和if一般结合使用： 
   - 若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字
   - 若where标签中的if条件满足，则where标签会自动添加where关键字，并将条件最前方多余的and/or去掉
```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select * from t_emp
	<where>
		<if test="empName != null and empName !=''">
			emp_name = #{empName}
		</if>
		<if test="age != null and age !=''">
			and age = #{age}
		</if>
		<if test="sex != null and sex !=''">
			and sex = #{sex}
		</if>
		<if test="email != null and email !=''">
			and email = #{email}
		</if>
	</where>
</select>
```

- 注意：where标签不能去掉条件后多余的and/or
```xml
<!--这种用法是错误的，只能去掉条件前面的and/or，条件后面的不行-->
<if test="empName != null and empName !=''">
emp_name = #{empName} and
</if>
<if test="age != null and age !=''">
	age = #{age}
</if>
```

## trim标签

- trim用于去掉或添加标签中的内容
- 常用属性 
   - prefix：在trim标签中的内容的前面添加某些内容
   - suffix：在trim标签中的内容的后面添加某些内容
   - prefixOverrides：在trim标签中的内容的前面去掉某些内容
   - suffixOverrides：在trim标签中的内容的后面去掉某些内容
- 若trim中的标签都不满足条件，则trim标签没有任何效果，也就是只剩下`select * from t_emp`
```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select * from t_emp
	<trim prefix="where" suffixOverrides="and|or">
		<if test="empName != null and empName !=''">
			emp_name = #{empName} and
		</if>
		<if test="age != null and age !=''">
			age = #{age} and
		</if>
		<if test="sex != null and sex !=''">
			sex = #{sex} or
		</if>
		<if test="email != null and email !=''">
			email = #{email}
		</if>
	</trim>
</select>
```

## choose、when、otherwise标签

- `choose、when、otherwise`相当于`if...else if..else`
- when至少要有一个，otherwise至多只有一个
- 相当于`if a else if b else if c else d`，只会执行其中一个
```xml
<select id="getEmpByChoose" resultType="Emp">
	select * from t_emp
	<where>
		<choose>
			<when test="empName != null and empName != ''">
				emp_name = #{empName}
			</when>
			<when test="age != null and age != ''">
				age = #{age}
			</when>
			<when test="sex != null and sex != ''">
				sex = #{sex}
			</when>
			<when test="email != null and email != ''">
				email = #{email}
			</when>
			<otherwise>
				did = 1
			</otherwise>
		</choose>
	</where>
</select>
```

## foreach标签

- 属性： 
   - collection：设置要循环的数组或集合
   - item：表示集合或数组中的每一个数据
   - separator：设置循环体之间的分隔符，分隔符前后默认有一个空格，如`,`
   - open：设置foreach标签中的内容的开始符
   - close：设置foreach标签中的内容的结束符
- 批量删除
```xml
<!--int deleteMoreByArray(Integer[] eids);-->
<delete id="deleteMoreByArray">
	delete from t_emp where eid in
	<foreach collection="eids" item="eid" separator="," open="(" close=")">
		#{eid}
	</foreach>
</delete>
```

- 批量添加
```xml
<!--int insertMoreByList(@Param("emps") List<Emp> emps);-->
<insert id="insertMoreByList">
	insert into t_emp values
	<foreach collection="emps" item="emp" separator=",">
		(null,#{emp.empName},#{emp.age},#{emp.sex},#{emp.email},null)
	</foreach>
</insert>
```

## SQL片段

- sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入
- 声明sql片段：`<sql>`标签
```xml

<sql id="empColumns">eid,emp_name,age,sex,email</sql>
```

- 引用sql片段：`<include>`标签
```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="Emp">
	select <include refid="empColumns"></include> from t_emp
</select>
```

# mybatis-缓存
## 一级缓存

- 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问
- 使一级缓存失效的四种情况： 
   1. 不同的SqlSession对应不同的一级缓存
   2. 同一个SqlSession但是查询条件不同
   3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
   4. 同一个SqlSession两次查询期间手动清空了缓存

## 二级缓存

- 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取
- 二级缓存开启的条件 
   1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
   2. 在映射文件中设置标签
   3. 二级缓存必须在SqlSession关闭或提交之后有效
   4. 查询的数据所转换的实体类类型必须实现序列化的接口
- 使二级缓存失效的情况：两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效

## 二级缓存配置

- 在mapper配置文件中添加的cache标签可以设置一些属性
- eviction属性：缓存回收策略 
   - LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
   - FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
   - SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
   - WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
   - 默认的是 LRU
- flushInterval属性：刷新间隔，单位毫秒 
   - 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句（增删改）时刷新
- size属性：引用数目，正整数 
   - 代表缓存最多可以存储多少个对象，太大容易导致内存溢出
- readOnly属性：只读，true/false 
   - true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
   - false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false

## 缓存查询顺序

- 如果二级缓存没有命中，再查询一级缓存
- 如果一级缓存也没有命中，则查询数据库
- SqlSession关闭之后，一级缓存中的数据会写入二级缓存


- MyBatis的逆向工程
- 正向工程：先创建Java实体类，由框架负责根据实体类生成数据库表。Hibernate是支持正向工程的
- 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：
   - Java实体类
   - Mapper接口
   - Mapper映射文件<dependencies> <!-- MyBatis核心依赖包 --> <dependency>   <groupId>org.mybatis</groupId>   <artifactId>mybatis</artifactId>   <version>3.5.9</version> </dependency> <!-- junit测试 --> <dependency>   <groupId>junit</groupId>   <artifactId>junit</artifactId>   <version>4.13.2</version>   <scope>test</scope> </dependency> <!-- MySQL驱动 --> <dependency>   <groupId>mysql</groupId>   <artifactId>mysql-connector-java</artifactId>   <version>8.0.27</version> </dependency> <!-- log4j日志 --> <dependency>   <groupId>log4j</groupId>   <artifactId>log4j</artifactId>   <version>1.2.17</version> </dependency> </dependencies> <!-- 控制Maven在构建过程中相关配置 --> <build> <!-- 构建过程中用到的插件 --> <plugins>   <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->   <plugin>       <groupId>org.mybatis.generator</groupId>       <artifactId>mybatis-generator-maven-plugin</artifactId>       <version>1.3.0</version>       <!-- 插件的依赖 -->       <dependencies>           <!-- 逆向工程的核心依赖 -->           <dependency>               <groupId>org.mybatis.generator</groupId>               <artifactId>mybatis-generator-core</artifactId>               <version>1.3.2</version>           </dependency>           <!-- 数据库连接池 -->           <dependency>               <groupId>com.mchange</groupId>               <artifactId>c3p0</artifactId>               <version>0.9.2</version>           </dependency>           <!-- MySQL驱动 -->           <dependency>               <groupId>mysql</groupId>               <artifactId>mysql-connector-java</artifactId>               <version>8.0.27</version>           </dependency>       </dependencies>   </plugin> </plugins> </build><?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE configuration   PUBLIC "-//mybatis.org//DTD Config 3.0//EN"   "http://mybatis.org/dtd/mybatis-3-config.dtd"> <configuration> <properties resource="jdbc.properties"/> <typeAliases>   <package name=""/> </typeAliases> <environments default="development">   <environment id="development">       <transactionManager type="JDBC"/>       <dataSource type="POOLED">           <property name="driver" value="${jdbc.driver}"/>           <property name="url" value="${jdbc.url}"/>           <property name="username" value="${jdbc.username}"/>           <property name="password" value="${jdbc.password}"/>       </dataSource>   </environment> </environments> <mappers>   <package name=""/> </mappers> </configuration>
# 创建逆向工程的步骤(待添加)

- 正向工程：先创建Java实体类，由框架负责根据实体类生成数据库表。Hibernate是支持正向工程的
- 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源： 
   -  Java实体类 
   -  Mapper接口 
   -  Mapper映射文件
## 添加依赖
```java
<dependencies>
<!-- MyBatis核心依赖包 -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.9</version>
</dependency>
<!-- junit测试 -->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.2</version>
  <scope>test</scope>
</dependency>
<!-- MySQL驱动 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.27</version>
</dependency>
<!-- log4j日志 -->
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
<!-- 构建过程中用到的插件 -->
<plugins>
  <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
  <plugin>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.3.0</version>
      <!-- 插件的依赖 -->
      <dependencies>
          <!-- 逆向工程的核心依赖 -->
          <dependency>
              <groupId>org.mybatis.generator</groupId>
              <artifactId>mybatis-generator-core</artifactId>
              <version>1.3.2</version>
          </dependency>
          <!-- 数据库连接池 -->
          <dependency>
              <groupId>com.mchange</groupId>
              <artifactId>c3p0</artifactId>
              <version>0.9.2</version>
          </dependency>
          <!-- MySQL驱动 -->
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>8.0.27</version>
          </dependency>
      </dependencies>
  </plugin>
</plugins>
</build>
```
  
## 创建逆向工程的配置文件

-  文件名必须是：`generatorConfig.xml` 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
  <!--
  targetRuntime: 执行生成的逆向工程的版本
  MyBatis3Simple: 生成基本的CRUD（清新简洁版）
  MyBatis3: 生成带条件的CRUD（奢华尊享版）
  -->

  <context id="DB2Tables" targetRuntime="MyBatis3Simple">
    <!-- 数据库的连接信息 -->
    <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
      connectionURL="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;allowPublicKeyRetrieval=true&amp;serverTimezone=UTC"
      userId="root"
      password="000000">
    </jdbcConnection>
    <!-- javaBean的生成策略-->
    <javaModelGenerator targetPackage="com.linmu.mybatis.pojo" targetProject=".\src\main\java">
      <property name="enableSubPackages" value="true" />
      <property name="trimStrings" value="true" />
    </javaModelGenerator>
    <!-- SQL映射文件的生成策略 -->
    <sqlMapGenerator targetPackage="com.linmu.mybatis.mapper"
      targetProject=".\src\main\resources">
      <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>
    <!-- Mapper接口的生成策略 -->
    <javaClientGenerator type="XMLMAPPER"
      targetPackage="com.linmu.mybatis.mapper" targetProject=".\src\main\java">
      <property name="enableSubPackages" value="true" />
    </javaClientGenerator>
    <!-- 逆向分析的表 -->
    <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
    <!-- domainObjectName属性指定生成出来的实体类的类名 -->
    <table tableName="users" domainObjectName="User"/>
  </context>
</generatorConfiguration>
```
## mybatis核心配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <properties resource="jdbc.properties"></properties>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="./com/linmu/mybatis/mapper/userMapper.xml"/> <!--扫描mapper.xml配置文件，获取SQL语句;可读取多个配置文件-->
    <!--        <mapper resource="depMapper.xml"/>-->
    <!--        <mapper resource="empMapper.xml"/>-->
  </mappers>
</configuration>
```
```xml
jdbc.driver=com.mysql.cj.jdbc.Driver
# ?url??mysql-8.0
jdbc.url=jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
jdbc.username=root
jdbc.password=000000
```
## 其他配置文件
```xml
# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=DEBUG, A1

# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n
```
## 执行MBG插件的generate目标
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1676080360466-cc5f80e8-5c09-4bb5-8563-e4785bfef887.png#averageHue=%2324292f&clientId=ud9ae09ec-822f-4&from=paste&height=506&id=u3105f5b6&name=image.png&originHeight=1011&originWidth=1150&originalType=binary&ratio=2&rotation=0&showTitle=false&size=120605&status=done&style=shadow&taskId=ueb2b1a07-816b-4d20-ac9b-cc8ef1b38c3&title=&width=575)
> 插件提供了几种类型，选取自己想用的即可

![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1676080457242-1780dcfd-cd83-4699-94c4-d566ac9b22fb.png#averageHue=%23282f38&clientId=ud9ae09ec-822f-4&from=paste&height=509&id=uc69f2928&name=image.png&originHeight=1017&originWidth=736&originalType=binary&ratio=2&rotation=0&showTitle=false&size=81455&status=done&style=shadow&taskId=u1c4928bb-97ec-4bc8-ab0a-418648e025c&title=&width=368)
## 测试-清新简洁版
**javaCode**
```java
import com.linmu.mybatis.mapper.UserMapper;
import com.linmu.mybatis.pojo.User;
import com.linmu.mybatis.pojo.UserExample;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class Test01 {

    public UserMapper mapperUtil() {
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        } catch (Exception e) {
            e.printStackTrace();
        }
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper;
    }

    @Test
    public void testMybatis() {
        UserMapper userMapper = mapperUtil();
        List<User> users = userMapper.selectByExample(null);
        users.forEach(user -> System.out.println(user));
    }
}

```
**SQLCode**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1676083870869-b2d3a7a3-7588-4681-b8ad-eb8682d37397.png#averageHue=%232a2e37&clientId=ud9ae09ec-822f-4&from=paste&height=169&id=u0b1ce737&name=image.png&originHeight=338&originWidth=2405&originalType=binary&ratio=2&rotation=0&showTitle=false&size=90330&status=done&style=shadow&taskId=u65631d5f-1c8f-4c88-83ff-2369b3ed1b3&title=&width=1202.5)

## 测试-奢华尊享版
**javaCode-1**
```java
import com.linmu.mybatis.mapper.UserMapper;
import com.linmu.mybatis.pojo.User;
import com.linmu.mybatis.pojo.UserExample;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class Test01 {

    public UserMapper mapperUtil() {
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        } catch (Exception e) {
            e.printStackTrace();
        }
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper;
    }

    @Test
    public void test01() {
        UserMapper userMapper = mapperUtil();
        // 创建条件对象
        UserExample userExample = new UserExample();
        // 添加条件
        userExample.createCriteria().andUserAgeEqualTo(25);
        List<User> users = userMapper.selectByExample(userExample);
        System.out.println(users);
    }
}

```
**SQLCode-1**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1676084020170-d0e41369-e57d-429c-90a4-60fe43bef0f7.png#averageHue=%23292e36&clientId=ud9ae09ec-822f-4&from=paste&height=202&id=u13b571d3&name=image.png&originHeight=404&originWidth=2448&originalType=binary&ratio=2&rotation=0&showTitle=false&size=112453&status=done&style=shadow&taskId=ub5e3ec55-9312-4ab5-bc44-d963387c64b&title=&width=1224)

**javaCode-2**
```java
import com.linmu.mybatis.mapper.UserMapper;
import com.linmu.mybatis.pojo.User;
import com.linmu.mybatis.pojo.UserExample;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.InputStream;
import java.util.List;

public class Test01 {

    public UserMapper mapperUtil() {
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        } catch (Exception e) {
            e.printStackTrace();
        }
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = sqlSessionFactoryBuilder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper;
    }

    @Test
    public void test02() {
        UserMapper userMapper = mapperUtil();
        // 创建条件对象
        UserExample userExample = new UserExample();
        // 添加条件
        userExample.createCriteria().andUserAgeEqualTo(25).andUserNameEqualTo("汪弘文");
        userExample.or().andUserAgeBetween(20,30).andUserSexEqualTo("男");
        List<User> users = userMapper.selectByExample(userExample);
        users.forEach(user -> System.out.println(user));
    }
}

```
**SQLCode-2**
```
0    [main] DEBUG org.apache.ibatis.logging.LogFactory  - Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
12   [main] DEBUG org.apache.ibatis.datasource.pooled.PooledDataSource  - PooledDataSource forcefully closed/removed all connections.
12   [main] DEBUG org.apache.ibatis.datasource.pooled.PooledDataSource  - PooledDataSource forcefully closed/removed all connections.
12   [main] DEBUG org.apache.ibatis.datasource.pooled.PooledDataSource  - PooledDataSource forcefully closed/removed all connections.
12   [main] DEBUG org.apache.ibatis.datasource.pooled.PooledDataSource  - PooledDataSource forcefully closed/removed all connections.
109  [main] DEBUG org.apache.ibatis.transaction.jdbc.JdbcTransaction  - Opening JDBC Connection
237  [main] DEBUG org.apache.ibatis.datasource.pooled.PooledDataSource  - Created connection 1287934450.
237  [main] DEBUG com.linmu.mybatis.mapper.UserMapper.selectByExample  - ==>  Preparing: select user_id, user_name, user_age, user_sex from users WHERE ( user_age = ? and user_name = ? ) or( user_age between ? and ? and user_sex = ? )
268  [main] DEBUG com.linmu.mybatis.mapper.UserMapper.selectByExample  - ==> Parameters: 25(Integer), 汪弘文(String), 20(Integer), 30(Integer), 男(String)
285  [main] DEBUG com.linmu.mybatis.mapper.UserMapper.selectByExample  - <==      Total: 4
User{userId=10, userName='曹烨磊', userAge=30, userSex='男'}
User{userId=12, userName='汪弘文', userAge=25, userSex='女'}
User{userId=15, userName='李哲瀚', userAge=22, userSex='男'}
User{userId=18, userName='覃伟祺', userAge=28, userSex='男'}

Process finished with exit code 0

```
> **奢华尊享版中的选择性修改（*Selective()）中，若传入的是null（空值），则此字段将不会被修改**

# mybatis-分页插件
## 添加分页插件
### 添加依赖
```xml
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.2.0</version>
</dependency>
```
### 配置分页插件
```xml
<plugins>
  <!--设置分页插件-->
  <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

## 分页插件的使用

### 开启分页功能

- 在查询功能之前使用`PageHelper.startPage(int pageNum, int pageSize)`开启分页功能 
   - pageNum：当前页的页码
   - pageSize：每页显示的条数

```java
@Test
public void testPageHelper() throws IOException {
	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
	SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
	SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	//访问第一页，每页四条数据
	PageHelper.startPage(1,4);
	List<Emp> emps = mapper.selectByExample(null);
	emps.forEach(System.out::println);
}
```
![分页测试结果.png](https://cdn.nlark.com/yuque/0/2023/png/34904523/1675753959785-ea62d0a2-843d-4711-9bf7-c703ef687c3d.png#averageHue=%232c313b&clientId=ud5bebd32-7cc5-4&from=paste&height=130&id=u4cbfd0b8&name=%E5%88%86%E9%A1%B5%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C.png&originHeight=259&originWidth=1424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71555&status=done&style=shadow&taskId=u3b27bd62-dca7-4961-b23f-0a824a93ac5&title=&width=712)
### 分页相关数据

#### 方法一：直接输出

```java
@Test
public void testPageHelper() throws IOException {
	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
	SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
	SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	//访问第一页，每页四条数据
	Page<Object> page = PageHelper.startPage(1, 4);
	List<Emp> emps = mapper.selectByExample(null);
	//在查询到List集合后，打印分页数据
	System.out.println(page);
}
```

- 分页相关数据：
```
Page{count=true, pageNum=1, pageSize=4, startRow=0, endRow=4, total=8, pages=2, reasonable=false, pageSizeZero=false}[Emp{eid=1, empName='admin', age=22, sex='男', email='456@qq.com', did=3}, Emp{eid=2, empName='admin2', age=22, sex='男', email='456@qq.com', did=3}, Emp{eid=3, empName='王五', age=12, sex='女', email='123@qq.com', did=3}, Emp{eid=4, empName='赵六', age=32, sex='男', email='123@qq.com', did=1}]
```
 

#### 方法二使用PageInfo

- 在查询获取list集合之后，使用`PageInfo<T> pageInfo = new PageInfo<>(List<T> list, intnavigatePages)`获取分页相关数据 
   - list：分页之后的数据
   - navigatePages：导航分页的页码数

```java
@Test
public void testPageHelper() throws IOException {
	InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
	SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
	SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
	SqlSession sqlSession = sqlSessionFactory.openSession(true);
	EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
	PageHelper.startPage(1, 4);
	List<Emp> emps = mapper.selectByExample(null);
	PageInfo<Emp> page = new PageInfo<>(emps,5);
	System.out.println(page);
}
```

- 分页相关数据：
```
PageInfo{
pageNum=1, pageSize=4, size=4, startRow=1, endRow=4, total=8, pages=2, 
list=Page{count=true, pageNum=1, pageSize=4, startRow=0, endRow=4, total=8, pages=2, reasonable=false, pageSizeZero=false}[Emp{eid=1, empName='admin', age=22, sex='男', email='456@qq.com', did=3}, Emp{eid=2, empName='admin2', age=22, sex='男', email='456@qq.com', did=3}, Emp{eid=3, empName='王五', age=12, sex='女', email='123@qq.com', did=3}, Emp{eid=4, empName='赵六', age=32, sex='男', email='123@qq.com', did=1}], 
prePage=0, nextPage=2, isFirstPage=true, isLastPage=false, hasPreviousPage=false, hasNextPage=true, navigatePages=5, navigateFirstPage=1, navigateLastPage=2, navigatepageNums=[1, 2]}
```
 

- 其中list中的数据等同于方法一中直接输出的page数据
- 在查询功能之前使用PageHelper.startPage(int pageNum, int pageSize)开启分页功能 

pageNum：当前页的页码 
pageSize：每页显示的条数 

- 在查询获取list集合之后，使用PageInfo<T> pageInfo = new PageInfo<>(List<T> list, int 

navigatePages)获取分页相关数据 
list：分页之后的数据 
navigatePages：导航分页的页码数 

- 分页相关数据 

PageInfo{ 
pageNum=8, pageSize=4, size=2, startRow=29, endRow=30, total=30, pages=8, 
list=Page{count=true, pageNum=8, pageSize=4, startRow=28, endRow=32, total=30, 
pages=8, reasonable=false, pageSizeZero=false}, 
prePage=7, nextPage=0, isFirstPage=false, isLastPage=true, hasPreviousPage=true, 
hasNextPage=false, navigatePages=5, navigateFirstPage4, navigateLastPage8, 
navigatepageNums=[4, 5, 6, 7, 8] 
} 

常用数据：
pageNum：当前页的页码 
pageSize：每页显示的条数 
size：当前页显示的真实条数 
total：总记录数 
pages：总页数 
prePage：上一页的页码 
nextPage：下一页的页码 
isFirstPage/isLastPage：是否为第一页/最后一页 
hasPreviousPage/hasNextPage：是否存在上一页/下一页 
navigatePages：导航分页的页码数 
navigatepageNums：导航分页的页码，[1,2,3,4,5]
