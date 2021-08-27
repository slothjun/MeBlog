---
title: maven
---

## maven介绍

### 什么是maven

	Maven 的正确发音是[ˈmevən],而不是“马瘟”以及其他什么瘟。Maven 在美国是一个口语化的词
	语,代表专家、内行的意思。

	maven 是一个项目管理工具,它包含了一个项目对象模型（pom:project object model）一组标准集合,一个项目生命周期（project Lifecycle）,一个依赖管理系统（dependency management system）和用来运行定义在生命周期阶段中插件目标的逻辑

### maven 能做什么

	1.引入jar包，可控制jar包冲突问题
	2.编译（java文件编译为字节码）
	3.运行测试用例
	4.打包

### maven 的特点

#### 依赖管理
	根据pom中的坐标将jar包导入项目中，可以做到jar包的复用。
	如将一个junit导入项目使用maven则体现为
	```xml
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.11</version>
		<scope>test</scope>
	</dependency>
	```

##### 依赖传递
下面从字面依赖和传递两个方面了解依赖传递的概念

依赖：项目A中引入了项目B,这时我们就可以说A依赖于B

传递：项目B中依赖了项目C由于A依赖于B,所以根据依赖的关系A也就依赖了项目C

maven的依赖传递可以方便引入一些项目，但是存在一个问题就是如果一个项目同时依赖2个项目。而依赖的2个项目分别又依赖同一个项目的不同版本。这里由于版本的不同可能会引发一些兼容性问题。我们把这种情况称之为依赖传递冲突。

maven对这种冲突提供的解决方案如下：

依赖调节原则：

1、第一声明者优先原则：在 pom 文件定义依赖,先声明的依赖为准。
2、路径近者优先原则:例如:A项目依赖B B依赖C C又依赖D 如果我们在A中申明依赖D的不同版本，那么A项目就会依赖D的声明版本而不是传递的D版本。

只要我们遵循原则maven会帮我们自动处理依赖 ，但我们如果不想遵循原则就不能解决冲突了吗？答案肯定是可以，毕竟maven是一款优秀的自动化构建工具

不遵循原则如何解决冲突：

1.排除依赖
```xml
<dependency>
    <groupId>com.itcast.ssm</groupId>
    <artifactId>ssm_service</artifactId>
    <version>1.0-SNAPSHOT</version>
    <exclusions>
        <exclusion>
            <groupId>XXX</groupId>
            <artifactId>XXX</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
2.锁定版本
```xml
 <dependencyManagement>
        <dependencies>
            <!-- Mybatis -->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
            </dependency>
        </dependencies>
</dependencyManagement>
```
3.可选依赖
当坐标被赋予 
```xml
<optional>true</optional>
```
时此坐标不会被传递依赖。

example：
```xml
 <dependencyManagement>
        <dependencies>
            <!-- Mybatis -->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
                <optional>true</optional>
            </dependency>
        </dependencies>
</dependencyManagement>
```

#### 一键构建
	maven 规范化流程如下图
![](http://39.104.171.29/image/maven_stream.png)
	一键构建是指，如果用户需要实现上述流程中的清理，只需要使用maven自带的mvn clean命令即可完成项目的清空。
	如果是实现流程中的打包，则使用命令mvn package 即可，并且它还会执行打包前的所有流程项。

### maven 安装后目录解析
bin:存放了 maven 的命令,比如我们前面用到的 mvn tomcat:run
boot:存放了一些 maven 本身的引导程序,如类加载器等
conf:存放了 maven 的一些配置文件,如 setting.xml 文件
lib:存放了 maven 本身运行所需的一些 jar 包
在使用maven前电脑需安装jdk或jre因为maven是用java编写的

### maven 命令

查看maven的帮助信息
```linux
spiderbao@spiderbao-CW65S:~$ mvn -help
```
maven 查看版本信息
```linux
spiderbao@spiderbao-CW65S:~$ mvn -v
Apache Maven 3.6.0
Maven home: xxx
Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: xxx
java-8-openjdk-amd64/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "linux", version: "4.18.0-18-generic", arch: "amd64", family: "unix"
```
compile 编译
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn complie
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.145 s
[INFO] Finished at: 2019-05-08T14:24:23+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Unknown lifecycle phase "complie". You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or <plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-clean, clean, post-clean, pre-site, site, post-site, site-deploy. -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/LifecyclePhaseNotFoundException
```
mvn complie 打错 改为 compile
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn compile
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ hello_maven ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /home/spiderbao/idea_dev/hello_maven/target/classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.307 s
[INFO] Finished at: 2019-05-08T14:26:40+08:00
[INFO] ------------------------------------------------------------------------
```
test 测试
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn test
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ hello_maven ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ hello_maven ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ hello_maven ---
[INFO] No tests to run.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.072 s
[INFO] Finished at: 2019-05-08T14:27:10+08:00
[INFO] ------------------------------------------------------------------------
```
由于没有写测试用例所以No tests to run.
clean 清空
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn clean
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ hello_maven ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.289 s
[INFO] Finished at: 2019-05-08T14:23:21+08:00
[INFO] ------------------------------------------------------------------------

```
package 打包
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ hello_maven ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ hello_maven ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ hello_maven ---
[INFO] No tests to run.
[INFO] 
[INFO] --- maven-war-plugin:2.2:war (default-war) @ hello_maven ---
[INFO] Packaging webapp
[INFO] Assembling webapp [hello_maven] in [/home/spiderbao/idea_dev/hello_maven/target/hello_maven]
[INFO] Processing war project
[INFO] Copying webapp resources [/home/spiderbao/idea_dev/hello_maven/src/main/webapp]
[INFO] Webapp assembled in [22 msecs]
[INFO] Building war: /home/spiderbao/idea_dev/hello_maven/target/hello_maven.war
[INFO] WEB-INF/web.xml already added, skipping
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.515 s
[INFO] Finished at: 2019-05-08T14:28:25+08:00
```
install 安装
```linux
spiderbao@spiderbao-CW65S:~/idea_dev/hello_maven$ mvn install
[INFO] Scanning for projects...
[INFO] 
[INFO] ----------------------< com.itheima:hello_maven >-----------------------
[INFO] Building hello_maven Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ hello_maven ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ hello_maven ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/spiderbao/idea_dev/hello_maven/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ hello_maven ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ hello_maven ---
[INFO] No tests to run.
[INFO] 
[INFO] --- maven-war-plugin:2.2:war (default-war) @ hello_maven ---
[INFO] Packaging webapp
[INFO] Assembling webapp [hello_maven] in [/home/spiderbao/idea_dev/hello_maven/target/hello_maven]
[INFO] Processing war project
[INFO] Copying webapp resources [/home/spiderbao/idea_dev/hello_maven/src/main/webapp]
[INFO] Webapp assembled in [23 msecs]
[INFO] Building war: /home/spiderbao/idea_dev/hello_maven/target/hello_maven.war
[INFO] WEB-INF/web.xml already added, skipping
[INFO] 
[INFO] --- maven-install-plugin:2.4:install (default-install) @ hello_maven ---
[INFO] Installing /home/spiderbao/idea_dev/hello_maven/target/hello_maven.war to /home/spiderbao/.m2/repository/com/itheima/hello_maven/1.0-SNAPSHOT/hello_maven-1.0-SNAPSHOT.war
[INFO] Installing /home/spiderbao/idea_dev/hello_maven/pom.xml to /home/spiderbao/.m2/repository/com/itheima/hello_maven/1.0-SNAPSHOT/hello_maven-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.604 s
[INFO] Finished at: 2019-05-08T14:42:26+08:00
[INFO] ------------------------------------------------------------------------
```

### maven 库

maven库可以分为3类分别是：

#### 本地仓库
	本地仓库 :用来存储从远程仓库或中央仓库下载的插件和 jar 包,项目使用一些插件或 jar 包,
优先从本地仓库查找
#### 远程仓库
	远程仓库:如果本地需要插件或者 jar 包,本地仓库没有,默认去远程仓库下载。
远程仓库可以在互联网内也可以在局域网内。
#### 中央仓库
	中央仓库 :在 maven 软件中内置一个远程仓库地址 http://repo1.maven.org/maven2 ,它是中
央仓库,服务于整个互联网,它是由 Maven 团队自己维护,里面存储了非常全的 jar 包,它包
含了世界上大部分流行的开源项目构件。

### setting

setting 文件主要用来配置maven的一些仓库设置，如本地仓库，远程仓库等。

#### 全局setting 和用户 setting

在 maven 安装目录下的有 conf/setting.xml 文件,此 setting.xml 文件用于 maven 的所有 project
项目,它作为 maven 的全局配置。
如需要个性配置则需要在用户配置中设置,用户配置的 setting.xml 文件默认的位置在: ${user.dir}
/.m2/settings.xml 目录中,${user.dir} 指 windows 中的用户目录。或在项目中添加setting.xml
maven 会先找用户配置,如果找到则以用户配置文件为准,否则使用全局配置文件。

![](http://39.104.171.29/image/maven_001.png)

### maven 工程认识

maven 工程的目录结构如下图

![](http://39.104.171.29/image/maven_002.png)

作为一个 maven 工程,它的 src 目录和 pom.xml 是必备的。
进入 src 目录后,我们发现它里面的目录结构如下:

![](http://39.104.171.29/image/maven_004.png)

src/main/java —— 存放项目的.java 文件
src/main/resources —— 存放项目资源文件,如 spring, hibernate 配置文件
src/test/java —— 存放所有单元测试.java 文件,如 JUnit 测试类
src/test/resources —— 测试资源文件
target —— 项目输出位置,编译后的 class 文件会输出到此目录
pom.xml——maven 项目核心配置文件
注意:如果是普通的 java 项目,那么就没有 webapp 目录。

### maven 指令的生命周期

maven 对项目构建过程分为三套相互独立的生命周期,请注意这里说的是“三套”,而且“相互独立”,
这三套生命周期分别是:

Clean Lifecycle 在进行真正的构建之前进行一些清理工作。
Default Lifecycle 构建的核心部分,编译,测试,打包,部署等等。
Site Lifecycle 生成项目报告,站点,发布站点

### maven 概念模型

Maven 包含了一个项目对象模型 (Project Object Model),一组标准集合,一个项目生命周期(Project
Lifecycle),一个依赖管理系统(Dependency Management System),和用来运行定义在生命周期阶段
(phase)中插件(plugin)目标(goal)的逻辑。

![](http://39.104.171.29/image/maven_005.png)

  项目对象模型 (Project Object Model)
一个 maven 工程都有一个 pom.xml 文件,通过 pom.xml 文件定义项目的坐标、项目依赖、项目信息、
插件目标等。

  依赖管理系统(Dependency Management System)
通过 maven 的依赖管理对项目所依赖的 jar 包进行统一管理。
比如:项目依赖 junit4.9,通过在 pom.xml 中定义 junit4.9 的依赖即使用 junit4.9,如下所示是 junit4.9
的依赖定义:

```xml
<!-- 依赖关系 -->
<dependencies>
	<!-- 此项目运行使用 junit,所以此项目依赖 junit -->
	<dependency>
	<!-- junit 的项目名称 -->
	<groupId>junit</groupId>
	<!-- junit 的模块名称 -->
	<artifactId>junit</artifactId>
	<!-- junit 版本 -->
	<version>4.9</version>
	<!-- 依赖范围:单元测试时使用 junit -->
	<scope>test</scope>
</dependency>
```

   一个项目生命周期(Project Lifecycle)
使用 maven 完成项目的构建,项目构建包括:清理、编译、测试、部署等过程,maven 将这些
过程规范为一个生命周期,如下所示是生命周期的各各阶段:
![](http://39.104.171.29/image/maven_006.png)
maven 通过执行一些简单命令即可实现上边生命周期的各各过程,比如执行 mvn compile 执行编译、
执行 mvn clean 执行清理。

   一组标准集合
maven 将整个项目管理过程定义一组标准,比如:通过 maven 构建工程有标准的目录结构,有
标准的生命周期阶段、依赖管理有标准的坐标定义等。

  插件(plugin)目标(goal)
maven 管理项目生命周期过程都是基于插件完成的

### maven的继承和聚合

maven的继承

继承后maven的子模块会拥有父模块的所有pom信息
```xml
<parent>
    <artifactId>ssm_parent</artifactId>
    <groupId>com.itcast.ssm</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
```
maven的聚合

我把maven的聚合看做，一个pom件就可以构建指定模块并组合成为一个项目

## maven 坐标的来源方式

### 坐标搜索网站

[central](http://search.maven.org/)	or [mvnrepository](http://mvnrepository.com/)

## maven 私服搭建及其使用

### maven私服搭建

下载nexus安装包
```linux
　　$　　cd /opt

　　$　　wget http://download.sonatype.com/nexus/oss/nexus-2.10.0-02-bundle.tar.gz
```
解压并安装
```linux
　　$　　cd /opt/

　　$　　tar -zxvf nexus-2.10.0-02-bundle.tar.gz

　　$　　cd /opt/nexus-2.10.0-02/bin

　　$　　vim nexus
```
在 nexus文件中添加RUN_AS_USER用户如下
```xml
RUN_AS_USER=root
```
启动nexus
```linux
　$　　./nexus start　
```
nexus 配置解析：

查看 nexus 的配置文件 conf/nexus.properties

```properties
# Jetty section
application-port=8081 # nexus 的访问端口配置
application-host=0.0.0.0 # nexus 主机监听配置(不用修改)
nexus-webapp=${bundleBasedir}/nexus
nexus-webapp-context-path=/nexus
# nexus 工程目录
# nexus 的 web 访问路径
# Nexus section
nexus-work=${bundleBasedir}/../sonatype-work/nexus
# nexus 仓库目录
runtime=${bundleBasedir}/nexus/WEB-INF # nexus 运行程序目录
```
### nexus仓库类型

nexus 的仓库有 4 种类型:

1.hosted,宿主仓库,部署自己的 jar 到这个类型的仓库,包括 releases 和 snapshot 两部分,Releases 公司内部发布版本仓库、 Snapshots 公司内部测试版本仓库

2.proxy,代理仓库,用于代理远程的公共仓库,如 maven 中央仓库,用户连接私服,私服自动去中央仓库下载 jar 包或者插件。

3.group,仓库组,用来合并多个 hosted/proxy 仓库,通常我们配置自己的 maven 连接仓库组。

4.virtual(虚拟):兼容 Maven1 版本的 jar 或者插件

nexus 仓库默认在 sonatype-work 目录中

### 将项目发布到私服

1.setting配置添加 server

```xml
<server>
	<id>releases</id>
	<username>admin</username>
	<password>admin123</password>
</server>
<server>
	<id>snapshots</id>
	<username>admin</username>
	<password>admin123</password>
</server>
```

2.项目pom添加仓库
```xml
<distributionManagement>
	<repository>
		<id>releases</id>
		<url>http://localhost:8081/nexus/content/repositories/releases/</url>
	</repository>
	<snapshotRepository>
		<id>snapshots</id>
		<url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
	</snapshotRepository>
</distributionManagement>
```

3.使用maven命令发布

```linux
mvn deploy
```

### maven 配置私服下载jar包

没有配置nexus之前,如果本地仓库没有,去中央仓库下载,通常在企业中会在局域网
内部署一台私服服务器,有了私服本地项目首先去本地仓库找 jar,如果没有找到则连接私
服从私服下载 jar 包,如果私服没有 jar 包私服同时作为代理服务器从中央仓库下载 jar 包,
这样做的好处是一方面由私服对公司项目的依赖 jar 包统一管理,一方面提高下载速度,项
目连接私服下载 jar 包的速度要比项目连接中央仓库的速度快的多。

setting配置

```xml
 <profile>
         <!--仓库id-->
         <id>dev</id>
         <repositories>
                 <repository>
                         <!--仓库 id,repositories 可以配置多个仓库,保证 id 不重复-->
                         <id>nexus</id>
                         <!--仓库地址,即 nexus 仓库组的地址-->
                         <url>http://localhost:8081/nexus/content/groups/public/</url>
                         <releases>
                                 <enabled>true</enabled>
                         </releases>
                         <snapshots>
                                 <enabled>true</enabled>
                         </snapshots>
                 </repository>
         </repositories>
   </profile>

	<activeProfiles>
		<activeProfile>dev</activeProfile>
	</activeProfiles>
```
### 将第三方jar导入本地maven
```linux
mvn install:install-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37
-Dfile= fastjson-1.1.37.jar -Dpackaging=jar
```
参数说明
DgroupId 和 DartifactId 构成了该 jar 包在 pom.xml 的坐标,项目就是依靠这两个属性定位。
自己起名字也行。
Dfile 表示需要上传的 jar 包的绝对路径。
Durl 私服上仓库的位置,打开 nexus——>repositories 菜单,可以看到该路径。
DrepositoryId 服务器的表示 id,在 nexus 的 configuration 可以看到。
Dversion 表示版本信息
### 将第三方jar导入私服

1.在setting中配置第三方server
```xml
<server>
	<id>thirdparty</id>
	<username>admin</username>
	<password>admin123</password>
</server>
```
```linux
mvn deploy:deploy-file -DgroupId=com.alibaba -DartifactId=fastjson -Dversion=1.1.37
-Dpackaging=jar -Dfile=fastjson-1.1.37.jar
-Durl=http://localhost:8081/nexus/content/repositories/thirdparty/
-DrepositoryId=thirdparty
```
参数说明
DgroupId 和 DartifactId 构成了该 jar 包在 pom.xml 的坐标,项目就是依靠这两个属性定位。
自己起名字也行。
Dfile 表示需要上传的 jar 包的绝对路径。
Durl 私服上仓库的位置,打开 nexus——>repositories 菜单,可以看到该路径。
DrepositoryId 服务器的表示 id,在 nexus 的 configuration 可以看到。
Dversion 表示版本信息
