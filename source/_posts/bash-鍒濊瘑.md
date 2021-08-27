---
title: bash-初识
---

## 什么是bash

简单的说，bash是一个命令解释工具

## 命令的语法以及规范

### 变量

变量和java中的变量所表示的含义大致相同，都是用一个引用表示地址值中的数据。

待验证：bash的变量只能存储文本。

变量的赋值方式和Java中变量赋值类似。下面列举几个例子，看后自会明白。

$var=word 注意等号的左右不能留有空格

$var='hello word'

$var="hello word"
上面两种都可以表示文本中包含空格的输入。不同之处在于，双引号中可以掺入变量，系统会放入变量引用的值。而单引号却只能打印所有输入的文本内容，变量引用也会当做文本。

$v1=word
$var="hello ${v1}test" ${}中放入的变量可以中缺的被标识并识别，如果不使用${} 表达式的含义将会变为引用v1!的变量此时不会输出 hello Wordtest 而是 hello 

$now=`date` 注意``中可以使用命令。查看now echo $now 这时会输出时间

$date=$now 变量之间也可以互相传值

$read name 这时Linux会等待用户输入，输入完成后会把输入的值赋值给name

### 数学运算

bash中的数学计算类似上学时的数学运算，只要吧运算表达式写入$(())中即可

加法运算 $((3+2))

减法运算 $((3-2))

乘法运算 $((3*2))

除法运算 $((3/2))

求余运算 $((2%3))

乘方运算 $((2**3))

### 返回代码

Linux命令执行完后会放回一个值，返回0表示这段命令执行成功
$? 可以查看当前命令执行的结果

### ；的使用

命令中使用；可以在一行命令中执行多个命令。如ls;$? 表示打印目录信息并回响命令返回值

### 短路与或

短路与，在多个命令中，后面命令的执行与否取决于之前命令的返回值，当返回值为0时之后的命令才会执行

如： rm dome.file && echo "remove dome.file sucess" 当删除过文件dome.file后会输出文本信息，失败后则不输出。

短路或，短路或正好是短路与的反向解释。当之前的命令失败后后面的命令才会执行，否则不执行

如：rm dome.file && echo "remove dome.file fail" 当没有删除文件dome.file后会输出文本信息，否则不会输出任何信息。

## bash script

### first bash file
```linux
#!/bin/bash

user=`whoami`
echo "Information of ${user} computer" >> log

lscpu >> log
uname -a >> log
free -h >> log

```
这是一个简单的bash脚本，刚刚解释了什么是bash他是一个命令解释器，所以bash脚步就是集多条bash命令的一个文本。
这样做的好处是我们可以记录使用命令的逻辑，并多次复用。

### bash中传入参数
bash 中的参数使用 $0 $1 ...表示

使用例子：

测试流程 1编辑bashscript 2运行 3显示结果

1 编辑script
```linux
#!/bin/bash

echo $0
echo $1
echo $2
```
2 运行
```linux
spiderbao@spiderbao-CW65S:~/my-bash$ ./test_arg.bash hello word
```
3.显示结果
```linux
./test_arg.bash
hello
word
```
### 函数

个人认为函数是对bash脚本的一次封装，通过内部定义函数可以将命令功能分块包装。从而达到脚本内函数的复用

### 无参函数示例

测试流程  1编写脚本 2运行 3展示结果

1 编写脚本
```Linux
#!/bin/bash

function meInfo(){
  date >> log
  lscpu >> log
  uname -a >> log
  free -h >> log
}

meInfo
```

2 运行

```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ ./test_function.bash 
```
3 显示结果
```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ cat log 
2019年 08月 26日 星期一 13:52:10 CST
xxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxx
xxxxxxxxxxxxx
x
```

#### 带参函数示例

测试流程  1编写脚本 2运行 3展示结果

1.编写脚本
```Linux
#!/bin/bash

function meInfo(){
  date >> $1
  echo 'hello' >> $1
  echo 'word'  >>  $1
}

meInfo melog
meInfo melog2
```

2 运行

```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ ./test_function.bash 
```
3 显示结果
```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ cat melog
2019年 08月 26日 星期一 14:14:37 CST
hello
word
spiderbao@spiderbao-CW65S:~/my-bash$ cat melog2
2019年 08月 26日 星期一 14:14:37 CST
hello
word
```
#### 跨脚本函数调用

首先跨脚本函数调用是基于脚本之上的操作，从此也可以看出函数封装的好处。

跨脚本函数调用主要使用source关键字

测试流程 1编写脚本 2 运行 3 展示结果

1 编写脚本

test_function.bash
```linux
#!/bin/bash

function meInfo(){
  date >> $1
  echo 'hello' >> $1
  echo 'word'  >>  $1
}

meInfo melog
meInfo melog2

```
app.bash
```linux
#!/bin/bash

source test_function.bash

meInfo melog3
meInfo log

```
2 运行

```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ ./app.bash 
```
3 展示结果

```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ cat melog melog2 melog3
2019年 08月 26日 星期一 14:30:22 CST
hello
word
2019年 08月 26日 星期一 14:30:22 CST
hello
word
2019年 08月 26日 星期一 14:30:22 CST
hello
word
```
可以看到source 引入的test_function.bash 脚本被执行了一遍，注意是整个脚本不单是脚本中的一个函数而是整个脚本。

### 逻辑判断

逻辑判断为的是判断一个函数式运行的真假或一个命令的真假

案例
```Linux
spiderbao@spiderbao-CW65S:~/my-bash$ test 3 -gt 2 ;echo $?
0
```
因为3大于2（-gt表示大于）所以命令的返回值为0

#### 关于数值的判断符

大于 -gt
小于 -lt
等于 -eq
不等于 -ne
大于等于 -ge
小于等于 -le

#### 关于文本的判断符

文本相同 =
文本不相同 !=
文本按字典排序一个文本在另一个文本之前 >
文本按字典排序一个文本在另一个文本之后 <

#### 关于文件判断符

检查文件是否存在 -e
检查文件是否存在并且是普通文件 -f
检查目录是否存在 -d
检查软连接是否存在 -l
检查文件是否可读 -r
检查文件是否可写 -w
检查文件是否可以执行 -x

#### 与或非
&& || !

注意 && || ! 不能联合使用

### 选择结构

#### if

if 选择结构，类似java中的流程控制语句if

案例
```Linux
#!/bin/bash

user=`whoami`
if [ $user = 'root' ]
then 
	echo "你是超级用户！"
fi

```
需要注意语法格式
#### if else

if else ,类似java中的流程控制语句 if else

案例

```Linux
#!/bin/bash

user=`whoami`
if [ $user = 'root' ]
then 
	echo "你是超级用户！"
else
	echo "你不是超级用户！"
fi

```
需要注意语法格式

if else 嵌套使用，类似java流程控制语句中的if else if else

案例

```linux
#!/bin/bash

user=`whoami`
if [ $user = 'root' ]
then 
	echo "你是超级用户！"
else  if [ $user = 'laowang' ]
then
	echo "你是老王用户！"
else if [ $user = 'spiderbao' ]
then
	echo '你是spidermen的朋友spiderbao!'
else
	echo '我也不知道你是谁！'
fi
fi
fi
```
格式样式模仿Java的if else if else ，唯一不同处在于结尾fi使用了多少个if就需要添加多少个fi来结尾。毕竟这是一个嵌套语句。

#### case

case 类似java流程控制语句中的switch

案例
```linux
#!/bin/bash

user=`whoami`

case $user in
	root)
		echo "你是超级管理员用户！"
	;;
	laowang)
		echo "你是老王用户！"
	;;
	spiderbao)
		echo '你是spiderbao用户！'
	;;
	*)
		echo '我也不知道你是谁！'
	;;
esac
```
需要注意的是 ）中可以填写文本以及通配符
通配符
* 表示任意个数的任意字符
? 表示一个字符的任意字符
[] 类似正则表达式中的[]，表示可选范围内的字符，另外[]只占一个字符位 。例如[1-5][a-x] 1a,2b 均匹配