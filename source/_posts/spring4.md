---
title: spring4 读后笔记
---
## spring

## 作者
spring 的作者叫做 Rob johnson
## 最初创建的目标
spring 创建的初期主要是为了解决企业级应用开发的复杂性
## spring 的根本使命
简化JAVA开发
## 支撑spring 使命实现的4个关键策略
基于pojo的轻量级和最小侵入性编程
通过依赖注入和面相接口实现松耦合
基于切面和惯例进行声明式编程
通过切面和模板减少样板式代码

### pojo示例代码
```java
public class HelloWordBean{
    public String sayHello(){
        return "hello word!";
    }
}
```
### 构造器模式
将一个事物运行的流程确定，用一个类表示，如上面的braveKnight类
将事物中多变的部分抽取出来，如执行的任务quest
### 这样做的好处
代码将松耦合
这样一来我们不用发愁骑士每次执行一个新的任务就需要重新创建一个骑士，而只需要新建一个需要执行的任务类并且实现quest接口即可
### DI(dependency injection)
#### 自动装配
github 上查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome4
#### Java注解声明式装配
github 上查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome5
#### xml装配bean
    C命名空间中的标签无法装配集合。
代码：github 上查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome6
#### 导入和混合装配
代码：github 上查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome7
##### 注解混合xml或注解
@import 将别的配置引入当前配置中
@importResource 将xml配置引入当前配置中
##### xml混合注解或xml
<import> 标签可将别的xml配置导入当前配置中
<bean> 标签可以将JavaConfig配置以bean的方式导入

#### profile条件化装配配置
使用profile 可以实现程序在不同配置下切换并运行

##### 基于注解下的使用
```java
@Configuration
@Profile("MagicExists")
public class MagicExistsConfig {

}
``` 
##### 基于xml下的使用
```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd" profile="MagicExists">
```
##### 激活并使用条件配置下的配置
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = MagicExistsConfig.class)
public class Dome8Test {
    
    @ActiveProfiles("MagicExists")
    public static class MagicExistsTest{

        @Autowired
        private MagicBean magicBean;

        @Test
        public void test1(){
            Assert.assertNotNull(magicBean);
        }

    }
}
```
关于条件化配置装配dome可到如下网址查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome8

#### 条件化bean

##### 基于注解实现（@conditional）
1.在bean声明上加入@condition注解
```java
    @Bean
    @Conditional(MagicExistsCondition.class)
    public MagicBean magicBean(){
        return new MagicBean();
    }
```
2.编写MagicExistsCondition条件判定类
```java
/**
 * @className MagicExistsCondition
 * @Descirption 判断Magic存在
 * @Author WengLiangBao
 * @Date 2020/1/8 上午10:22
 * @Vsersion 1.0
 */
public class MagicExistsCondition implements Condition {

    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        Environment env = conditionContext.getEnvironment();
        return env.containsProperty("magic");
    }
}

```
3.测试
@conditional注解会根据条件判定类中的matches方法返回的结果来装配当前被声明的bean
代码 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome8
#### 处理自动装配歧义

##### Primary
指定每次都首选的bean（个人不太推荐，因为它不够灵活）
```java
@Component
@Primary //指定首选bean解决方案
public class Cake implements Dessert {
}

```
##### Qualifier
使用限定符指定要装配的bean
###### 根据bean的ID装配
```java
    @Autowired
    @Qualifier("cake")//限定符解决方案
    private Dessert dessert;
```
###### 根据自定义限定符注解装配

1.创建自定义限定符注解
```java
/**
 * @className Cold
 * @Descirption
 * @Author WengLiangBao
 * @Date 2020/1/14 下午1:07
 * @Vsersion 1.0
 */
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Cold {
}
```
2.在bean的声明处使用注解
```java
@Component
@Cold
public class IceCream implements Dessert {
}
```
3.自动注入处使用注解
```Java
    @Autowired
    @Cold //自定义限定符注解
    private Dessert dessert;
```
代码：github 上查看 https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome9

### spring 是通过什么装载这些配置后的类的
spring通过应用上下文来实现应用装载配置
spring提供了多种上下文的实现，也就是说spring可以通过多种不同的方式来实现应用类的装载和配置

### 关于dependency injection 的更过信息可以查看《dependency injection》

总结 关于依赖注入和面相接口编程，本事阐述的核心思想是基于 装饰器模式+spring的多样化配置方式+spring 装载配置的核心接口（application context）
### AOP面向切面
#### spring实现AOP的的方式
1.spring 本身提供的aop方案是基于代理实现，这种方案只能做到方法级别的把控无法对类的创建构造函数等操作
2.可以引入aspectJ方案替代，但是学习成本比较高。此方案可以做到细粒度的把控
#### 注解实现
主要操控的内容有 
1.5种通知（前置通知，后置通知，返回通知，异常通知，环绕通知）
其中环绕通知可以通过ProceedingJoinPoint 参数类实现前后返异四中通知。
2.带参通知
3.为接口添加新的接口
代码案例 ：https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome12
https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome13
https://github.com/slothjun/Spring4BookProject/tree/master/src/main/java/dome14
#### xml实现
基于XML配置
### sping 容器（基于IOC思想）
spring 容器负责创建，装配，配置bean。并管理bean的整个生命周期
#### spring 自带的容器实现
总体可归纳为2种类型
##### bean工厂（由org.springframework.beans.factory.BeanFactory定义）
提供基本的DI支持
##### 应用上下文（由org.springframework.context.ApplicationContext定义）
基于BeanFactory构建，提供应用框架级别的服务
AnnotationConfigApplicationContext 从一个或多个基于java配置类中加载spring应用上下文
AnnotationConfigWebApplicationContext 从一个或多个基于java的配置类中加载SpringWeb应用上下文
ClassPathXmlApplicationContext 从类路径下的一个或多个xml配置文件中加载spring应用上下文
FileSystemXmlapplicationContext 从文件系统中的一个或多个xml配置文件中加载spring应用上下文
XmlWebApplicationContext 从Web应用下的一个或多个xml配置文件中加载应用上下文
#### bean的生命周期
![](http://39.104.171.29/image/springBeanLife.jpg)
#### bean 的作用域
spring默认bean的作用域是单例的。
##### scope的4大作用域
Singleton 单例：在整个应用中只创建一次bean
Prototype 原型：每次注入或根据spring上下文获取时，都会创建新的bean实例
Session 会话：在web应用中为每次会话创建一个bean的实例
Request 请求：在web应用中为每次请求创建一个bean的实例

使用Scope注解指定作用域

###### 传统项目
```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public class PrototypeCart implements ShoppingCart {
}
```
```xml
<bean id="prototypeCart" class="dome10.PrototypeCart" scope="Prototype"/>
```
###### web项目
在为web项目指定bean的作用域时需要注意bean构建时依赖传递的问题
问题举例：
    如有一个购物车bean需的作用域需要被指定为Session级别。当spring的上下文创建时发现购物车bean是的作用域是购物车级别的所以不能被创建。
    如何解决此问题，答案是通过代理的手段解决

对于bean是以接口的方式注入 (基于java代理)
```java
@Component
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE,proxyMode = ScopedProxyMode.INTERFACES)
public class PrototypeCart implements ShoppingCart {
}
```
```xml
<bean id="prototypeCart" class="dome10.PrototypeCart" scope="Session">
    <aop:scoped-proxy proxy-target-class="false"/>
</bean>
```
对于bean是以类的方式注入（基于CGLib代理）
```java
@Component
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE,proxyMode = ScopedProxyMode.TARGET_CLASS)
public class PrototypeCart implements ShoppingCart {
}
```
```xml
<bean id="prototypeCart" class="dome10.PrototypeCart" scope="Session">
    <aop:scoped-proxy/>
</bean>
```


## springMVC

​	运行流程图

![](http://39.104.171.29/image/springMvc-generater.png)

dispatcher Servlet 前端控制器（front Controller）核心

快速尝鲜：

#### 基于spring 搭建spring mvc

1.将DispatcherServlet 注册到web容器中

 	在 Servlet 3. 0 环境 中， 容器 会在 类 路径 中 查找 实现 javax. servlet. ServletContainerInitializer 接口 的 类， 如果 能 发现 的 话， 就会 用 它来 配置 Servlet 容器。

​	Spring 提供 了 这个 接口 的 实现， 名为 SpringServletContainerInitializer。

```
@HandlesTypes({WebApplicationInitializer.class})
public class SpringServletContainerInitializer implements ServletContainerInitializer 
```

​	其中的HandlesTypes 是什么？

1.Tomcat的主机容器在添加子容器时，会通过解析.xml并通过类加载器加载@HandlesTypes注解的类

2.读取@HandlesTypes注解值值。并放入ServletContainerInitializers对应的Set集合中

3.在ApplicationContext内部启动时会通知ServletContainerInitializers的onStart方法（）。这个onStart方法的第一个参数就是@HandlesTypes注解的值值指定的类集合

4.在Spring应用中，对ServletContainerInitializers的实现就是SpringServletContainerInitializer，注解指定的类就是WebApplicationInitializer。

通过上面解读过程我们知道，如果要想使用基于servlet3.0支持注解申明servlet。使用spring框架时的关注点就在WebApplicationInitializer接口中。

如果个人有能力或者有兴趣的化可以自己通过实现WebApplicationInitializer接口来注册servlet。但是一般我们只需要使用spring进行过一些封装的AbstractAnnotationConfigDispatcherServletInitializer抽象类即可。

```java
package org.springframework.web.servlet.support;

import org.springframework.lang.Nullable;
import org.springframework.util.ObjectUtils;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.support.AnnotationConfigWebApplicationContext;

public abstract class AbstractAnnotationConfigDispatcherServletInitializer extends AbstractDispatcherServletInitializer {
    public AbstractAnnotationConfigDispatcherServletInitializer() {
    }

    @Nullable
    protected WebApplicationContext createRootApplicationContext() {
        Class<?>[] configClasses = this.getRootConfigClasses();
        if (!ObjectUtils.isEmpty(configClasses)) {
            AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
            context.register(configClasses);
            return context;
        } else {
            return null;
        }
    }

    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        Class<?>[] configClasses = this.getServletConfigClasses();
        if (!ObjectUtils.isEmpty(configClasses)) {
            context.register(configClasses);
        }

        return context;
    }

    @Nullable
    protected abstract Class<?>[] getRootConfigClasses();

    @Nullable
    protected abstract Class<?>[] getServletConfigClasses();
}
```

使用 AbstractAnnotationConfigDispatcherServletInitializer注册容器并初始化上下文

```java
package com.healthengine.medpro.common.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class ServletConfigAchieve extends AbstractAnnotationConfigDispatcherServletInitializer {
    
   //用来配置ContextLoaderListener 的上下文
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }
    
    //用来配置springMvc 中Dispatcher Servlet 上下文
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[0];
    }
    
    //配置容器请求的映射路径
    @Override
    protected String[] getServletMappings() {
        return new String[0];
    }
}
```

#### 配置Filter 

##### 	方式1：基于Disparther Servlet 配置

​	

```java
package com.healthengine.medpro.common.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

import javax.servlet.Filter;
import javax.servlet.MultipartConfigElement;
import javax.servlet.ServletContext;
import javax.servlet.ServletRegistration;

public class ServletConfigAchieve extends AbstractAnnotationConfigDispatcherServletInitializer {

    //用来配置ContextLoaderListener 的上下文
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    //用来配置springMvc 中Dispatcher Servlet 上下文
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[0];
    }

    //配置容器请求的映射路径
    @Override
    protected String[] getServletMappings() {
        return new String[0];
    }

    //设置filter
    @Override
    protected Filter[] getServletFilters() {
        return //自己定义的filter 类（数组）
    }

}

```

##### 	方式2：自行实现

#### 处理Multipart格式的内容信息

​	1.配置类

```java
package com.healthengine.medpro.common.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

import javax.servlet.Filter;
import javax.servlet.MultipartConfigElement;
import javax.servlet.ServletContext;
import javax.servlet.ServletRegistration;

public class ServletConfigAchieve extends AbstractAnnotationConfigDispatcherServletInitializer {

    //用来配置ContextLoaderListener 的上下文
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    //用来配置springMvc 中Dispatcher Servlet 上下文
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[0];
    }

    //配置容器请求的映射路径
    @Override
    protected String[] getServletMappings() {
        return new String[0];
    }

    //自定义注册
    @Override
    protected void customizeRegistration(ServletRegistration.Dynamic registration) {
        //Multipart 配置设置
        registration.setMultipartConfig(new MultipartConfigElement("temp/medpro/"));
    }

}

```

2.配置MultipartResolver Bean

MultipartResolver 是springframework.web提供用于处理Multipart的接口规范

​	1.CommonsMultipartResolver： 使用 Jakarta Commons FileUpload 解析 multipart 请求； 			  

​	2.StandardServletMultipartResolver： 依赖于 Servlet 3. 0 对 multipart 请求 的 支持（ 始于 Spring 3. 1）。

```java
@Bean public MultipartResolver multipartResolver() throws IOException {
    return new StandardServletMultipartResolver();
}
```

3.测试代码

```java
    @GetMapping
    public void multipartHandle(MultipartFile multipartFile) throws IOException {
        String originalFilename = multipartFile.getOriginalFilename();
        byte[] bytes = multipartFile.getBytes();
        String contentType = multipartFile.getContentType();
        InputStream inputStream = multipartFile.getInputStream();
        String name = multipartFile.getName();
    }
```
