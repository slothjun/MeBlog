---
title: python_argparse
---

## argparse由来

​	原文：

  The argparse module is a command line parsing library which provides more functionality than the existing command line parsing modules in the standard library, getopt [2] and optparse [3]. It includes support for positional arguments (not just options), subcommands, required options, options syntaxes like "/f" and "+rgb", zero-or-more and one-or-more style arguments, and many other features the other two lack.

  The argparse module is also already a popular third-party replacement for these modules. It is used in projects like IPython (the Scipy Python shell) [4], is included in Debian testing and unstable [5], and since 2007 has had various requests for its inclusion in the standard library [6] [7] [8]. This popularity suggests it may be a valuable addition to the Python libraries.

  个人理解：

  argparse 是一个命令行分析库，它比标准库中的getopt optparse要强大。argparse属于目前流行的第三方命令分析库。

## 命令解析模块必知的一些基础概念

### 位置参数
      
      如 ls /home/XXX  其中的 ‘/home/XXX’就称之为位置参数
### 可选参数
    
      如 ls -l 其中的 ‘-l’ 就称之为可选参数
### 帮助文档

      如 ls --help 

## 入门dome1

  ```python
    import argparse

    parse = argparse.ArgumentParser()
    parse.parse_args()
  ```
  用来创建一个基础的命令程序
  ```python
  import argparse

  parse = argparse.ArgumentParser()
  parse.add_argument('echo',help='回显你输入的字符串')
  args = parse.parse_args()
  print(args.echo)
  ```
  当用户使用命令行python 文件名 --help 会查看到
  positional arguments:
  echo        回显你输入的字符串


  positional arguments:

    what is positional arguments?

    通常它代表命令中的坐标信息，并且它一个必选项参数。

    下面的dome演示如何构建及其调用
  ```python
  import argparse

  parse = argparse.ArgumentParser()
  parse.add_argument('echo',help='回显你输入的字符串')
  parse.add_argument('square',help='计算输入数的平方',type=int)

  args = parse.parse_args()
  print(args.echo)
  print(args.square**2)
  ```
  当用户使用命令行python 文件名 --help 会查看到
  positional arguments:
  echo        回显你输入的字符串
  square      计算输入数的平方

  optional arguments:
  -h, --help  show this help message and exit


  optional arguments

    what is optional arguments?

    它可以代表命令中的一些附件信息，如下面的2个dome一个是表示选择只有一个参数，一个表示选项可以有多个参数

### 多参数
  ```python
  import argparse

  parse = argparse.ArgumentParser()

  parse.add_argument('--verbosity',help="increase output verbosity")
  args = parse.parse_args()

  if args.verbosity:
      print('verbosity turned on')
  ```

  ```linux
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py --version
    usage: argparse_study.py [-h] [--verbosity VERBOSITY]
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -h
    usage: argparse_study.py [-h] [--verbosity VERBOSITY]

    optional arguments:
      -h, --help            show this help message and exit
      --verbosity VERBOSITY
                            increase output verbosity
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -verbosity 1
    usage: argparse_study.py [-h] [--verbosity VERBOSITY]
    argparse_study.py: error: unrecognized arguments: -verbosity 1
    spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py --verbosity 1
    verbosity turned on
    spiderbao@spiderbao-CW65S:~/python/study/code$ 

  ```

### 单参数
  ```python
  import argparse

  parse = argparse.ArgumentParser()

  parse.add_argument('--verbosity',help="increase output verbosity",action='store_true')
  args = parse.parse_args()

  if args.verbosity:
      print('verbosity turned on')

  ```

  ```linux
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py --verbosity
  verbosity turned on
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py --verbosity 1
  usage: argparse_study.py [-h] [--verbosity]
  argparse_study.py: error: unrecognized arguments: 1
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -h
  usage: argparse_study.py [-h] [--verbosity]

  optional arguments:
    -h, --help   show this help message and exit
    --verbosity  increase output verbosity
  spiderbao@spiderbao-CW65S:~/python/study/code$ 
  ```
  
### 短选项
  ```python
  import argparse
  parse = argparse.ArgumentParser()
  parse.add_argument('-v','--verbosity',help='increase output verbosity',action='store_true')
  args = parse.parse_args()

  if args.verbosity:
    print('verbosity turned on')

  ```
  命令行输入
  ```linux
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -v 1
  verbosity turned on
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -v 
  verbosity turned on
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py --verbosity
  verbosity turned on
  ```

### 组合Positional和Optional参数
  ```python
  parse = argparse.ArgumentParser()
  parse.add_argument('-v','--verbosity',help='显示输出详情',action='count',default=1)
  parse.add_argument('square',help='计算输入数的平方',type=int)
  args = parse.parse_args()
  answer = args.square**2
  if args.verbosity>=1:
    print('%s^2=%s' %(args.square,answer))
  if args.verbosity>=2:
    print('runing %s'%__file__)
  if args.verbosity<1:
    print(answer)

  ```
  命令行输入显示
  ```linux
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -h 
  usage: argparse_study.py [-h] [-v] square

  positional arguments:
    square           计算输入数的平方

  optional arguments:
    -h, --help       show this help message and exit
    -v, --verbosity  显示输出详情
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -h
  usage: argparse_study.py [-h] [-v] square

  positional arguments:
    square           计算输入数的平方

  optional arguments:
    -h, --help       show this help message and exit
    -v, --verbosity  显示输出详情
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4
  4^2=16
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 -v
  4^2=16
  runing argparse_study.py
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 -vv
  4^2=16
  runing argparse_study.py

  ```
### 选参互斥异常
  ```python
  parse = argparse.ArgumentParser(description='calculate X to the power of Y')
  group = parse.add_mutually_exclusive_group();
  group.add_argument("-v", "--verbose", action="store_true")
  group.add_argument("-q", "--quiet", action="store_true")
  #group 添加互斥项，相当于usage: argparse_study.py [-h] [-v | -q] x y 中的[-v | -q] 只能选取一个
  parse.add_argument("x", type=int, help="the base")
  parse.add_argument("y", type=int, help="the exponent")
  args = parse.parse_args()
  answer = args.x**args.y

  if args.quiet:
      print(answer)
  elif args.verbose:
      print("{} to the power {} equals {}".format(args.x, args.y, answer))
  else:
      print("{}^{} == {}".format(args.x, args.y, answer))

  ```
### 命令行输入显示
  ```linux
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py -h
  usage: argparse_study.py [-h] [-v | -q] x y

  calculate X to the power of Y

  positional arguments:
    x              the base
    y              the exponent

  optional arguments:
    -h, --help     show this help message and exit
    -v, --verbose
    -q, --quiet
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 2
  4^2 == 16
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 2 -v
  4 to the power 2 equals 16
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 2 -q
  16
  spiderbao@spiderbao-CW65S:~/python/study/code$ python3 argparse_study.py 4 2 -vq
  usage: argparse_study.py [-h] [-v | -q] x y
  argparse_study.py: error: argument -q/--quiet: not allowed with argument -v/--verbose

  ```
  以上内容参考https://docs.python.org/3/howto/argparse.html?highlight=argparse