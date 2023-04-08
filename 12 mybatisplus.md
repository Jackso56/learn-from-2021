> mybatisplus官网地址：[https://baomidou.com/pages/24112f/](https://baomidou.com/pages/24112f/)

# 简介
为了提高开发效率，在mybatis的基础上只做增强，不做修改
支持多种数据库


# 实现低代码开发两种方式
## 继承BaseMapper
### 前置工作
```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.32</version>
</dependency>

<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.5.3</version>
</dependency>
```

```sql
create table user(
id bigint primary key,
name varchar(255),
age int
)
```

```java
package com.jason.mbp04.pojo;

import com.baomidou.mybatisplus.annotation.TableName;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/8 08:11:30
 **/
@SuppressWarnings({"all"})
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("user")
public class User {
    private Long id;
    private String name;
    private Integer age;

    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}

```

```java
package com.jason.mbp04.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.jason.mbp04.pojo.User;
import org.springframework.stereotype.Repository;

@Repository
public interface UserMapper extends BaseMapper<User> {

}

```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.jason.mbp04.mapper.UserMapper"> <!--参数类型绑定-->
  	// 可以实现自定义SQL
</mapper>
```

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 加上双引号，区分字符窜，否则会报错
    # Access denied for user 'root'@'localhost' (using password: YES)
    username: "root"
    password: "000000"
    url: jdbc:mysql://localhost:3306/db1?serverTimezone=GMT%2b8

#为 mybatis plus 添加日志功能
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

### 基本crud
```java
package com.jason.mbp04;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.mbp04.mapper.UserMapper;
import com.jason.mbp04.pojo.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
class Mbp04ApplicationTests {

    @Autowired
    private UserMapper userMapper;

    @Test
    void contextLoads() {
        // 查询所有数据：SELECT id,name,age FROM user
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }

    @Test
    void test01(){
        // 根据id查询数据  SELECT id,name,age FROM user WHERE id=?
        // 也可以通过传入列表的方式查询多条数据
        User user = userMapper.selectById(1);
        System.out.println(user);
    }

    @Test
    void test02(){
        // 插入数据 INSERT INTO user ( id, name, age ) VALUES ( ?, ?, ? )
        int result = userMapper.insert(new User("Jasoncon", 23));
        System.out.println(result);
    }

    @Test
    void test03(){
        // update data: UPDATE user SET name=?, age=? WHERE (id = ?)
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("id",8L);
        int result = userMapper.update(new User("Black", 30), userQueryWrapper);
        System.out.println(result);
    }

    @Test
    void test04(){
        // delete data:DELETE FROM user WHERE (name = ?)
        // 也可以通过传入列表的方式实现批量删除数据
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("name","Black");
        int result = userMapper.delete(userQueryWrapper);
        System.out.println(result);
    }

}

```

## 继承官方接口实现类
> **说明:**
> - 通用 Service CRUD 封装[IService(opens new window)](https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/service/IService.java)接口，进一步封装 CRUD 采用 get 查询单行 remove 删除 list 查询集合 page 分页 前缀命名方式区分 Mapper 层避免混淆，
> - 泛型 T 为任意实体对象
> - 建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承 Mybatis-Plus 提供的基类
> - 对象 Wrapper 为 [条件构造器](https://baomidou.com/01.%E6%8C%87%E5%8D%97/02.%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD/wrapper.html)

### 前置工作
```java
package com.jason.mbp01.service;

import com.baomidou.mybatisplus.extension.service.IService;
import com.jason.mbp01.pojo.User;

/**
 * 自定义接口，继承官方提供的IService接口，泛型为操作的实体类对象
 */
public interface UserService extends IService<User> {
    
}

```

```java
package com.jason.mbp01.service.imp;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.jason.mbp01.mapper.UserMapper;
import com.jason.mbp01.pojo.User;
import com.jason.mbp01.service.UserService;
import org.springframework.stereotype.Service;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/5 08:28:50
 **/
@SuppressWarnings({"all"})
// 实现自定义接口，继承官方实现的接口，并且实现自定义的接口
@Service
public class UserServiceImp extends ServiceImpl<UserMapper, User> implements UserService {

}

```
### 基本的crud
```java
package com.jason.mbp04;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.mbp04.pojo.User;
import com.jason.mbp04.service.imp.MyUserMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/8 13:33:16
 **/
@SuppressWarnings({"all"})
@SpringBootTest
public class TestIService {

    @Autowired
    private MyUserMapper myUserMapper;

    @Test
    public void test01(){
        // 查询数据： SELECT id,name,age FROM user
        List<User> list = myUserMapper.list();
        list.forEach(System.out::println);
    }

    @Test
    public void test02(){
        // 插入数据: INSERT INTO user ( id, name, age ) VALUES ( ?, ?, ? )
        boolean result = myUserMapper.save(new User("Jason", 77));
        System.out.println(result);
    }

    @Test
    public void test03(){
        // 更新数据：UPDATE user SET name=?, age=? WHERE (name = ?)
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("name","Jason");
        User user = new User("Jason Black", 21);
        boolean result = myUserMapper.update(user,userQueryWrapper);
        System.out.println(result);
    }

    @Test
    public void test04(){
        // 删除数据：DELETE FROM user WHERE (id > ?)
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.gt("id",20);
        boolean result = myUserMapper.remove(userQueryWrapper);
        System.out.println(result);
    }
}

```


# 其他操作
## 常用注解
> - @TableName：实体类管理数据表
> - @TableId：类属性关联主键
> - @TableField：类属性关联表字段
> - @TableLogic：逻辑删除

```java
package com.jason.mbp01.pojo;

import com.baomidou.mybatisplus.annotation.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/4 20:24:48
 **/
@SuppressWarnings({"all"})
@Data
@AllArgsConstructor
@NoArgsConstructor
// 设置实体类与数据表的对应关系
//@TableName("users")
public class User {
    // 将实体类对应的属性设置为主键
    // 其中value属性为可设置实体类属性与表字段的对应关系
    // type = IdType.AUTO  设置自增主键，数据表中必须同时设置自增
    // 默认值为 type = IdType.ASSIGN_ID 表示雪花算法生成主键
    // @TableId(value="User_id",type = IdType.AUTO)
    @TableId("user_id")
    private Long id;

    // 用于设置实体类中属性与表字段的对应关系
    @TableField(value = "user_name")
    private String name;
    private int age;
    private String email;
    @TableLogic
    @TableField(value = "user_delete")
    // 命名时不能与语言的关键字相互冲突，如：delete
    private int userDelete;

    public User(String name, int age, String email){
        this.name = name;
        this.age = age;
        this.email = email;
    }
}

```

## 条件构造器
> QueryWapper、UpdateWapper、LambdaQueryWrapper

```java
package com.jason.mbp01;

import ch.qos.logback.core.rolling.helper.IntegerTokenConverter;
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
import com.jason.mbp01.pojo.User;
import com.jason.mbp01.service.imp.UserServiceImp;
import org.junit.jupiter.api.Test;
import org.junit.platform.commons.util.StringUtils;
import org.mockito.internal.util.StringUtil;
import org.omg.PortableInterceptor.INACTIVE;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.AutoConfigureOrder;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;
import java.util.Map;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/5 10:03:19
 **/
@SuppressWarnings({"all"})
@SpringBootTest
public class MyPlWaTe {

    @Autowired
    private UserServiceImp userServiceImp;

    @Test
    public void test01() {
        // 条件构造器，查询数据  ELECT user_id AS id,user_name AS name,age,email,user_delete
        //      FROM tmp_user WHERE user_delete=0 AND (age BETWEEN ? AND ? AND user_name LIKE ?)
        // 需要注意的是，这里的字段名必须与数据表中的字段名一致
        QueryWrapper<User> wapper = new QueryWrapper<>();
        wapper.between("age", 20, 30)
                .like("user_name", "k");
        List<User> list = userServiceImp.list(wapper);
        list.forEach(System.out::println);
    }

    @Test
    public void test03() {
        // 使用条件构造器进行排序操作
        // SELECT user_id AS id,user_name AS name,age,email,user_delete FROM tmp_user
        // WHERE user_delete=0 ORDER BY user_id DESC,age DESC
        QueryWrapper<User> wapper = new QueryWrapper<>();
        wapper.orderByDesc("user_id")
                .orderByDesc("age");
        List<User> list = userServiceImp.list(wapper);
        list.forEach(System.out::println);
    }


    @Test
    public void test04() {
        // 使用条件构造器实现删除操作
        // SELECT user_id AS id,user_name AS name,age,email,user_delete FROM tmp_user
        // WHERE user_delete=0 AND (user_name = ?)
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("user_name", "Jason Black");
        boolean remove = userServiceImp.remove(userQueryWrapper);
        System.out.println(remove);
    }

    @Test
    public void test05() {
        // 条件构造器实现修改操作
        // UPDATE tmp_user SET user_name=?, age=? WHERE user_delete=0 AND (user_name = ? OR user_name = ?)
        // 条件构造器默认使用and连接多条件查询
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("user_name", "Jason Black")
                .or()
                .eq("user_name", "Mike12");
        User user = new User();
        user.setName("Jason");
        user.setAge(23);
        boolean update = userServiceImp.update(user, userQueryWrapper);
        System.out.println("update=" + update);
    }

    @Test
    public void test06() {
        // 通过lambda表达式来设置条件的优先级
        // UPDATE tmp_user SET user_name=?, age=?, email=?
        // WHERE user_delete=0 AND (user_name LIKE ?
        // AND (email IS NULL AND age = ?))
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.like("user_name", "J")
                .and(com -> com.isNull("email")
                        .eq("age", 20));
        User user = new User("Tom", 22, "www@qq.com");
        boolean result = userServiceImp.update(user, userQueryWrapper);
        System.out.println("result=" + result);
    }

    @Test
    public void test07() {
        // 查询所有数据
        // SELECT user_name,age,email FROM tmp_user WHERE user_delete=0
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.select("user_name", "age", "email");
        List<Map<String, Object>> maps = userServiceImp.listMaps(userQueryWrapper);
        maps.forEach(System.out::println);
    }

    @Test
    public void test08() {
        // 子查询
        //  SELECT user_id AS id,user_name AS name,age,email,user_delete
        //  FROM tmp_user WHERE user_delete=0 AND
        //  (user_id IN (select user_id from tmp_user where user_id < 6))
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.inSql("user_id",
                "select user_id from tmp_user where user_id < 6");
        List<User> list = userServiceImp.list(userQueryWrapper);
        list.forEach(System.out::println);
    }

    @Test
    public void test09() {
        // UpdateWrapper设置修改值
        // UPDATE tmp_user SET user_name=?,email=?
        // WHERE user_delete=0 AND (user_id < ?)
        UpdateWrapper<User> userUpdateWrapper = new UpdateWrapper<>();
        userUpdateWrapper.lt("user_id", 7);
        userUpdateWrapper.set("user_name", "Jason")
                .set("email", "Jason@123.com");
        boolean result = userServiceImp.update(null, userUpdateWrapper);
        System.out.println("result=" + result);
    }

    @Test
    public void test10() {
        // 选择性拼接SQL
        // SELECT user_id AS id,user_name AS name,age,email,user_delete
        // FROM tmp_user WHERE user_delete=0 AND (age > ? AND user_name = ?)
        Integer age = 20;
        String userName = "Jason";
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.gt(age != null, "age",age)
                .eq(StringUtils.isNotBlank(userName),"user_name",userName);
        List<User> list = userServiceImp.list(userQueryWrapper);
        list.forEach(System.out::println);
    }

    @Test
    public void test11(){
        // LambdaQueryWrapper/LambdaUpdateWrapper 通过实体类获取属性对应的字段名，为了防止写错字段名
        // SELECT user_id AS id,user_name AS name,age,email,user_delete
        // FROM tmp_user WHERE user_delete=0 AND (age > ? AND user_name = ?)
        Integer age = 20;
        String userName = "Jason";
        LambdaQueryWrapper<User> userLambdaQueryWrapper = new LambdaQueryWrapper<>();
        userLambdaQueryWrapper.gt(age!=null,User::getAge,age)
                .eq(StringUtils.isNotBlank(userName),User::getName,userName);
        List<User> list = userServiceImp.list(userLambdaQueryWrapper);
        list.forEach(System.out::println);
    }
}

```

## 数据分页
```java
package com.jason.mbp01.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/6 18:58:02
 **/
@SuppressWarnings({"all"})
// 扫描接口
@MapperScan("com.jason.mbp01.mapper")
@org.springframework.context.annotation.Configuration
public class Configuration {

    /**
     * 配置分页插件
     * @return
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        // 配置数据库类型
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return mybatisPlusInterceptor;
    }


}
```

```java
package com.jason.mbp01;

import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.jason.mbp01.pojo.User;
import com.jason.mbp01.service.imp.UserServiceImp;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/6 19:01:00
 **/
@SuppressWarnings({"all"})
@SpringBootTest
public class TestPage {

    @Autowired
    private UserServiceImp userServiceImp;


    @Test
    public void test01(){
        // current表示当前页，size表示每一页的总条记录数
        Page<User> page = new Page<User>(2,3);
        userServiceImp.page(page,null);
        System.out.println(page.getPages());
        System.out.println(page.getRecords());
        System.out.println(page.getSize());
        System.out.println(page.getCurrent());
        System.out.println(page.getTotal());
        System.out.println(page.hasPrevious());
    }
}
```

## 配置文件其他配置
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 000000
    url: jdbc:mysql://localhost:3306/db1?serverTimezone=UTC

#为 mybatis plus 添加日志功能
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # mapper映射文件默认位置，可以不设置
  mapper-locations: classpath:/mapper/userMapper.xml
  # MybatisPlus 全局配置
  global-config:
    db-config:
      # 区局配置，设置表的前缀
      table-prefix: tmp_
      # 在全局范围内设置主键自增
      id-type: auto
  # 设置类型别名
  type-aliases-super-type: com.jason.mbp01
```

## 支持反向工程、同用枚举、支持mapper接口在maper.xml中自动生成SQL



