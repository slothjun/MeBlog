---
title: Anaconda 工具
---

## Python

### version 环境切换

#### 列出当前Anaconda python拥有的环境列表

```linux
(python36) spiderbao@spiderbao-CW65S:~$ conda info -e
# conda environments:
#
base                     /home/spiderbao/anaconda3
python36              *  /home/spiderbao/anaconda3/envs/python36
```
其中base 为安装Anaconda 所默认安装的

#### 查看当前Python的版本

```linux
(python36) spiderbao@spiderbao-CW65S:~$ python -V
Python 3.6.9 :: Anaconda, Inc.
```

#### 安装新的Python 版本

```linux
conda create --name python36 python=3.6
```
其中 python36 为别名

新版本安装完后一些常用的库和包需要手动安装

#### 切换Python 版本

```linux
conda activate python36
```
其中Python36 为别名

#### 切换会之前的环境

```linux
conda deactivate
```
#### 删除指定的版本环境

```linux
conda remove --name python34 --all
```
其中python34 为别名

#### 手动安装常用的库和包

```linux
conda install numpy

conda install scipy

conda install matplotlib
```