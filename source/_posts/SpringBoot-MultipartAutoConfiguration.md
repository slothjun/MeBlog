---
title: springBoot-MultipartAutoConfiguration
---

## 前提：

​	今天本人想实现一个文件上传的功能。

​	文件上传的难点在以什么样的传输格式上传文件。

​	一般 表单 提交 所 形成 的 请求 结果是 很 简单 的， 就是 以“&” 符 分割 的 多个 name- value 对。 例如

```
firstName= Charles& lastName= Xavier& email= professorx% 40xmen. org &username= professorx& password= letmein01
```

尽管 这种 编码 形式 很 简单， 并且 对于 典型的 基于 文本 的 表单 提交 也 足够 满足要求， 但是 对于 传送 二进制 数据， 如上 传 图片等文件， 就 显得 力不从心 了。

​	那么对应二进制数据的上传依托Http请求要怎么去实现呢？

​	multipart 格式 的 数据 会 将 一个 表单 拆分 为多 个 部分（ part）， 每个 部分 对应 一个 输入 域。 在 一般 的 表单 输入 域 中， 它 所 对应 的 部分 中会 放置 文本 型 数据， 但是 如果 上传 文件 的 话， 它 所 对应 的 部分 可以 是 二进制， 下面 展现 了 multipart 的 请求 体：

```
------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" firstName" Charles 
------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" lastName" Xavier ------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" email" charles@ xmen. com 
------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" username" professorx 
------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" password" letmein01 
------WebKitFormBoundaryqgkaBn8IHJCuNmiW Content- Disposition: form- data; name=" profilePicture"; filename=" me. jpg" Content- Type: image/ jpeg [[ Binary image data goes here ]] 
------WebKitFormBoundaryqgkaBn8IHJCuNmiW--
```

在 这个 multipart 的 请求 中， 我们 可以 看到 profilePicture 部分 与其 他 部分 明显 不同。 除了 其他 内容 以外， 它 还有 自己的 Content- Type 头， 表明 它是 一个 JPEG 图片。 尽管 不一定 那么 明显， 但 profilePicture 部分 的 请求 体 是 二进制 数据， 而 不是 简单 的 文本。

尽管 multipart 请求 看起来 很复杂， 但通过 Spring MVC 处理 它们 却 很容易。 在 编写 控制器 方法 处理 文件 上传 之前， 我们 必须 要 配置 一个 multipart 解析 器， 通过 它来 告诉 DispatcherServlet 该 如何 读取 multipart 请求。

​	spring mvc 对multipart 解析器定义了 multipart Resolver 接口规范。 并提供了CommonsMultipartResolver： 使用 Jakarta Commons FileUpload 解析 multipart 请求的实现和StandardServletMultipartResolver： 依赖于 Servlet 3. 0 对 multipart 请求 的 支持（ 始于 Spring 3. 1）。

![](http://39.104.171.29:80/image/CommonsMultipartResolver.png)

![](http://39.104.171.29:80/image/StandardServletMultipartResolver.png)

使用springboot 如何操作multipart Resolver  接下来请看MultipartAutoConfiguration装配类都做了什么

## 	MultipartAutoConfiguration



