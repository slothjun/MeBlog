---
title: SpringBoot 参数校验
---


### 解决方案

​JSR-303 实现方案+JSR-349Bean 验证

JSR-303的实现使用hibernate的实现 @Valid注解

JSR-349Bean 验证的实现使用spring提供的@Validated注解

### 校验案例

本案例的环境为，springboot2.0.5.RELEASE

基础代码实现：

1. 导入坐标

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   ```

   此坐标包含了@Valid 和@Validated

2. 创建入口启动类

   ```java
   package com.healthengine.department;
   
   import com.healthengine.medpro.common.utils.IdWorker;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.boot.autoconfigure.domain.EntityScan;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.ComponentScan;
   import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
   
   @SpringBootApplication()
   @ComponentScan("com.healthengine.department")
   @EntityScan("com.healthengine.medpro.department")
   @EnableJpaRepositories(basePackages = "com.healthengine.department.dao")
   public class DepartmentApplication {
   
       public static void main(String[] args){
           SpringApplication.run(DepartmentApplication.class,args);
       }
   	
       //雪花算法用来生成id
       @Bean
       public IdWorker idWorker(){
           return new IdWorker(1,1);
       }
   }
   
   ```

   注意：此入口启动类要放在Controller包的同级目录下，因为@SpringBootApplication注解会扫描同级目录及其子目录。此类只放在java目录下是不对的。要放在Java目录下的下级目录下

   ![](http://39.104.171.29:80/image/springBoot-validatd1.png)（正确目录格式）

   ![](http://39.104.171.29:80/image/springBoot-validatd2.png)(错误目录格式)

   

3. 配置文件设置

   ```yml
   server:
     port: 9002
   spring:
     application:
       name: medpro-department
     datasource:
       driver-class-name: com.mysql.jdbc.Driver
       url: jdbc:mysql://localhost:3306/medprotwo
       username: xxxx
       password: xxxx
     jpa:
       database: mysql
       show-sql: true
       open-in-view: true
   ```

4. 创建controller

案例使用到的java Bean

```java
package com.healthengine.medpro.company;

import com.healthengine.medpro.department.Department;
import lombok.Data;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.validation.Valid;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotEmpty;
import java.io.Serializable;
import java.util.Date;
import java.util.List;

@Entity
@Table(name="co_company")
@Data
public class Company implements Serializable {

    private static final long serialVersionUID = 594829320797158219L;

    @Id
    private String id;

    @NotBlank(message = "用户名不能为空！",groups = {AddDepartment.class})
    private String name;

    private String managerId;

    @NotBlank(message = "版本号不能为空",groups = {AddDepartment.class})
    private String version;

    private Date renewalDate;

    private Date expirationDate;

    private String companyArea;

    private String companyAddress;

    private String businessLicenseId;

    private String legalRepresentative;

    private String companyPhone;

    private String mailbox;

    private String companySize;

    private String industry;

    private String remarks;

    private String auditState;

    private Integer state;

    private Double balance;

    private Date createTime;

    /**
     * 部门列表
     */
    @Valid //开启嵌套校验
    @NotEmpty(groups = {AddDepartment.class},message = "部门列表不能为空！")
    private List<Department> departmentList;
}

```

```java
package com.healthengine.medpro.department;

import lombok.Data;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.validation.constraints.NotBlank;
import java.util.Date;

@Entity
@Table(name = "co_department")
@Data
public class Department {

    /**
     * id
     */
    @Id
    @NotBlank(message = "id不能为空")
    private String id;

    /**
     * 父id
     */
    private String pid;

    /**
     * 企业id
     */
    private String companyId;

    /**
     * 部门名称
     */
    private String name;

    /**
     * 部门编码，同级部门不可重复
     */
    private String code;

    /**
     * 负责人id
     */
    private String managerId;

    /**
     * 负责人名称
     */
    private String manager;

    /**
     * 介绍
     */
    private String introduce;

    /**
     * 创建时间
     */
    private Date create;

}

```

```java
package com.healthengine.medpro.company;

public interface AddDepartment {
}

```



#### 	基础校验（快速尝鲜）

```java
    //对象参数校验
    @PostMapping("/test1")
    public Result test1(@Validated @RequestBody Company company){
        return new Result(ResultCode.SUCCESS);
    }
```

#### 	单个参数校验

1. 在类中加入@Validated

   ```java
   @RestController()
   @RequestMapping("/validation")
   @Validated
   public class ValidationTest {
   ```

2. 方法上对参数使用校验注解

   ```java
       @GetMapping("/test2")
       public Result test2(@NotNull(message = "年齡不能為空") Integer age){
           return new Result(ResultCode.SUCCESS);
       }
   ```

	### 	分组校验

1. 创建分组标识接口

   ```Java
   public interface AddDepartment {
   }
   ```

2. 在要校验的字段上使用校验注解并添加groups属性

   ```java
    @NotBlank(message = "用户名不能为空！",groups = {AddDepartment.class})
    private String name;
   ```

3. 在方法上使用校验

   ```java
   @PostMapping("/test3")
   public Result test3(@Validated(AddDepartment.class) @RequestBody Company company){
       return new Result(ResultCode.SUCCESS);
   }
   ```

	### 	嵌套属性校验

1. 在要需要嵌套校验的对象上添加@valid

   ```java
   /**
    * 部门列表
    */
   @Valid //开启嵌套校验
   @NotEmpty(groups = {AddDepartment.class},message = "部门列表不能为空！")
   private List<Department> departmentList;
   ```

2. 在方法上使用

   ```java
   @PostMapping("/test4")
   public Result test4(@Valid @Validated(AddDepartment.class) @RequestBody Company company){
       return new Result(ResultCode.SUCCESS);
   }
   ```

	### 	非嵌套属性校验

```java
@PostMapping("/test4")
public Result test4(@Validated(AddDepartment.class) @RequestBody Company company){
    return new Result(ResultCode.SUCCESS);
}
```

#### 	自定义校验规则

### 校验异常类全局处理

```java
package com.healthengine.department.exception;

import com.healthengine.medpro.common.entity.Result;
import org.springframework.http.converter.HttpMessageConversionException;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.ObjectError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import javax.validation.ConstraintViolation;
import javax.validation.ConstraintViolationException;
import java.util.List;

@RestControllerAdvice
public class GlobalExceptionHandler {
	
    /**
     * 单个参数异常抛出
     */
    @ExceptionHandler(ConstraintViolationException.class)
    public Result constraintViolationException(ConstraintViolationException ce){
        StringBuilder sb = new StringBuilder();
        for(ConstraintViolation violation:ce.getConstraintViolations()){
            sb.append("异常信息： "+violation.getMessage());
        }
        return new Result(500,sb.toString(),false);
    }

    /**
     * 实体类异常抛出
     * @param exception
     * @return
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Result validationBodyException(MethodArgumentNotValidException exception){

        StringBuilder sb = new StringBuilder();
        BindingResult result = exception.getBindingResult();
        if (result.hasErrors()) {

            List<ObjectError> errors = result.getAllErrors();

            errors.forEach(p ->{

                FieldError fieldError = (FieldError) p;
                sb.append("异常字段："+fieldError.getField()+" 异常信息："+fieldError.getDefaultMessage()).append(System.lineSeparator());
            });

        }
        return new Result(500,sb.toString(),false);
    }


    /**
     * 参数类型转换错误
     *
     * @param exception 错误
     * @return 错误信息
     */
    @ExceptionHandler(HttpMessageConversionException.class)
    public Result parameterTypeException(HttpMessageConversionException exception){
        return new Result(500,exception.getCause().getLocalizedMessage(),false);

    }
}

```



### 案例后发现的缺陷

1. 使用hibernate参数校验注解后发现，使用Spring jpa 存入数据时也会产生校验。

   目前解决方案，使用注解时为其分组。这样可以解决存入数据库时的校验，当存入数据库前，想做校验需要自己手动编写逻辑。