---
title: mongodb
---
## mongodb入门

### 简介

mongodb是一个基于分布式文件存储的数据库。由c++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。

mongoab是一个介于关系型数据库和非关系型数据库之间的产品，是非关系型数据库当中功能丰富，最像关系型数据库的一个产品。它支持的数据格式有json,bson等。他最大的特点是它的查询语法类似于面向对象语言。并且它还支持对数据创建索引。

下面的图片是百度百科对mongodb的介绍
![](http://39.104.171.29/image/mongodb-1.png)

### 基础概念

在mongodb中是通过数据库、集合、文档的方式来管理数据的。下边是mongodb与关系型数据库的一些概念比较图。
![](http://39.104.171.29/image/mongodb-2.png)
和关系型数据库一样，mongodb允许一个实例创建多个数据库，一个数据库创建多个集合，一个集合创建多个文档

### 链接mongodb

mongodb使用CS架构开发。client shell使用链接命令如下：
```text
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```
![](http://39.104.171.29/image/mongodb-3.png)

### 数据库操作的相关命令

#### 查询数据库
show dbs: 查询所有数据库信息
![](http://39.104.171.29/image/mongodb-4.png)

db
![](http://39.104.171.29/image/mongodb-5.png)

#### 创建数据库
use 库名
使用指定的库，没有则创建（注意：当使用一个新建的库，而且没有插入任何数据时。此时的库实际没有被创建。）
![](http://39.104.171.29/image/mongodb-6.png)

#### 删除数据库
db.dropDatabase
![](http://39.104.171.29/image/mongodb-7.png)

### 集合相关操作命令

#### 创建集合

db.createConllection(name,options)
name:表示集合的名称
options:表示创建参数

#### 删除集合

db.<Conllection>.drop()
<collection> 表示选中的集合

### 文档先关操作

#### 集合中插入文档

db.<collection>.insert(document)
<collection> 表示选中的集合
document:表示文档，可以是json类型的数据格式


#### 集合中修改指定文档信息
db.<collection>.update(<query>,<update>,<options>)
<collection> 表示选中的集合
<query> 表示 查询条件,相当于sql语句的where
<update> 表示 更新文档内容
<options> 表示 选项

#### $set修改器

db.test_collection.update({"age":18},{$set:{"name":"wan","age":16}},{multi:true})

test_collection：集合名称

{"age":18} ：查询结果

{$set:{"name":"wan","age":16}} 修改条件

{multi:true}：参数 multi 为true时表示修改全部，false时表示修改第一个匹配的文档。默认为false


#### 集合中移除指定文档

db.<collection>.remove(<query>)
<collection> 表示选中的集合
<query> 表示查询的结果
删除前最好先执行查询

#### 集合中搜索指定文档 

db.<collection>.find(query,projection)
<collection> 表示选中的集合
query :查询条件
projection:投影查询key

投影查询
db.test_collection.find({"age":18},{name:1,age:1,address:1,_id:0})
test_collection:制定的集合
{"age":18} ：查询条件
{name:1,age:1,address:1,_id:0} ：投影参数，个人不明白只有_id可以设置为0，如果别的设置为0会报Projection cannot have a mix of inclusion and exclusion.异常

### 用户相关操作命令

#### 添加用户
```text
db.createUser({
    user:"spiderBao",
    pwd:"xxx",
    roles:[
        {role:"root",db:"admin"}
    ]
    })
```
#### 修改用户
```text
db.updateUser(
    'spiderBao1',{
        roles:[
        {role:"readWriteAnyDatabase",db:"admin"}]
        })
```
#### 查询用户
```text
show users
```
#### 删除用户
```text
db.dropUser("spiderBao1")
```
#### 修改密码
```text
db.changeUserPassword("test1","123")
```