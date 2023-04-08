# 概要
> 官网：spring.io

Spring Security是一个提供身份验证、授权和针对常见攻击的保护的框架。可以无缝对接spring框架


# 身份验证
## 方式一：配置文件方式
```properties
spring.security.user.name=jason
spring.security.user.password=jason
```

## 方式二：实现接口、编写配置类
```java
package com.jason.ss1.service;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:18:34
 **/
@SuppressWarnings({"all"})
@Service("userDetailService")
public class LoginSer implements UserDetailsService {


    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 设置权限列表
        List<GrantedAuthority> authorities =
                AuthorityUtils.commaSeparatedStringToAuthorityList("a1");
        // 返回身份验证实体：设置用户名、加密密码
        return new User("black",new BCryptPasswordEncoder().encode("black"),
                authorities);
    }
}
```

```java
package com.jason.ss1.cong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config_ extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    // 这里的userDetailsService来自服务层，该类实现了UserDetailsService接口
    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).
        passwordEncoder(passwordEncoder());
    }
}

```

## 方式三：查询数据库实现身份验证
```sql
-- 创建数据表
create table user(
  id int primary key auto_increment comment "用户唯一标识",
  username varchar(255) comment "用户名"，
  password varchar(255) comment "密码"
)

-- 插入数据
insert into user values(null,"jason","jason"),(null,"black","black")

```

```java
package com.jason.ss1.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:20:34
 **/
@SuppressWarnings({"all"})
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String username;
    private String password;

    public User(String username, String password){
        this.username = username;
        this.password = password;
    }
}
```

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=000000
```

```java
package com.jason.ss1.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.jason.ss1.pojo.User;
import org.springframework.stereotype.Repository;

@Repository // 接口标识
public interface UserMapper extends BaseMapper<User> {
    // 注意这里是继承接口
}

```

```java
package com.jason.ss1.service;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.ss1.mapper.UserMapper;
import com.jason.ss1.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:18:34
 **/
@SuppressWarnings({"all"})
@Service("userDetailService")
public class LoginSer implements UserDetailsService {

    @Autowired // 注入接口
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 调用userMappper方法查询数据库  创建条件
        QueryWrapper<User> wrapper = new QueryWrapper<User>();
        wrapper.eq("username",username);
        // 调用方法查询数据库
        User user = userMapper.selectOne(wrapper);
        // 判断用户是否存在
        if(user == null){
            throw new UsernameNotFoundException("user is not found..");
        }
        List<GrantedAuthority> authorities = AuthorityUtils.commaSeparatedStringToAuthorityList("a1");
        return new org.springframework.security.core.userdetails.User(user.getUsername(),new BCryptPasswordEncoder().encode(user.getPassword()),authorities);
    }
}

```

```java
package com.jason.ss1;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication(scanBasePackages = "com.jason.ss1")
// 添加接口mapper扫描地址
@MapperScan("com.jason.ss1.mapper")
public class Ss1Application {

    public static void main(String[] args) {
        SpringApplication.run(Ss1Application.class, args);
    }
}

```

# 授权
## 基于权限
> hasAutority()、hasAnyAuthority()

```java
package com.jason.ss3.conf;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin()  // 使用自定义的登录页面
        .loginPage("/login.html")  // 自定义登录页面的名称
        .loginProcessingUrl("/testlogin")  // 登录访问的路径
        .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
        .permitAll()  // 表示允许访问
        .and()  // 哪些请求可以直接访问
        .authorizeHttpRequests()
        .antMatchers("/").permitAll()
        // antMatchers() 用于设置访问路径   hasAuthority() 用于设置访问权限  
        // 设置一个访问权限
        // .antMatchers("/test01").hasAuthority("a1")
        // 设置多个访问权限
        .antMatchers("/test01").hasAnyAuthority("a1","a2")
        .anyRequest().authenticated() // 所有请求都可以访问
        .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailService).
        passwordEncoder(passwordEncoder());
    }
}

```

```java
package com.jason.ss3.service;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.ss3.mapper.UserMapper;
import com.jason.ss3.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:18:34
 **/
@SuppressWarnings({"all"})
@Service("userDetailService")
public class LoginSer implements UserDetailsService {

    @Autowired // 注入接口
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 调用userMappper方法查询数据库  创建条件
        QueryWrapper<User> wrapper = new QueryWrapper<User>();
        wrapper.eq("username",username);
        // 调用方法查询数据库
        User user = userMapper.selectOne(wrapper);
        // 判断用户是否存在
        if(user == null){
            throw new UsernameNotFoundException("user is not found..");
        }
        List<GrantedAuthority> authorities =
                AuthorityUtils.commaSeparatedStringToAuthorityList("a1");
        return new org.springframework.security.core.userdetails.User
                (user.getUsername(),new BCryptPasswordEncoder().
                        encode(user.getPassword()),authorities);
    }
}

```

## 基于角色
> hasRole()、hasAnyRole()
> 在配置文件中需要将角色添加前缀  ROLE_     因为security底层是这么封装的

```java
package com.jason.ss3.conf;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin()  // 使用自定义的登录页面
                .loginPage("/login.html")  // 自定义登录页面的名称
                .loginProcessingUrl("/testlogin")  // 登录访问的路径
                .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
                .permitAll()  // 表示允许访问
                .and()  // 哪些请求可以直接访问
                .authorizeHttpRequests()
                .antMatchers("/").permitAll()
                // antMatchers() 用于设置访问角色   hasRole() 用于设置访问角色
                // 设置一个访问角色
                // .antMatchers("/test01").hasRole("a2")
                // 同时设置多个访问角色
                .antMatchers("/test01").hasAnyRole("a1","a2")
                .anyRequest().authenticated() // 所有请求都可以访问
                .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailService).
                passwordEncoder(passwordEncoder());
    }
}

```

```java

package com.jason.ss3.service;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.ss3.mapper.UserMapper;
import com.jason.ss3.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:18:34
 **/
@SuppressWarnings({"all"})
@Service("userDetailService")
public class LoginSer implements UserDetailsService {

    @Autowired // 注入接口
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 调用userMappper方法查询数据库  创建条件
        QueryWrapper<User> wrapper = new QueryWrapper<User>();
        wrapper.eq("username",username);
        // 调用方法查询数据库
        User user = userMapper.selectOne(wrapper);
        // 判断用户是否存在
        if(user == null){
            throw new UsernameNotFoundException("user is not found..");
        }
        List<GrantedAuthority> authorities =
                AuthorityUtils.commaSeparatedStringToAuthorityList("a1,ROLE_a1");
        return new org.springframework.security.core.userdetails.User
                (user.getUsername(),new BCryptPasswordEncoder().
                        encode(user.getPassword()),authorities);
    }
}

```

# 自定义设置
## 自定义登录页面
```java
package com.jason.ss1.cong;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config_ extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    // 自定义登录页面核心方法
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin()  // 使用自定义的登录页面
                .loginPage("/login.html")  // 自定义登录页面的名称
                .loginProcessingUrl("/loginss")  // 登录访问的路径，由security管理，随便写
                .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
                .permitAll()  // 表示允许访问
                .and()  // 哪些请求可以直接访问
                .authorizeHttpRequests().antMatchers("/testlogin","/","/hello").permitAll()
                .anyRequest().authenticated() // 所有请求都可以访问
                .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).
                passwordEncoder(passwordEncoder());
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
    <div> <!--方法必须为post-->
      <form action="/testlogin" method="post">
        <p>  <!--name必须为username-->
          username:<input type="text" name="username">
        </p>
        <p>  <!--name必须为password-->
          password:<input type="password" name="password">
        </p>
        <input type="submit" value="submit">
      </form>
    </div>
  </body>
</html>
```

## 自定义无权限访问页面
```java
package com.jason.ss3.conf;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 指定无权限访问页面
        http.exceptionHandling().accessDeniedPage("/failure.html");
        http.formLogin()  // 使用自定义的登录页面
        .loginPage("/login.html")  // 自定义登录页面的名称
        .loginProcessingUrl("/testlogin")  // 登录访问的路径
        .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
        .permitAll()  // 表示允许访问
        .and()  // 哪些请求可以直接访问
        .authorizeHttpRequests()
        .antMatchers("/").permitAll()
        // antMatchers() 用于设置访问路径   hasAuthority() 用于设置访问权限
        .antMatchers("/test01").hasAuthority("a1")
        // .antMatchers("/test01").hasAnyAuthority("a1","a2")
        // antMatchers() 用于设置访问角色   hasRole() 用于设置访问角色
        // .antMatchers("/test01").hasRole("a1")
        // .antMatchers("/test01").hasAnyRole("a1","a2")
        .anyRequest().authenticated() // 所有请求都可以访问
        .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailService).
        passwordEncoder(passwordEncoder());
    }
}

```

```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <h1>without authority...</h1>
</body>
</html>
```

## 注解方式实现身份验证和授权
```java

package com.jason.ss3;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

@SpringBootApplication(scanBasePackages = "com.jason.ss3")
// 添加接口mapper地址
@MapperScan("com.jason.ss3.mapper")
// 开启注解   该注解可以放在启动类上，也可以放在配置类上
// securedEnabled = true 开启Secured注解识别
// prePostEnabled = true  开启PreAuthorize和PostAuthorize注解识别
@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled = true)
public class Ss3Application {

    public static void main(String[] args) {
        SpringApplication.run(Ss3Application.class, args);
    }

}
```

```java
package com.jason.ss3.controller;

import org.springframework.security.access.annotation.Secured;
import org.springframework.security.access.prepost.PostAuthorize;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/2 13:29:56
 **/
@SuppressWarnings({"all"})
@RestController
public class Test01 {

    @RequestMapping("/testSecured")
    // 判断是否具有其中任意一个访问角色，可设置多个
    @Secured({"ROLE_a1","ROLE_a2"})
    public String testSecured(){
        return "test secured successfully...";
    }

    @RequestMapping("/testPreAuthorize")
    // 方法执行前判断是否具有访问权限
    @PreAuthorize("hasAnyAuthority('a2')")
    public String testPreAuthorize(){
        return "test PreAuthorize successfully...";
    }

    @RequestMapping("/testPostAuthorize")
    // 方法执行后判断是否具有访问权限
    @PostAuthorize("hasAnyAuthority('a1')")
    public String testPostAuthorize(){
        return "test PostAuthorize successfully...";
    }

}
```

```java
package com.jason.ss3.service;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.jason.ss3.mapper.UserMapper;
import com.jason.ss3.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:18:34
 **/
@SuppressWarnings({"all"})
@Service("userDetailService")
public class LoginSer implements UserDetailsService {

    @Autowired // 注入接口
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 调用userMappper方法查询数据库  创建条件
        QueryWrapper<User> wrapper = new QueryWrapper<User>();
        wrapper.eq("username",username);
        // 调用方法查询数据库
        User user = userMapper.selectOne(wrapper);
        // 判断用户是否存在
        if(user == null){
            throw new UsernameNotFoundException("user is not found..");
        }
        List<GrantedAuthority> authorities =
                AuthorityUtils.commaSeparatedStringToAuthorityList("a1,ROLE_a1");
        return new org.springframework.security.core.userdetails.User
                (user.getUsername(),new BCryptPasswordEncoder().
                        encode(user.getPassword()),authorities);
    }
}

```

## 自定义退出页面
```java
package com.jason.ss3.conf;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 设置退出页面
        http.logout().logoutUrl("/logout").logoutSuccessUrl("/logout/success");
        http.exceptionHandling().accessDeniedPage("/failure.html");
        http.formLogin()  // 使用自定义的登录页面
                .loginPage("/login.html")  // 自定义登录页面的名称
                .loginProcessingUrl("/testlogin")  // 登录访问的路径
                .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
                .permitAll()  // 表示允许访问
                .and()  // 哪些请求可以直接访问
                .authorizeHttpRequests()
                .antMatchers("/","/logout/success").permitAll()
                // antMatchers() 用于设置访问路径   hasAuthority() 用于设置访问权限
                 .antMatchers("/test01").hasAuthority("a1")
                // .antMatchers("/test01").hasAnyAuthority("a1","a2")
                // antMatchers() 用于设置访问角色   hasRole() 用于设置访问角色
                 // .antMatchers("/test01").hasRole("a1")
                // .antMatchers("/test01").hasAnyRole("a1","a2")
                .anyRequest().authenticated() // 所有请求都可以访问
                .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailService).
                passwordEncoder(passwordEncoder());
    }
}

```

```java
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<a href="/logout">logout...</a>
</body>
</html>
```

## 自动登录配置
```java
package com.jason.ss3.conf;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.authentication.rememberme.JdbcTokenRepositoryImpl;
import org.springframework.security.web.authentication.rememberme.PersistentTokenRepository;

import javax.sql.DataSource;

/**
 * @author Jason Black
 * @version The past cannot be redeemed, the future can be changed.
 * @CreateTime 2023/4/1 19:17:22
 **/
@SuppressWarnings({"all"})
@Configuration
public class Config extends WebSecurityConfigurerAdapter {

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 设置退出页面
        http.logout().logoutUrl("/logout").logoutSuccessUrl("/logout/success");
        http.exceptionHandling().accessDeniedPage("/failure.html");
        http.formLogin()  // 使用自定义的登录页面
                .loginPage("/login.html")  // 自定义登录页面的名称
                .loginProcessingUrl("/testlogin")  // 登录访问的路径
                .defaultSuccessUrl("/successfully")  // 登录成功后访问的路径
                .permitAll()  // 表示允许访问
                .and()  // 哪些请求可以直接访问
                .authorizeHttpRequests()
                .antMatchers("/","/logout/success").permitAll()
                // antMatchers() 用于设置访问路径   hasAuthority() 用于设置访问权限
                 .antMatchers("/test01").hasAuthority("a1")
                // .antMatchers("/test01").hasAnyAuthority("a1","a2")
                // antMatchers() 用于设置访问角色   hasRole() 用于设置访问角色
                 // .antMatchers("/test01").hasRole("a1")
                // .antMatchers("/test01").hasAnyRole("a1","a2")
                .anyRequest().authenticated() // 所有请求都可以访问
                .and().rememberMe().tokenRepository(persistentTokenRepository())
                .tokenValiditySeconds(999999) // 设置有效时长，单位秒
                .userDetailsService(userDetailService)
                .and().csrf().disable(); // 关闭csrf防护
    }

    @Autowired
    private UserDetailsService userDetailService;

    @Autowired // 注入数据源
    private DataSource dataSource;

    @Bean  // 配置对象
    public PersistentTokenRepository persistentTokenRepository(){
        JdbcTokenRepositoryImpl jdbcTokenRepository = new JdbcTokenRepositoryImpl();
        jdbcTokenRepository.setDataSource(dataSource);
        // 创建数据库，用于保存cookie数据
        // jdbcTokenRepository.setCreateTableOnStartup(true);
        return jdbcTokenRepository;
    }



    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailService).
                passwordEncoder(passwordEncoder());
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
    <div> <!--方法必须为post-->
      <form action="/testlogin" method="post">
        <p>  <!--name必须为username-->
          username:<input type="text" name="username">
        </p>
        <p>  <!--name必须为password-->
          password:<input type="password" name="password">
        </p>
        <input type="checkbox" name="remember-me">自动登录<br>
        <input type="submit" value="submit">
      </form>
    </div>
  </body>
</html>
```

# CSRF理解
**跨站请求伪造**（英语：Cross-site request forgery），也被称为 **one-click **
**attack **或者 **session riding**，通常缩写为 **CSRF **或者 **XSRF**， 是一种挟制用户在当前已 
登录的 Web 应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，**XSS **
利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。 

跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个 
自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买 
商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。 
这利用了 web 中用户身份验证的一个漏洞：**简单的身份验证只能保证请求发自某个用户的 **
**浏览器，却不能保证请求本身是用户自愿发出的**。 

从 Spring Security 4.0 开始，默认情况下会启用 CSRF 保护，以防止 CSRF 攻击应用 
程序，Spring Security CSRF 会针对 PATCH，POST，PUT 和 DELETE 方法进行防护。
