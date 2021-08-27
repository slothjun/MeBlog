---
title: modelAttribute注解
---

## @modelAttribute


	moelAttribute 是springmvc提供的注解。

	1.运用在参数上，会将客户端传递过来的参数按名称注入到指定对象中，并且会将这个对象自动加入ModelMap中，便于View层使用；
	
	2.运用在方法上，会在每一个@RequestMapping标注的方法前执行，如果有返回值，则自动将该返回值加入到ModelMap中；


### 运用在方法上

#### 实现案例，抽取request 和response、modelMap将其注入在baseController中。

beseController
```java
import lombok.Data;
import org.springframework.web.bind.annotation.ModelAttribute;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Data
public class BaseController {

    protected HttpServletRequest request;

    protected HttpServletResponse response;

    @ModelAttribute
    public void initParams(HttpServletResponse response,HttpServletRequest request){
        this.request = request;
        this.response = response;
    }
}
```

基础controller

```java
import com.healthengine.medpro.common.controller.BaseController;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/file")
@CrossOrigin
public class FileBaseController extends BaseController {


    @GetMapping("/modelAttributeTest")
    public String modelAttributeTest(){
        request.getAuthType();
        response.getStatus();
        return "OK";
    }

}
```

注意：其中的response是多例线程不安全的，如果想要时其线程安全可以使用ThreadLocal