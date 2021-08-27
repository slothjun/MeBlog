---
title: python带我飞-note
---

## 第二篇 进阶

### 变量

变量是编程语言中最基础的术语，用来指计算机中存储的可变数据。如下例子

```python
Val = "hello"
```

#### 变量的规则

类型不同，存储功能不同

申明变量无需定义变量类型，直接赋值即可

如何做到直接赋值，原因是Python内部对变量进行了对象封装，并且它具有三个重要的属性id,type,value

#### 变量的类型

Python3中的类型划分为6种：

number 数字可以划分为 int float bool complex 等

string 字符串

tuple 元组

sets 集合

dictionaries 字典

#### 变量类型的帮助函数

dir 用来查询类型的所有属性。如dir(number)

help 用来查询类型具体的说明文档。如help(number)