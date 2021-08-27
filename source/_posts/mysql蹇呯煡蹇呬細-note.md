---
title: mysql必知必会-note
---

## 了解sql

### 数据库基础术语

#### 什么是数据库

数据库是一个以某种有组织的方式存储的数据集合。所以本人认为数据库是一个管理数据的概念。

#### 表

表是某种特定类型数据的结构化清单。

#### 模式

模式 是关于数据库和表的布局及特性的信息

#### 列

列 column 表中的一个字段，所有表是由一个或多个列组成。

#### 数据类型

数据类型 datatype 表示所容许的数据类型。每个表的列都有相应的数据类型，它限制了列中存储的数据。数据类型对于数据的排序，和优化磁盘使用方面起到了重要的作用。

#### 行

行 row 表中的一行记录。

#### 主键

主键 primarykey 由一列或列表示。它们与普通的列不同，它们具有其值具有表中唯一性切不为null的特性。

#### 什么是sql

sql 的全称是structured query language （结构化查询语言）它是一个专门用来跟数据库通讯的语言。

sql具有的特定及优点如下

1.互通性 sql 不是某个DBMS厂商的专属语言，所以使用sql可以操作大多数DBMS产品 如MySQL

2.简单易学。sql的大多数功能由一个或几个关键字组成，所以学习成本较低。

3.在保持语法简单的情况下，做到了功能强大操作灵活。

## mysql简介

### 什么是MySQL

首先MySQL它是一种DBMS产品，DBMS是databaseManageSystem 的简称。其次MySQL具有如下特性

1.成本底，由于MySQL是开源产品所以企业商业是不需要支付费用的。

2.性能，MySQL在DBMS系列产品中的性能优越

3.可信赖，就像Linux内核一样。MySQL的开源使得其的使用者和维护者增多。

4.简单易用

### DBMS产品分类

DBMS产品可以大致分为2类，一类是基于共享文件系统如File Maker 等。另一类是基于客户机与服务器的如MySQL、Oracle等

### MySQL工具（客户端）

MySQL workbench

mysql命令行（自带）

## 使用MySQL

### 链接

链接一般用于像 MySQL workbench 这样的工具或程序代码中。

链接案例如下：

```text
root@localhost:3306
```
其中 root表示用户名

localhost 表示IP地址

3306 表示服务端口

### 选择数据库

登录数据库后使用MySQL命令行工具选择切换数据库可以使用关键字USE实现

```sql
USE 存在的数据库名称
```
### 关于SHOW的一些操作

显示所有的数据库名称

```linux
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| crashcourse        |
| fileupload         |
| maven_study        |
| mybatis_study      |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
8 rows in set (0.00 sec)
```
显示数据库中的所有表名称，注意：此时需要使用USE选择要查看的数据库

```linux
mysql> USE crashcourse;
Database changed
mysql> SHOW TABLES;
+-----------------------+
| Tables_in_crashcourse |
+-----------------------+
| customers             |
| orderitems            |
| orders                |
| productnotes          |
| products              |
| vendors               |
+-----------------------+
6 rows in set (0.00 sec)
```
显示表中列column的信息

```linux
mysql> SHOW COLUMNS FROM orders;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| order_num  | int(11)  | NO   | PRI | NULL    | auto_increment |
| order_date | datetime | NO   |     | NULL    |                |
| cust_id    | int(11)  | NO   | MUL | NULL    |                |
+------------+----------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```
此查询还有一个快捷方式就是 

```linux
mysql> DESCRIBE orders;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| order_num  | int(11)  | NO   | PRI | NULL    | auto_increment |
| order_date | datetime | NO   |     | NULL    |                |
| cust_id    | int(11)  | NO   | MUL | NULL    |                |
+------------+----------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```
DESCRIBE 是 SHOW COLUMNS FROM的一种快捷方式

显示数据库创建语句或表的创建语句

```linux
mysql> SHOW CREATE DATABASE crashcourse;
+-------------+---------------------------------------------------------------------------------------+
| Database    | Create Database                                                                       |
+-------------+---------------------------------------------------------------------------------------+
| crashcourse | CREATE DATABASE `crashcourse` /*!40100 DEFAULT CHARACTER SET utf8 COLLATE utf8_bin */ |
+-------------+---------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> SHOW CREATE TABLE order;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'order' at line 1
mysql> SHOW CREATE TABLE orders;
+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table  | Create Table                                                                                                                                                                                                                                                                                                                                                                          |
+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| orders | CREATE TABLE `orders` (
  `order_num` int(11) NOT NULL AUTO_INCREMENT,
  `order_date` datetime NOT NULL,
  `cust_id` int(11) NOT NULL,
  PRIMARY KEY (`order_num`),
  KEY `fk_orders_customers` (`cust_id`),
  CONSTRAINT `fk_orders_customers` FOREIGN KEY (`cust_id`) REFERENCES `customers` (`cust_id`)
) ENGINE=InnoDB AUTO_INCREMENT=20010 DEFAULT CHARSET=utf8 COLLATE=utf8_bin |
+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

```
关于SHOW的一些命令可以通过 HELP SHOW获取

## 检索数据

### SELECT语句

select是sql中最常用的查询语句

#### 检索单个列

```linux
mysql> SELECT prod_name FROM products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```
本次查询只是单纯的检索了单个列的数据，没有对数据做过滤和排序。

个人认为过滤不是必须的，但排序是必须的。因为不指定排序的数据顺序是不可控的。

#### 检索多个列

```linux
mysql> SELECT prod_id,prod_name,prod_price FROM products;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| ANV01   | .5 ton anvil   |       5.99 |
| ANV02   | 1 ton anvil    |       9.99 |
| ANV03   | 2 ton anvil    |      14.99 |
| DTNTR   | Detonator      |      13.00 |
| FB      | Bird seed      |      10.00 |
| FC      | Carrots        |       2.50 |
| FU1     | Fuses          |       3.42 |
| JP1000  | JetPack 1000   |      35.00 |
| JP2000  | JetPack 2000   |      55.00 |
| OL1     | Oil can        |       8.99 |
| SAFE    | Safe           |      50.00 |
| SLING   | Sling          |       4.49 |
| TNT1    | TNT (1 stick)  |       2.50 |
| TNT2    | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
14 rows in set (0.00 sec)

```
注意多个列间使用,分割最后一个不需要加入,不然会出现语法错误

如果现在需要检索表中的所有列，可以使用通配符`*`代替列名。

#### 检索数据去重

```linux
mysql> SELECT vend_id FROM products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1001 |
|    1001 |
|    1002 |
|    1002 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1005 |
|    1005 |
+---------+
14 rows in set (0.00 sec)

mysql> SELECT DISTINCT vend_id FROM products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1002 |
|    1003 |
|    1005 |
+---------+
4 rows in set (0.00 sec)

```

第一次select 查询获取了products.vend_id的所有数据
第二次select 查询内容同上但是加入了DISTINCT关键字对数据进行了去重。

DISTINCT使用注意事项

1.不能对部分列使用，如 prod_id,DISTINCT vend_id。

2.DISTINCT 位于 SELECT 后 列前 ，所有的被检索的列将都具有去重功能。

#### 检索限制输出

想要控制输出就使用 limit关键字如下

输出查询结果的前5行
```linux
mysql> SELECT prod_name FROM products LIMIT 5;
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
| Detonator    |
| Bird seed    |
+--------------+
5 rows in set (0.00 sec)
```

从第1行开始显示3行

```linux
mysql> SELECT prod_name FROM products LIMIT 1,3;
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
| 2 ton anvil |
| Detonator   |
+-------------+
3 rows in set (0.00 sec)
```
#### 使用完全限定

之前的查询语句中使用的都是先对表名，举个对比的例子如下

未使用完全限定的查询语句

SELECT prod_name FROM products LIMIT 1,3

使用完全限定的查询语句

SELECT products.prod_name FROM crashcourse.products LIMIT 1,3

使用完全限定时语句更严谨。推荐使用！

## 排序检索数据

### 单列排序数据

```linux
mysql> SELECT prod_name FROM products ORDER BY prod_name;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Bird seed      |
| Carrots        |
| Detonator      |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```
根据products.prod_name 按字母升序排序。

### 多列排序顺序

```linux
mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price,prod_name;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
| FU1     |       3.42 | Fuses          |
| SLING   |       4.49 | Sling          |
| ANV01   |       5.99 | .5 ton anvil   |
| OL1     |       8.99 | Oil can        |
| ANV02   |       9.99 | 1 ton anvil    |
| FB      |      10.00 | Bird seed      |
| TNT2    |      10.00 | TNT (5 sticks) |
| DTNTR   |      13.00 | Detonator      |
| ANV03   |      14.99 | 2 ton anvil    |
| JP1000  |      35.00 | JetPack 1000   |
| SAFE    |      50.00 | Safe           |
| JP2000  |      55.00 | JetPack 2000   |
+---------+------------+----------------+
14 rows in set (0.00 sec)
```
上述例子中的输出，仅在多行拥有相同的prod_price值时，才对产品prod_name进行排序。如果prod_price的值都是唯一的就不会使用prod_name的条件进行排序

### 排序的方向

默认排序的方向是升序，相反降序的案例如下

```linux
mysql> SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC;
+---------+------------+----------------+
| prod_id | prod_price | prod_name      |
+---------+------------+----------------+
| JP2000  |      55.00 | JetPack 2000   |
| SAFE    |      50.00 | Safe           |
| JP1000  |      35.00 | JetPack 1000   |
| ANV03   |      14.99 | 2 ton anvil    |
| DTNTR   |      13.00 | Detonator      |
| FB      |      10.00 | Bird seed      |
| TNT2    |      10.00 | TNT (5 sticks) |
| ANV02   |       9.99 | 1 ton anvil    |
| OL1     |       8.99 | Oil can        |
| ANV01   |       5.99 | .5 ton anvil   |
| SLING   |       4.49 | Sling          |
| FU1     |       3.42 | Fuses          |
| FC      |       2.50 | Carrots        |
| TNT1    |       2.50 | TNT (1 stick)  |
+---------+------------+----------------+
14 rows in set (0.00 sec)
```
另外 DESC 不同于 DISTINCT 它可用于不同的条件列，只需要在每个列后加入DESC即可。

SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC,prod_name;

表示按价格降序排序，如果有价格相同的按商品名称升序排序。

## 过滤数据

### where子语句

where子语句可以添加一些查询条件，过滤数据。

#### 根据where 操作符过滤

where操作符如下

=	等于

<>	不等于

!=	不等于

<	小于

<=	小于等于

>	大于

>=	大于等于

BETWEEN	在指定两个值之间，包括指定的值

#### BETWEEN 使用

获取价格在5-10元的商品
```linux
mysql> SELECT prod_name,prod_price FROM products WHERE prod_price BETWEEN 5 AND 10 ORDER BY prod_price DESC;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| Bird seed      |      10.00 |
| TNT (5 sticks) |      10.00 |
| 1 ton anvil    |       9.99 |
| Oil can        |       8.99 |
| .5 ton anvil   |       5.99 |
+----------------+------------+
5 rows in set (0.00 sec)
```
#### 空值null过滤

获取商品价格为null 的所有商品

```linux
mysql> SELECT prod_name FROM products WHERE prod_price IS NULL;
Empty set (0.00 sec)
```

获取商品价格不为null 的所有商品

```linux
mysql> SELECT prod_name FROM products WHERE prod_price IS NOT NULL;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```
## AND OR IN NOT 操作符

操作符（operator）的定义：用来联结或改变WHERE字句中的关键字。也称为逻辑操作符

### AND

相当于语句中的并且，用作条件追加。

查询厂商id为1003 并且价格大于等于10

```Linux
mysql> SELECT vend_id,prod_id,prod_price,prod_name FROM products WHERE vend_id = 1003 AND prod_price >= 10 ORDER BY prod_name;
+---------+---------+------------+----------------+
| vend_id | prod_id | prod_price | prod_name      |
+---------+---------+------------+----------------+
|    1003 | FB      |      10.00 | Bird seed      |
|    1003 | DTNTR   |      13.00 | Detonator      |
|    1003 | SAFE    |      50.00 | Safe           |
|    1003 | TNT2    |      10.00 | TNT (5 sticks) |
+---------+---------+------------+----------------+
4 rows in set (0.00 sec)
```
AND 可以在一次查询中多次使用

### OR

相当于语句中或者，用于条件选择。

查询厂商id为1003或1002的商品信息

```linux
mysql> SELECT vend_id,prod_name,prod_price FROM products WHERE vend_id = 1003 OR vend_id = 1002 ORDER BY prod_name;
+---------+----------------+------------+
| vend_id | prod_name      | prod_price |
+---------+----------------+------------+
|    1003 | Bird seed      |      10.00 |
|    1003 | Carrots        |       2.50 |
|    1003 | Detonator      |      13.00 |
|    1002 | Fuses          |       3.42 |
|    1002 | Oil can        |       8.99 |
|    1003 | Safe           |      50.00 |
|    1003 | Sling          |       4.49 |
|    1003 | TNT (1 stick)  |       2.50 |
|    1003 | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
9 rows in set (0.00 sec)
```
OR 可以在一次查询中多次使用

### AND OR 联合使用

使用时需要注意优先级问题。

MySQL中默认 AND 与 OR 同时出现时 AND 优先被执行。

误区案例

查询厂商为1002或1003的价格大于等于10的商品
```linux
mysql> SELECT vend_id,prod_name,prod_price FROM products WHERE vend_id = 1002 OR vend_id = 1003 AND prod_price >= 10 ORDER BY prod_name;
+---------+----------------+------------+
| vend_id | prod_name      | prod_price |
+---------+----------------+------------+
|    1003 | Bird seed      |      10.00 |
|    1003 | Detonator      |      13.00 |
|    1002 | Fuses          |       3.42 |
|    1002 | Oil can        |       8.99 |
|    1003 | Safe           |      50.00 |
|    1003 | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
6 rows in set (0.00 sec)
```
此时发现 价格 为 3.42 和 8.99的商品被查出。原因是上面所说的优先级导致的。

误区解决

```linux
mysql> SELECT vend_id,prod_name,prod_price FROM products WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10 ORDER BY prod_name;
+---------+----------------+------------+
| vend_id | prod_name      | prod_price |
+---------+----------------+------------+
|    1003 | Bird seed      |      10.00 |
|    1003 | Detonator      |      13.00 |
|    1003 | Safe           |      50.00 |
|    1003 | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
4 rows in set (0.00 sec)
```

当OR条件加入（）时它的优先级就大于了AND,就像是数学运算的优先级问题一样被（）解决。

AND 和 OR 同时使用的场景下建议使用（）。

### IN

IN 用来指定条件范围
使用OR操作符过滤数据的一些案例，同样使用IN也可以完成

查询厂商id为1003或1002的商品信息

```linux
mysql> SELECT vend_id,prod_name,prod_price FROM products WHERE vend_id IN (1002,1003) ORDER BY prod_name;
+---------+----------------+------------+
| vend_id | prod_name      | prod_price |
+---------+----------------+------------+
|    1003 | Bird seed      |      10.00 |
|    1003 | Carrots        |       2.50 |
|    1003 | Detonator      |      13.00 |
|    1002 | Fuses          |       3.42 |
|    1002 | Oil can        |       8.99 |
|    1003 | Safe           |      50.00 |
|    1003 | Sling          |       4.49 |
|    1003 | TNT (1 stick)  |       2.50 |
|    1003 | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
9 rows in set (0.00 sec)
```

IN与OR比较具有的优点如下

1.语法更清晰简洁

2.在使用IN时操作符优先级问题会减少

3.执行更快

4.IN可以包含其他的SELECT语句。使得能够动态创建WHERE字句

### NOT

WHERE 字句中的NOT操作符有且只能有一个，功能是否定之后所跟的任何条件。


查询厂商id不是1002,1003的所有商品信息
```linux
mysql> SELECT prod_name,prod_price FROM products WHERE vend_id NOT IN (1002,1003) ORDER BY prod_name;
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
| JetPack 1000 |      35.00 |
| JetPack 2000 |      55.00 |
+--------------+------------+
5 rows in set (0.00 sec)
```

NOT支持的取反的操作符有 IN、BETWEEN、EXISTS。

## 通配符进行过滤

### LIKE

LIKE 是开启通配符匹配的关键字。

通配符号 % _

%表示匹配任意字符
_表示匹配一个字符


查询商品名称中包含 anvil
```linux
mysql> SELECT prod_id,prod_name FROM products WHERE prod_name LIKE '%anvil';
+---------+--------------+
| prod_id | prod_name    |
+---------+--------------+
| ANV01   | .5 ton anvil |
| ANV02   | 1 ton anvil  |
| ANV03   | 2 ton anvil  |
+---------+--------------+
3 rows in set (0.00 sec)

mysql> SELECT prod_id,prod_name FROM products WHERE prod_name LIKE '_ ton anvil';
+---------+-------------+
| prod_id | prod_name   |
+---------+-------------+
| ANV02   | 1 ton anvil |
| ANV03   | 2 ton anvil |
+---------+-------------+
2 rows in set (0.00 sec)

```

使用通配符的技巧

1.不要过度使用通配符，会造成数据库运算压力。

2.在明确需要使用通配符时，除非绝对有必要，否则不要把他们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。

3.仔细注意通配符的位置。

## 使用正则表达式进行搜索

MySQL开启使用正则表达式的关键字符是 REGEXP

```linux
mysql> SELECT prod_name FROM products WHERE prod_name REGEXP '.000' ORDER BY prod_name;
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

## 创建计算字段

### 计算字段

计算字段是原先表中不存在的字段，通过使用数据库函数或计算后并重命名的字段。

#### 拼接字段

vendors 表中厂商名称和地址是分离的。客户端需要获取一个vend—title的字段。格式是这样的 vend_name (vend_country)

```linux
mysql> SELECT Concat(Trim(vend_name),' (',Trim(vend_country),')') AS vend_title FROM vendors ORDER BY vend_name;
+-------------------------+
| vend_title              |
+-------------------------+
| ACME (USA)              |
| Anvils R Us (USA)       |
| Furball Inc. (USA)      |
| Jet Set (England)       |
| Jouets Et Ours (France) |
| LT Supplies (USA)       |
+-------------------------+
6 rows in set (0.00 sec)
```

其中函数 Concat 用于链接多个字符串

Trim函数用于去除字段左右的空格

AS 关键字用于起别名 alias

#### 执行算数计算

在表查询中，需要添加一个字段，这个字段来至于表中2个字段的乘积

```linux
mysql> SELECT prod_id,quantity,item_price,quantity*item_price AS expended_price FROM orderitems ORDER BY expended_price;
+---------+----------+------------+----------------+
| prod_id | quantity | item_price | expended_price |
+---------+----------+------------+----------------+
| SLING   |        1 |       4.49 |           4.49 |
| OL1     |        1 |       8.99 |           8.99 |
| FB      |        1 |      10.00 |          10.00 |
| FB      |        1 |      10.00 |          10.00 |
| ANV03   |        1 |      14.99 |          14.99 |
| ANV02   |        3 |       9.99 |          29.97 |
| TNT2    |        5 |      10.00 |          50.00 |
| JP2000  |        1 |      55.00 |          55.00 |
| ANV01   |       10 |       5.99 |          59.90 |
| FC      |       50 |       2.50 |         125.00 |
| TNT2    |      100 |      10.00 |        1000.00 |
+---------+----------+------------+----------------+
11 rows in set (0.00 sec)
```

### 测试计算

使用 SELECT 函数 或运算即可

测试now函数获取当前时间
```linux
mysql> SELECT NOW();
+---------------------+
| NOW()               |
+---------------------+
| 2019-09-03 16:07:01 |
+---------------------+
1 row in set (0.00 sec)
```

测试 三乘五
```linux
mysql> SELECT 3*5;
+-----+
| 3*5 |
+-----+
|  15 |
+-----+
1 row in set (0.00 sec)

```

## MySQL函数

sql是通用的，但是函数并不是通用的。以下介绍关于MySQL提供的函数

### 文本处理函数

下面列举一些关于MySQL处理文本的函数

Left()	返回字符串左边的字符

length()	返回字符串的长度

Locate()	找出字符串的一个子串

Lower()		将字符串转换为小写

LTrim()		去掉字符串左边的空格

Right()		返回字符串右边的字符

RTrim()		去掉字符串右边的空格

Soundex()	返回字符串的soundex值（用于声音模糊查询）

SubString()	返回字符串的字符

Upper()		将字符串转换为大写

Upper示例

```linux
mysql> SELECT vend_name,UPPER(vend_name) AS vend_name_upper FROM vendors ORDER BY vend_name;
+----------------+-----------------+
| vend_name      | vend_name_upper |
+----------------+-----------------+
| ACME           | ACME            |
| Anvils R Us    | ANVILS R US     |
| Furball Inc.   | FURBALL INC.    |
| Jet Set        | JET SET         |
| Jouets Et Ours | JOUETS ET OURS  |
| LT Supplies    | LT SUPPLIES     |
+----------------+-----------------+
6 rows in set (0.00 sec)
```
### 日期和时间处理函数

关于MySQL处理日期和时间的函数

AddDate()	添加一个日期

AddTime()	添加一个时间

CurDate()	返回当前日期

CurTime()	返回当前时间

Date()		返回日期时间的日期部分

DateDiff()	计算两个日期之差

Date_Add()	高度灵活的日期运算函数

Date_Format	返回一个格式化的日期或时间串

Day()		返回一个日期的天数部分

DayOfWeek()	对于一个日期返回对应的星期

Hour()		返回时间中的小时

Minute()	返回时间中的分钟

Month()		返回一个日期的月份部分

Now()		返回当前日期和时间

Second()	返回一个时间的秒

Time()		返回日期时间的时间部分

Year()		返回一个日期的年份部分

Year示例

```linux
mysql> SELECT order_date,Year(order_date) AS order_date_year From orders ORDER BY order_date_year;
+---------------------+-----------------+
| order_date          | order_date_year |
+---------------------+-----------------+
| 2005-09-01 00:00:00 |            2005 |
| 2005-09-12 00:00:00 |            2005 |
| 2005-09-30 00:00:00 |            2005 |
| 2005-10-03 00:00:00 |            2005 |
| 2005-10-08 00:00:00 |            2005 |
+---------------------+-----------------+
5 rows in set (0.00 sec)
```

### MySQL中的数值处理函数

Abs()	返回一个数的绝对值

Cos()	返回一个角度的余弦

Exp()	返回一个数的指数值

Mod()	返回操作的余数

Pi()	返回圆周率

Rand()	返回一个随机数

Sin()	返回一个角度的正弦

Sqrt()	返回一个数的平方根

Tan()	返回一个角度的正切

## 汇总数据

### 聚集函数

#### AVG

AVG()	返回某列的平均值

案例：获取所有商品的平均价格

```linux
mysql> SELECT AVG(prod_price) AS prod_price_avg FROM products;
+----------------+
| prod_price_avg |
+----------------+
|      16.133571 |
+----------------+
1 row in set (0.00 sec)
```

#### COUNT

COUNT()	统计表中的行数，参数（parameter）为*时代表所有列null也会被统计。当指定特定的列时只针对指定的列统计并且null不会计入统计。

案例：统计商品个数

```linux
mysql> SELECT COUNT(*) FROM customers;
+----------+
| COUNT(*) |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)

mysql> SELECT COUNT(cust_email) FROM customers;
+-------------------+
| COUNT(cust_email) |
+-------------------+
|                 3 |
+-------------------+
1 row in set (0.00 sec)
```

#### MAX

MAX()	获取指定列中的最大值，必须传入指定的列。当遇到null会忽略

案例：获取商品的最大价格

```linux
mysql> SELECT MAX(prod_price) AS max_price FROM products;
+-----------+
| max_price |
+-----------+
|     55.00 |
+-----------+
1 row in set (0.00 sec)
```

#### MIN

MIN()	与MAX相反获取列中的最小值。注意事项一致。

案例：获取商品的最小值

```linux
mysql> SELECT MIN(prod_price) AS min_price FROM products;
+-----------+
| min_price |
+-----------+
|      2.50 |
+-----------+
1 row in set (0.00 sec)
```

#### SUN

SUN()	求和函数。null被忽略

案例：获取20005订单的商品的数量

```linux
mysql> SELECT SUM(quantity) AS items_ordered FROM orderitems WHERE order_num = 20005;
+---------------+
| items_ordered |
+---------------+
|            19 |
+---------------+
1 row in set (0.00 sec)
```

### 聚集函数中DISTINCT和All

聚集函数默认取值范围是all（指定列的所有值）我们可以在上面的几个聚集函数中使用DISTINCT去重后进行聚合计算

上面的5个函数中MAX和MIN使用DISTINCT无意义。因为我们要获取的是最大值或最小值

DISTINCT不能用于COUNT(*)，因为不允许使用COUNT(DISTINCT)。类似的DISTINCT必须使用列名。

案例：获取不同商品价格的平均值

```linux
mysql> SELECT AVG(DISTINCT prod_price) AS avg_price FROM products;
+-----------+
| avg_price |
+-----------+
| 17.780833 |
+-----------+
1 row in set (0.00 sec)
```

### 组合聚合函数

案例：获取所有商品数量，商品的最大值、最小值以及商品的平均值

```linux
mysql> SELECT COUNT(*) AS num_item,MAX(prod_price) AS max_price,MIN(prod_price) AS min_price,AVG(prod_price) AS avg_price FROM products;
+----------+-----------+-----------+-----------+
| num_item | max_price | min_price | avg_price |
+----------+-----------+-----------+-----------+
|       14 |     55.00 |      2.50 | 16.133571 |
+----------+-----------+-----------+-----------+
1 row in set (0.00 sec)
```
## 分组数据

### 数据分组

数据为什么要分组，当遇到求每组每种......的需求时我们可以考虑使用分组。

使用group by可以实现分组声明。同时下面列举一些关于分组的规定

1.GROUP BY 分组后，数据将在最后规定的分组上进行汇总。也就是说分组后不能从个别的列中获取数据

2.GROUP BY子句中列出的每列都必须是检索或有效的表达式（不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。

3.除聚集计算外，SELECT语句中的每个列必须在GROUP BY字句中给出

4.如果分组中存在null值，则将null作为一个分组返回。如果列中存在多行null值，它们将分为一组。

5.GROUP BY 语法顺序在 WHERE之后ORDER BY之前。

案例：获取每个厂商的商品个数

```linux
mysql> SELECT vend_id,COUNT(*) FROM products GROUP BY vend_id ORDER BY vend_id;
+---------+----------+
| vend_id | COUNT(*) |
+---------+----------+
|    1001 |        3 |
|    1002 |        2 |
|    1003 |        7 |
|    1005 |        2 |
+---------+----------+
4 rows in set (0.00 sec)
```

### 过滤分组

就是在分组的前提下过滤数据，可使用HAVING完成过滤

案例：获取每个厂商商品数量，厂商商品数量小于2过滤掉。

```linux
mysql> SELECT vend_id,COUNT(*) AS prod_num FROM products GROUP BY vend_id HAVING prod_num >=2;
+---------+----------+
| vend_id | prod_num |
+---------+----------+
|    1001 |        3 |
|    1002 |        2 |
|    1003 |        7 |
|    1005 |        2 |
+---------+----------+
4 rows in set (0.00 sec)
```
### 过滤分组+排序

案例：获取每个厂商商品数量，厂商商品数量小于2过滤掉。最后按商品数量的升序排列

```linux
mysql> SELECT vend_id,COUNT(*) AS prod_num FROM products GROUP BY vend_id HAVING prod_num >=2 ORDER BY prod_num;
+---------+----------+
| vend_id | prod_num |
+---------+----------+
|    1002 |        2 |
|    1005 |        2 |
|    1001 |        3 |
|    1003 |        7 |
+---------+----------+
4 rows in set (0.01 sec)
```

### 行过滤+分组过滤+排序

案例：获取每个厂商商品数量，过滤掉商品价格小于10的商品，厂商商品数量小于2也过滤掉。最后按商品数量的升序排列

```linux
mysql> SELECT vend_id,COUNT(*) AS prod_num FROM products WHERE prod_price >=10  GROUP BY vend_id HAVING prod_num >=2 ORDER BY prod_num;
+---------+----------+
| vend_id | prod_num |
+---------+----------+
|    1005 |        2 |
|    1003 |        4 |
+---------+----------+
2 rows in set (0.00 sec)
```
### WHERE GROUP BY HAVING ORDER BY 各自的作用

WHERE 	用于行过滤

GROUP BY 	用于分组说明

HAVING 		用于分组过滤

ORDER BY 	用于输出排序

## 子查询

首先子查询是MySQL4.1引入的特性。

子查询可以组合使用

子查询的效率不高，尽量少用

下面列举2个例子

1.列出订购物品TNT2的所有客户信息

```linux

mysql> SELECT cust_name,cust_contact FROM customers WHERE cust_id IN (SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2'))
    -> ;
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
2 rows in set (0.00 sec)
```
2.获取customers表中每个顾客的订单总数

```linux
mysql> SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE customers.cust_id = orders.cust_id) AS orders FROM customers ORDER BY cust_name;
+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         |      2 |
| E Fudd         | IL         |      1 |
| Mouse House    | OH         |      0 |
| Wascals        | IN         |      1 |
| Yosemite Place | AZ         |      1 |
+----------------+------------+--------+
5 rows in set (0.00 sec)
```

使用子查询的诀窍是分步骤循序渐进的运行（编写）sql

## 联结表

联结表就是将一些关系表联结起来，获取数据。

### 关系表

关系表简单说就是2张表之间存在关联性。它具有以下优点

1.因为每张表都是根据其特性建立的，所以它不允许出现重复的字段。复用性强

2.可以细粒度的利用字段进行查询过滤

3.操作简单，一次操作多次引用

联结案例：

```Linux
mysql> SELECT vend_name,prod_name,prod_price FROM vendors,products WHERE vendors.vend_id = products.vend_id ORDER BY vend_name,prod_name;
+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Detonator      |      13.00 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
+-------------+----------------+------------+
14 rows in set (0.00 sec)
```

### 内联结

#### 笛卡尔集

```linux
mysql> SELECT vend_name,prod_name,prod_price FROM vendors,products ORDER BY vend_name,prod_name;
+----------------+----------------+------------+
| vend_name      | prod_name      | prod_price |
+----------------+----------------+------------+
| ACME           | .5 ton anvil   |       5.99 |
| ACME           | 1 ton anvil    |       9.99 |
| ACME           | 2 ton anvil    |      14.99 |
| ACME           | Bird seed      |      10.00 |
| ACME           | Carrots        |       2.50 |
| ACME           | Detonator      |      13.00 |
| ACME           | Fuses          |       3.42 |
| ACME           | JetPack 1000   |      35.00 |
| ACME           | JetPack 2000   |      55.00 |
| ACME           | Oil can        |       8.99 |
| ACME           | Safe           |      50.00 |
| ACME           | Sling          |       4.49 |
| ACME           | TNT (1 stick)  |       2.50 |
| ACME           | TNT (5 sticks) |      10.00 |
| Anvils R Us    | .5 ton anvil   |       5.99 |
| Anvils R Us    | 1 ton anvil    |       9.99 |
| Anvils R Us    | 2 ton anvil    |      14.99 |
| Anvils R Us    | Bird seed      |      10.00 |
| Anvils R Us    | Carrots        |       2.50 |
| Anvils R Us    | Detonator      |      13.00 |
| Anvils R Us    | Fuses          |       3.42 |
| Anvils R Us    | JetPack 1000   |      35.00 |
| Anvils R Us    | JetPack 2000   |      55.00 |
| Anvils R Us    | Oil can        |       8.99 |
| Anvils R Us    | Safe           |      50.00 |
| Anvils R Us    | Sling          |       4.49 |
| Anvils R Us    | TNT (1 stick)  |       2.50 |
| Anvils R Us    | TNT (5 sticks) |      10.00 |
| Furball Inc.   | .5 ton anvil   |       5.99 |
| Furball Inc.   | 1 ton anvil    |       9.99 |
| Furball Inc.   | 2 ton anvil    |      14.99 |
| Furball Inc.   | Bird seed      |      10.00 |
| Furball Inc.   | Carrots        |       2.50 |
| Furball Inc.   | Detonator      |      13.00 |
| Furball Inc.   | Fuses          |       3.42 |
| Furball Inc.   | JetPack 1000   |      35.00 |
| Furball Inc.   | JetPack 2000   |      55.00 |
| Furball Inc.   | Oil can        |       8.99 |
| Furball Inc.   | Safe           |      50.00 |
| Furball Inc.   | Sling          |       4.49 |
| Furball Inc.   | TNT (1 stick)  |       2.50 |
| Furball Inc.   | TNT (5 sticks) |      10.00 |
| Jet Set        | .5 ton anvil   |       5.99 |
| Jet Set        | 1 ton anvil    |       9.99 |
| Jet Set        | 2 ton anvil    |      14.99 |
| Jet Set        | Bird seed      |      10.00 |
| Jet Set        | Carrots        |       2.50 |
| Jet Set        | Detonator      |      13.00 |
| Jet Set        | Fuses          |       3.42 |
| Jet Set        | JetPack 1000   |      35.00 |
| Jet Set        | JetPack 2000   |      55.00 |
| Jet Set        | Oil can        |       8.99 |
| Jet Set        | Safe           |      50.00 |
| Jet Set        | Sling          |       4.49 |
| Jet Set        | TNT (1 stick)  |       2.50 |
| Jet Set        | TNT (5 sticks) |      10.00 |
| Jouets Et Ours | .5 ton anvil   |       5.99 |
| Jouets Et Ours | 1 ton anvil    |       9.99 |
| Jouets Et Ours | 2 ton anvil    |      14.99 |
| Jouets Et Ours | Bird seed      |      10.00 |
| Jouets Et Ours | Carrots        |       2.50 |
| Jouets Et Ours | Detonator      |      13.00 |
| Jouets Et Ours | Fuses          |       3.42 |
| Jouets Et Ours | JetPack 1000   |      35.00 |
| Jouets Et Ours | JetPack 2000   |      55.00 |
| Jouets Et Ours | Oil can        |       8.99 |
| Jouets Et Ours | Safe           |      50.00 |
| Jouets Et Ours | Sling          |       4.49 |
| Jouets Et Ours | TNT (1 stick)  |       2.50 |
| Jouets Et Ours | TNT (5 sticks) |      10.00 |
| LT Supplies    | .5 ton anvil   |       5.99 |
| LT Supplies    | 1 ton anvil    |       9.99 |
| LT Supplies    | 2 ton anvil    |      14.99 |
| LT Supplies    | Bird seed      |      10.00 |
| LT Supplies    | Carrots        |       2.50 |
| LT Supplies    | Detonator      |      13.00 |
| LT Supplies    | Fuses          |       3.42 |
| LT Supplies    | JetPack 1000   |      35.00 |
| LT Supplies    | JetPack 2000   |      55.00 |
| LT Supplies    | Oil can        |       8.99 |
| LT Supplies    | Safe           |      50.00 |
| LT Supplies    | Sling          |       4.49 |
| LT Supplies    | TNT (1 stick)  |       2.50 |
| LT Supplies    | TNT (5 sticks) |      10.00 |
+----------------+----------------+------------+
84 rows in set (0.00 sec)
```

#### inner join(ANSI SQL语法)

```linux
mysql> SELECT vend_name,prod_name,prod_price FROM vendors INNER JOIN products ON vendors.vend_id = products.vend_id;
+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
| ACME        | Detonator      |      13.00 |
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
+-------------+----------------+------------+
14 rows in set (0.00 sec)
```

当两个表中vend_id都同时存在并对应的数据

### 性能问题

联结会影响数据库性能，所以使用联结时要尽量避免出现不必要的表联结。

## 高级联结

### 不同类型的联结

#### 自联结

单个表使用别名被联结后获取数据的情况属于自联结

```linux
mysql> SELECT p1.prod_id,p1.prod_name FROM products as p1 INNER JOIN products as p2 ON p1.vend_id = p2.vend_id WHERE p2.prod_id = 'DTNTR' ORDER BY p1.prod_name;
+---------+----------------+
| prod_id | prod_name      |
+---------+----------------+
| FB      | Bird seed      |
| FC      | Carrots        |
| DTNTR   | Detonator      |
| SAFE    | Safe           |
| SLING   | Sling          |
| TNT1    | TNT (1 stick)  |
| TNT2    | TNT (5 sticks) |
+---------+----------------+
7 rows in set (0.00 sec)
```

自联结的好处是，它可以替代子查询完成相同的操作。并且比子查询性能要高

#### 自然联结

明确联结后检索的列，并且不重复的的情况属于自然联结

之前的大多数联结案例都属于自然联结

#### 外部联结

外部联结分为做联结和右联结

### 使用联结的注意事项

1.联结的时候，联结类型要考虑清楚

2.联结条件很重要，所以也需要深思。不提供条件的话会出现笛卡尔集

## 组合查询

组合查询就是将多条sql查询组合后使用。前提是这两条sql语句检索的列必须一致。

### 创建组合查询

```linux
mysql> SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <=5 UNION SELECT vend_id,prod_id,prod_price FROM products WHERE vend_id IN (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
8 rows in set (0.00 sec)
```

组合查询使用注意事项

1. 组合查询中SELECT最少为2条

2. 组合查询中每条查询语句中检索的列必须相同，顺序可以不一样。数据类型是数据库支持的类型

3. 组合查询ORDER by要用在最后.

4. 组合查询会自动过滤重复的数据。如果不想被过滤可以使用 union all 如下面的例子

```linux
mysql> SELECT vend_id,prod_id,prod_price FROM products WHERE prod_price <=5 UNION ALL SELECT vend_id,prod_id,prod_price FROM products WHERE vend_id IN (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
9 rows in set (0.00 sec)
```

## 全文本搜索

由于本章内容myISAM 引擎不支持中文所以不做笔记介绍

## 插入数据

### 插入完整一行

```linux
mysql> INSERT INTO customers VALUES(null,'Pep E. LaPew','100 Main Street','Los Angeles','CA','90046','USA',null,null);
Query OK, 1 row affected (0.04 sec)
```
#### 提高性能降低insert的优先级

如果数据检索是最重要的，在insert into 将加入 low_priority可降低insert语句的优先级。这样别的用户的查询语句就有可能被优先执行。

```linux
insert low_priority into
```
### 插入多个行

```linux
mysql> INSERT INTO customers(cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country) values('Pep e. LaPew','100 Main Street','Los Angeles','CA','90046','USA'),('M.martian','42 Galaxy Way','New York','Ny','11213','USA');
Query OK, 2 rows affected (0.06 sec)
Records: 2  Duplicates: 0  Warnings: 0
```
### 插入检索后的行

```sql
INSERT INTO customers(
	cust_id,
	cust_contact,
	cust_email,
	cust_name,
	cust_address,
	cust_city,
	cust_country)
select 
	cust_id,
	cust_contact,
	cust_email,
	cust_name,
	cust_address,
	cust_city,cust_country 
from
	custnew;
```

### 插入时的注意事项

1. 插入数据时应该明确指定要插入的列

## 更新和删除数据

### 更新数据

```linux
mysql> UPDATE customers SET cust_email = 'elmer@fudd.com' WHERE cust_id = 10005;
Query OK, 1 row affected (0.09 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
### IGNORE

如果update更新多行，并且更新中如果一行或多行出现错误时，会取消整个update操作。使用ignore可以忽略错误行完成修改更新。(谨慎使用，个人认为它打破了sql执行时数据的一致性)

### 删除数据

```linux
mysql> DELETE FROM customers Where cust_id = 10006;
Query OK, 1 row affected (0.06 sec)
```
### 删除表内所有数据

```linux
mysql> truncate table custnew1;
Query OK, 0 rows affected (0.24 sec)
```

### update 和 delete使用注意事项

1. 更新或删除数据时要考虑where条件，除非操作是针对全表内容的

2. 更新或删除的where条件，操作前要在SELECT 中查询并确认