---
title: Ubuntu add-apt-repository
---

## 简介：

### 来自于Ubuntu自带文档上的介绍：

```text
 add-apt-repository is a script which adds an external APT repository to
       either  /etc/apt/sources.list  or a file in /etc/apt/sources.list.d/ or
       removes an already existing repository.

       The options supported by add-apt-repository are:

       -h, --help Show help message and exit

       -m, --massive-debug Print a lot of debug  information  to  the  command
       line

       -r, --remove Remove the specified repository

       -y, --yes Assume yes to all queries

       -u, --update After adding the repository, update the package cache with
       packages from this repository (avoids need to apt-get update)

       -k, --keyserver Use a custom keyserver URL instead of the default

       -s, --enable-source Allow downloading of the source packages  from  the
       repository

```
### 来自百度百科的介绍：

```text
add-apt-repository的提供方python-software-properties ，平台是Ubuntu Karmic (9.10)。
在 Ubuntu Karmic (9.10) 我们可以使用 "add-apt-repository" 脚本添加 ppa 到当前的库中并且自动导入公钥.
```