---
title: configuartionProperties注解
---

## @configuartionProperties

### 作用

​	用于将properties 文件中的属性或yml属性文件中的属性 放入pojo中

### 使用案例

#### @configuartionProperties + @Component

实体类

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "me.user")
@Data
public class User {

    private String name;

    private String password;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}

```

测试方法

```java
import com.healthengine.medpro.fileSystem.FileSystemApplication;
import com.healthengine.medpro.fileSystem.controller.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = FileSystemApplication.class)
public class ConfigurationProperties {
    @Autowired
    private User user;

    @Test
    public void configuration(){
        System.out.println(user);
    }


}
```

配置文件设置

```yaml
me:
  user:
    name: baowengliang
    password: 123456
```

测试结果

```
User{name='baowengliang', password='123456'}
```

#### @Bean + @ConfigurationProperties

实体类

```java
import lombok.Data;

@Data
public class User {

    private String name;

    private String password;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

启动类

```java
@SpringBootApplication()
public class FileSystemApplication {

    public static void main(String[] args){
        SpringApplication.run(FileSystemApplication.class,args);
    }
    
    @Bean
    @ConfigurationProperties(prefix = "me.user")
    public User user(){
        return new User();
    }
}
```

测试方法

```java
import com.healthengine.medpro.fileSystem.FileSystemApplication;
import com.healthengine.medpro.fileSystem.controller.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = FileSystemApplication.class)
public class ConfigurationProperties {
    @Autowired
    private User user;

    @Test
    public void configuration(){
        System.out.println(user);
    }


}
```

配置文件设置

```yaml
me:
  user:
    name: baowengliang
    password: 123456
```

测试结果
```
User{name='baowengliang', password='123456'}
```



#### @EnableConfigurationProperties + @ConfigurationProperties

实体类

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

@Data
@ConfigurationProperties(prefix = "me.user")
public class User {

    private String name;

    private String password;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

启动类

```java
@SpringBootApplication()
@EnableConfigurationProperties(User.class)
public class FileSystemApplication {

    public static void main(String[] args){
        SpringApplication.run(FileSystemApplication.class,args);
    }
    
}
```

配置文件

```yaml
me:
  user:
    name: baowengliang
    password: 123456
```

测试类

```java
import com.healthengine.medpro.fileSystem.FileSystemApplication;
import com.healthengine.medpro.fileSystem.controller.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = FileSystemApplication.class)
public class ConfigurationProperties {
    @Autowired
    private User user;

    @Test
    public void configuration(){
        System.out.println(user);
    }


}
```

测试结果

```
User{name='baowengliang', password='123456'}
```

