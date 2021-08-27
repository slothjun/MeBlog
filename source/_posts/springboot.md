---
title: springboot
---

## springboot

### 概述

springboot是spring的一个boot版本，它的出现是为了让你能更快的体验或使用spring。我经常把它看做spring的简单易用版。那他是从那几个方面去做到spring的简单易用的呢？

1.约定优于配置 ---自动配置
2.依赖自带了起步依赖的坐标。---起步依赖

### springboot_quick （dome）

1.pom构建：继承springbootparent ，起步依赖配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itcast</groupId>
    <artifactId>springboot_quick</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>

    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    
</project>
```
2.创建启动类
```Java
package com.itcast;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import javax.swing.*;

/**
 * @className MysoringBoot
 * @Descirption springboot boot class
 * @Author WengLiangBao
 * @Date 19-5-12 下午1:21
 * @Vsersion 1.0
 */
@SpringBootApplication
public class MysoringBoot {
	/*
	SpringApplication.run(MysoringBoot.class) 代表运行SpringBoot的启动类,参数为SpringBoot启动类的字节码对象
	*/
    public static void main(String[] args){
        SpringApplication.run(MysoringBoot.class);
    }
}

```
3.构建访问控制层controller
```Java
package com.itcast.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * @className QuickStartController
 * @Descirption quick dome contorller
 * @Author WengLiangBao
 * @Date 19-5-12 下午1:37
 * @Vsersion 1.0
 */
@Controller
public class QuickStartController {


    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功！";
    }
}

```
pom中添加热部署

```xml
<!-- 热部署-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```
### 起步依赖
### 自动配置

### springboot配置文件

SpringBoot是基于约定的,所以很多配置都有默认值,但如果想使用自己的配置替换默认配置的话,就可以使用application.properties或者application.yml(application.yaml)进行配置。

SpringBoot默认会从Resources目录下加载application.properties或application.yml(application.yaml)

上面提及过,SpringBoot的配置文件,主要的目的就是对配置信息进行修改的,但在配置时的key从哪里去查询
呢?我们可以查阅SpringBoot的官方文档
文档URL:https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties

### 配置文件与配置类的属性映射关系

#### @value

1.在application.yml 中添加如下属性
```yml
person:
	name: xiaoming
	age: 18
```
2.在对应类中使用
```Java
@Value("${person.name}")
private String name;
@Value("${person.age}")
private Integer age;
```
#### @configurationProperties
1.在application.yml 中添加如下属性
```yml
person:
	name: xiaoming
	age: 18
```
2.在对应的实体类中使用
```Java
@ConfigurationProperties(prefix = "person")
@Data
public class QuickStartController {


    private String name;

    private int age;
```
注意使用configurationProperties需要为指定的属性添加getting或setting ，本人使用lombok的@Data自动生成了getting，setting


### springboot整合

#### mybatis 整合

1.pom 中添加坐标
```xml
<!--由于spring官方没有对mybatis整合提供技术方案支持，所以这里需要使用mybatis提供的整合jar包，并指定版本-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
2.application.yml 中配置mybatis及数据库链接信息
```yml
#DB configuration
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/mybatis_study?useUnicode=true&charaterEncoding=utf-8&useSSL=true
    username: spider
    password: *****
#mybatis configuration
mybatis:
  mapper-locations: classpath:mapper/*Mapper.xml
```

3.创建dao控制层和mapper映射文件

dao层文件
```java
package com.itcast.dao;

import com.itcast.pojo.User;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

/**
 * @className UserMapper
 * @Descirption
 * @Author WengLiangBao
 * @Date 19-5-12 下午3:09
 * @Vsersion 1.0
 */
@Mapper
public interface UserMapper {

    public List<User> queryUserList();
}

```

Mapper ,编写位置对应application.yml中mapper-locations的位置，内容为
```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="com.itcast.dao.UserMapper">

    <select id="queryUserList" resultType="com.itcast.pojo.User">
        select * from user
    </select>
</mapper>
```

4.数据库对应pojo编写
```Java
package com.itcast.pojo;

import lombok.Data;

/**
 * @className User
 * @Descirption
 * @Author WengLiangBao
 * @Date 19-5-12 下午3:07
 * @Vsersion 1.0
 */
@Data
public class User {

    private Integer id;

    private String username;

    private String password;

    private String name;

}

```
#### Junit 整合

1.pom中添加Junit起步依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
</dependency>
```

2.编写测试类
```Java
package com.itcast.daoTest;

import com.itcast.MysoringBoot;
import com.itcast.dao.UserMapper;
import com.itcast.pojo.User;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.ArrayList;
import java.util.List;

/**
 * @className UserMapperTest
 * @Descirption 用户映射测试
 * @Author WengLiangBao
 * @Date 19-5-12 下午3:50
 * @Vsersion 1.0
 */
@RunWith(SpringRunner.class)
@SpringBootTest(classes = MysoringBoot.class)//程序启动入口类
public class UserMapperTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void queryListUser(){
        List<User> users = userMapper.queryUserList();
        Assert.assertNotNull(users);
        List<User> array = new ArrayList<User>();
        array.add(new User(1,"zhangsan","123","张三"));
        array.add(new User(2,"lisi","123","李四"));
        System.out.print(users);
        Assert.assertArrayEquals(users.toArray(),array.toArray());
    }
}

```

#### redis 整合

1.添加起步依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
2.application.yml 中配置redis所需属性
```yml
spring:
  redis:
    host: xx.104.xxx.29
    port: 6379
```
3.编写测试类测试
```Java
    @Autowired
    private UserMapper userMapper;

    @Autowired
    private RedisTemplate<String,String> redisTemplate;
    
    @Test
    public void redisTest() throws JsonProcessingException {
        String userListData = redisTemplate.boundValueOps("user.findAll").get();
        if(userListData==null){
            List<User> users = userMapper.queryUserList();
            String uD = new ObjectMapper().writeValueAsString(users);
            userListData = uD;
            redisTemplate.boundValueOps("user.findAll").set(uD);
            System.out.print("从数据库中获取");
        }else {
            System.out.print("从redis缓存中获取");
        }
        System.out.print(userListData);
    }
```

#### springJPA 整合
1.添加spring Data JPA起步依赖及其所需的一些坐标
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>
```
2.application.yml中配置数据库及jpa的相关属性
```properties
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/mybatis_study?useUnicode=true&charaterEncoding=utf-8&useSSL=true
spring.datasource.username=spider
spring.datasource.password=*****

spring.jpa.database=MySQL
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy
```
3.创建pojo并映射
```Java
package com.example.demo.pojo;

import lombok.Data;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

/**
 * @className User
 * @Descirption
 * @Author WengLiangBao
 * @Date 19-5-12 下午4:58
 * @Vsersion 1.0
 */
@Entity
@Data
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;

    private String password;

    private String name;
}

```
4.编写dao层的对应操作接口
```Java
package com.example.demo.dao;

import com.example.demo.pojo.User;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

/**
 * @className UserRepository
 * @Descirption
 * @Author WengLiangBao
 * @Date 19-5-12 下午5:21
 * @Vsersion 1.0
 */
public interface UserRepository extends JpaRepository<User,Long> {
    public List<User> findAll();
}

```
5.编写测试类测试验证
```Java
package com.example.demo;

import com.example.demo.dao.UserRepository;
import com.example.demo.pojo.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = DemoApplication.class)
public class DemoApplicationTests {

	@Autowired
	private UserRepository userRepository;

	@Test
	public void contextLoads() {
		List<User> all = userRepository.findAll();
		System.out.print(all);
	}

}
```
