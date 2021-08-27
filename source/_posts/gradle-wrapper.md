---
title: Gradle Wrapper
---

# 关于Gradle Wrapper

推荐的方式执行任何Gradle构建的帮助下Gradle包装器(总之只是“包装器”)。包装是一个脚本,该脚本调用它宣布版本,必要时预先下载它。因此,开发人员可以用Gradle项目迅速而不必遵循手动安装过程节省贵公司的时间和金钱 

![](https://docs.gradle.org/current/userguide/img/wrapper-workflow.png)

你将获得以下好处：

同一版本

减低了版本统一的维护成本，只需在wrapper 文件下声明处修改版本就可以

```
task wrapper(type: Wrapper) {
    gradleVersion = '1.11' //修改此处版本
}
```

## 使用wrapper 的3中情况

### 创建一个新的项目并加入wrapper

1. 在自己指定的工作空间目录下运行如下命令

   ```
   gradle wrapper
   ```

2. 创建指令

   --gradle-version ：用于指定gradle 的版本

   --distribution-type ：用于指定gradle的类型 bin 或 all

   --gradle-distribution-url : 用于指导gradle的下载路径

   --gradle-distribution-sha256-sum ：用于指定编码格式

   案例：创建gradle 版本为5.1.1的all类型

   ```
   gradle wrapper --gradle-version 5.1.1 --distribution-type all
   ```

3. 创建后的目录结构图

   ```
   ├── gradle
   │   └── wrapper
   │       ├── gradle-wrapper.jar
   │       └── gradle-wrapper.properties
   ├── gradlew
   └── gradlew.bat
   ```

   gradle-wrapper.jar  wrapper的源文件

   gradle-wrapper.properties  wrapper的版本等配置属性

   gradlew  shell脚本工具

   gradlew.bat windows 批处理工具

### 运行已经加入wrapper的项目

1. 在项目目录下运行下列代码

   ```
   gradlew.bat build
   ```

   

### 在已有wrapper的gradle项目中更新gradle的版本为最新。

使用前面的命令可进行更新

```
gradle wrapper --gradle-version 5.1.1
```

