---
title: JNDI-转载
---
JNDI是 Java 命名与目录接口（Java Naming and Directory Interface），在J2EE规范中是重要的规范之一，不少专家认为，没有透彻理解JNDI的意义和作用，就没有真正掌握J2EE特别是EJB的知识。
那么，JNDI到底起什么作用？

要了解JNDI的作用，我们可以从“如果不用JNDI我们怎样做？用了JNDI后我们又将怎样做？”这个问题来探讨。

没有JNDI的做法：
程序员开发时，知道要开发访问MySQL数据库的应用，于是将一个对 MySQL JDBC 驱动程序类的引用进行了编码，并通过使用适当的 JDBC URL 连接到数据库。
就像以下代码这样：

Java code
Connection conn=null; 
try { 
	Class.forName("com.mysql.jdbc.Driver", true, Thread.currentThread().getContextClassLoader()); 
	conn=DriverManager.getConnection("jdbc:mysql://MyDBServer?user=xxx&password=xxx"); 
	...... 
	conn.close(); 
} catch(Exception e) { 
	e.printStackTrace(); 
} finally { 
	if(conn!=null) { 
		try { 
			conn.close(); 
		} catch(SQLException e) {} 
	}
}


这是传统的做法，也是以前非Java程序员（如Delphi、VB等）常见的做法。这种做法一般在小规模的开发过程中不会产生问题，只要程序员熟悉Java语言、了解JDBC技术和MySQL，可以很快开发出相应的应用程序。

没有JNDI的做法存在的问题：
1、数据库服务器名称MyDBServer 、用户名和口令都可能需要改变，由此引发JDBC URL需要修改；
2、数据库可能改用别的产品，如改用DB2或者Oracle，引发JDBC驱动程序包和类名需要修改；
3、随着实际使用终端的增加，原配置的连接池参数可能需要调整；
4、......

解决办法：
程序员应该不需要关心“具体的数据库后台是什么？JDBC驱动程序是什么？JDBC URL格式是什么？访问数据库的用户名和口令是什么？”等等这些问题，程序员编写的程序应该没有对 JDBC 驱动程序的引用，没有服务器名称，没有用户名称或口令 —— 甚至没有数据库池或连接管理。而是把这些问题交给J2EE容器来配置和管理，程序员只需要对这些配置和管理进行引用即可。

由此，就有了JNDI。

用了JNDI之后的做法：
首先，在在J2EE容器中配置JNDI参数，定义一个数据源，也就是JDBC引用参数，给这个数据源设置一个名称；然后，在程序中，通过数据源名称引用数据源从而访问后台数据库。
具体操作如下（以JBoss为例）：
1、配置数据源
在JBoss 的 D:/jboss420GA/docs/examples/jca 文件夹下面，有很多不同数据库引用的数据源定义模板。将其中的 mysql-ds.xml 文件Copy到你使用的服务器下，如 D:/jboss420GA/server/default/deploy。
修改 mysql-ds.xml 文件的内容，使之能通过JDBC正确访问你的MySQL数据库，如下：

<?xml version="1.0" encoding="UTF-8"?>
<datasources>
<local-tx-datasource>
  <jndi-name>MySqlDS</jndi-name>
  <connection-url>jdbc:mysql://localhost:3306/lw</connection-url>
  <driver-class>com.mysql.jdbc.Driver</driver-class>
  <user-name>root</user-name>
  <password>rootpassword</password>
<exception-sorter-class-name>org.jboss.resource.adapter.jdbc.vendor.MySQLExceptionSorter</exception-sorter-class-name>
  <metadata>
  <type-mapping>mySQL</type-mapping>
  </metadata>
</local-tx-datasource>
</datasources>

这里，定义了一个名为MySqlDS的数据源，其参数包括JDBC的URL，驱动类名，用户名及密码等。

2、在程序中引用数据源：
Java code
Connection conn=null; 
try { 
	Context ctx = new InitialContext(); 
	Object datasourceRef = ctx.lookup("java:MySqlDS"); 
	//引用数据源 
	DataSource ds = (Datasource) datasourceRef; 
	conn = ds.getConnection(); 
	...... 
	c.close(); 
} catch(Exception e) { 
	e.printStackTrace(); 
} finally { 
	if(conn!=null) { 
		try { 
			conn.close(); 
		} catch(SQLException e) {} 
	} 
}

直接使用JDBC或者通过JNDI引用数据源的编程代码量相差无几，但是现在的程序可以不用关心具体JDBC参数了。
在系统部署后，如果数据库的相关参数变更，只需要重新配置 mysql-ds.xml 修改其中的JDBC参数，只要保证数据源的名称不变，那么程序源代码就无需修改。

由此可见，JNDI避免了程序与数据库之间的紧耦合，使应用更加易于配置、易于部署。


总结：
J2EE 规范要求所有 J2EE 容器都要提供 JNDI 规范的实现。JNDI 在 J2EE 中的角色就是“交换机” —— J2EE 组件在运行时间接地查找其他组件、资源或服务的通用机制。在多数情况下，提供 JNDI 供应者的容器可以充当有限的数据存储，这样管理员就可以设置应用程序的执行属性，并让其他应用程序引用这些属性（Java 管理扩展（Java Management Extensions，JMX）也可以用作这个目的）。JNDI 在 J2EE 应用程序中的主要角色就是提供间接层，这样组件就可以发现所需要的资源，而不用了解这些间接性。

在 J2EE 中，JNDI 是把 J2EE 应用程序合在一起的粘合剂，JNDI 提供的间接寻址允许跨企业交付可伸缩的、功能强大且很灵活的应用程序。这是 J2EE 的承诺，而且经过一些计划和预先考虑，这个承诺是完全可以实现的。

 

说白了就是把资源取个名字，再根据名字来找资源。

//===============================================================================================================
首先我们来回顾一下简单的问题，列在下面第一点。
1.我们知道，Java 的运行从 static main 开始，为什么一定要从 static 方法开始呢？
2.在我们知道这个世界上的另外一个地方有一个对象存在而且服务器也会在我们开始工作前为我们准备好，那么我该怎么找到它呢？如果这个对象是我这个类创建的，那么当然简单，直接用对象的引用就能调用它的方法，那如果这个对象不是我创建的，我想主动调用它的方法这似乎在任何编程语言中都不可能，记得写一个方法那是被别人调用的不是主动调用别人。

就像你找人一样，如果他还没有和你建立联系的话(建立联系就是保存一份对象的引用，如果两个对象彼此没有创建另外一个而且也没有被中间的第三方建立这种关系问题就出现了)，请问你如何和他打交道？
现实中是：
a. 我们拨通 114   
  Java 中：Context ctx = new InitialContext();
b. 请问哪里有通马桶的？114 答，xxx... 为您转接中，请稍候。   
  Java 中：DataSource ds = (DataSource) ctx.lookup("便民服务公司");
c. 过了一会儿，人来了，你说：师付，请帮我通马桶吧。
  Java 中: ds.getConnection();

上面的话，我没有回答你什么是 JNDI, 但是我回答了为什么我们需要 JNDI. 希望你在概念上了解了它存在的必要性。

下面的话，给你一点指导如何更好的理解 JNDI 实现：
1.一个对象如果它在另外一个地方(可能与当前运行的程序不在同一个 VM / 同一进程中), 对象怎么可能从一个 VM 中发送到另外一个 VM 中呢？像 LDAP 这种，对象的状态还需要持久地保存的话(重启服务器进程后它还在)，又该怎么办呢？请看 JNDI StateFactory, 它用一种方法把一个对象转换成某种方式保存下来，就像我们把一个 Entity 对象保存下来时，我们会用 SQL 来做一样。

2.有一个对象上次已经保存了状态，现在服务器重启了，上次的对象肯定不在内存里面，我们怎么恢复上次的状态呢？
请看 JNDI ObjectFactory. 它读取一些上次保存的状态信息，来创建并初始化一个对象。比如：我们配置了一个 XML，它是某个 JDBC 数据源的配置数据，Application Server 启动时读取这个信息(相当于上次的状态),然后重启对象。

3.企业应用这么复杂，面向接口编程，那如何用一种简单的方式来配置新的实现类呢？Java 的做法是：
已经定义了 SPI (Service Provider Interface). 包括以下几点：
  接口准备好了，如：StateFactory / ObjectFactory.
  配置：先搜索 JRE 下面的某个 jndiprovider.properties 文件当作默认实现，再查找用户 classpath 根路径下 /jndi.properties. 另外还有 System.getProperties() 和在创建 InitialContext 给一个 hashtable 作为参数，这三个参数, 有优先级的关系，越是后面具体的参数优先级越高，越前面越通用型的参数优先级越低。这一点，请看 JDK ResourceManager 这个类的源码。
  实现类与初始化它们是如何自动完成的呢？这个你需要看 Context 接口里面的常量，以及拿 Sun LDAP InitialContextFactory 运行样例来看 Context 接口的常量一个样本参数值，一般我们很重要的是 InitialContextFactory 这个参数，但也有时候也有其他参数要配置，比如：pkgs, 它是说，我们给一个包名，JNDI 管理器要查找实现时用这个包名列表当成包名，类名就是 协议名 + 固定的后缀：比如： ldap://localhost:389, 它会用一个'包名前缀.协议名.协议名 + URLContextFactory' 作为类名来搜索一个类，如果它存在就把它当成实现类，如果没找到再尝试另外一个包名前缀。你可以看 com.sun.jndi.url 名，下面有例子看，比如说 ldap:// 的情况就是 找一个类 com.sun.jndi.url.ldap.ldapURLContextFactory，如果是 dns://www.163.com/xxx 就找个 com.sun.jndi.url.dns.dnsURLContextFactory。这是 URL context factory 也就是当你使用 ctx.lookup("java:xxxx/yyy") 这种带协议前缀的时候。

另外你也可以类比地看 com.sun.www.protocol 包里面的类，它是另外不一个与 JNDI 不相关的 URLStreamHandler 处理的规则，与些设计和配置几乎完全相同。我以前写过一个 jdbc:oracle:username/password:@localhost:1521:training/[select A from C where DEL_IND = 0] , 在 java 程序中输入这个 URL 我们可以把数据库里面的数据读取出来，效果就根你输入 file:/C:/boot.ini 读取了这个文件内容一样，办法就是我写了一个支持 jdbc 协议的 URLStreamHandler 在命令行配置一个使用它，其他的应用程序类就能自动处理，它们都不知道我是从数据库里面读取的数据。


4. J2EE 1.3 开始，资源的管理由应用服务器单独来管理和配置，这与 J2EE 1.2 不同，在 J2EE 1.2 中我们直接在应用程序中配置我们要用的资源。J2EE 1.3 中我们配置一个数据源在服务器上，我们在应用程序中只需要说明我们配置的资源的引用就行了，比如我们只在 web.xml 或 ejb-jar.xml 配置 <resource-ref /> 而不是 data source 本身。这有什么好处？比如：我们定义了两个 training 的数据源：jdbc/training/db2. jdbc/oracle/db2. 一个是开发环境，一个是 UAT 环境，现在开发时我们建立一个 <resource-ref /> 指向jdbc/training/db2，那么就用 db2 数据库，UAT 测试时我们建立另外一个 <resource-ref /> 指向 jdbc/training/oracle, 就会使用 oracle 数据库，而这本身不需要修改代码，只是修改了 web.xml / ejb-jar.xml ,而且现在连接到数据的用户名和密码不再是应用程序开发本身的事情，因为你不需要配置资源也就不需要知道它的登录名和密码，而是由管理员在服务器上配置数据源，这里注意，开发人员做他代码部分的事情，服务器管理员负责配置资源源，J2EE component provider 和 Deployer 两个角色的职责分开了，虽然现实中 deployer 都是委托给了开发人员，但 J2EE 规范是分开来描述的。

5.上面说了半天，目的是什么呢？这是我的痛苦经历，第一次写 EJB, 买了本书，J2EE 从入门到精通(就是那本传说中的黃皮宝典系列)，写了一个无状态 session bean 来访问数据源，死活找不到数据源：NamingException: xxx not found. 在 IBM developerworks 上看到一篇文章，茅塞顿开，原来那本书讲的是J2EE 1.2, 我用的 WSAD 5.1.2 开发用的默认配置都是 J2EE 1.3。这里面引出了 JNDI LinkRef, 为了实现上面 4 里面所说的服务器上配置一个资源，但应用程序里面配置一个引用的话，现在的应用服务器在处理这点JNDI技术实现上基本上都是用 LinkRef 来实现的，这是 JNDI 里面的一个类。服务器启动时会创建一个 jdbc/training/db2 和 jdbc/training/oracle 两个 DataSource 对象 (用的是 ObjectFactory), 当一个应用程序访问了准备访问数据源时，服务器检测到了 web.xml/ejb-jar.xml 中指定了 <resource-ref /> 它就会创建一个 LinkRef 放到 context 中去，它的名称是：jdbc/training,但它的 ref 是 jdbc/training/db2.
这样我们 ctx.lookup("java:jdbc/training") 时，java 协议对应的 javaURLContextFactory 会把这个 jdbc/training 对象找出来，在检测到它是一个 LinkRef 对象时，会自动再用它的 ref 值(这里是 jdbc/training/db2) 再 lookup 一遍，这下终于找到 jdbc/training/db2 这个 data source 对象。

6.JNDI 里面还有其他的相关的东西。再结合一个 Reference 概念看，LinkRef 是继承它的。想再具体的了解一个实现细节，请拿一份 apache commons-xxx.jar (名字我忘记了，不过用过 spring / hibernate 来创建数据源的人可能知道它们用 xxxDataSource 做一个不需要在服务器上配置，但却能使用 data source 的办法)，我不是推荐你这个 jar, 我是推荐你看这个xxxDataSource 源码，里面演示了一个 ObjectFactory 用法。这和 JMS ConnectionFactory等其他 J2EE 托管资源的配置和使用都是用的同样的技术实现的。举一反三。
想了解更，就再看一个 StateFactory 的实现以及 Reference 的源码之类的。作为期望迈入 J2EE 中级编程的你，至少在概念和理论上要知道 ObjectFactory / LinkRef / SPI / resource ref 配置这几点，如果你再知道 StateFactory 是怎么实现的就更好了。

转载自https://blog.csdn.net/wn084/article/details/80729230