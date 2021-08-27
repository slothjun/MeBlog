---
title: ubuntu 下的软件管理
---

## 概念部分

### Ubuntu软件包管理概述

	Ubuntu Linux采用了Debian的软件包管理机制。由于软件包具有易用性、灵活性和扩展性的特点，再加上Internet的支持，使用户随时都能拥有最新的 Ubuntu系统，这也是Ubuntu受到推崇的一个重要原因。因而，Deb软件包管理也成为Ubuntu中最有活力的部分。本章介绍Ubuntu软件包 管理和dpkg软件包管理器。

### Deb软件包概述

	Deb软件包本质上是文件包，这点类似于tar文件将多个文件合并为一个归档文件。但是Deb的身价不在于整合文件，而在于使应用程序更易于传播。

	当年流行的两种软件包管理机制

	最初，基于Linux系统的开发者在完成应用程序开 发后，将很多二进制文件发给用户，用户使用之前需要将相关程序逐个安装。因此，Debian Linux首先提出“软件包”的管理机制——Deb软件包，将应用程序的二进制文件、配置文档、man/info帮助页面等文件合并打包在一个文件中，用 户使用软件包管理器直接操作软件包，完成获取、安装、卸载、查询等操作。

	随即，Redhat Linux基于这个理念推出了自己的软件包管理机制——Rpm软件包。当然，Redhat Linux采用了自己的打包格式生成Rpm包文件，由Rpm包管理器负责安装、维护、查询，甚至软件包版本管理。由于Redhat Linux系统的普及，Rpm软件包被广泛使用，甚至出现第三方开发的软件管理工具，专门管理Rpm格式的软件包。

	随着Linux操作系统规模的不断扩大，系统中软件 包之间复杂的依赖关系，导致Linux用户麻烦不断。为了解决这个问题，Debian Linux开发出了APT软件包管理器。它能够自动检查和修复软件包之间的依赖关系。并且，利用Internet网络带来的快捷的连通手段，APT工具可 以帮助用户主动获取软件包。因此，APT工具再次促进了Deb软件包更为广泛地使用，成为Debian Linux的一个无法替代的亮点。
	
	Ubuntu Linux系统的软件包管理机制延续了Debian的包管理方法。

### 软件包的类型
	Debian包文件包含了二进制可执行文件、库文件、配置文件和man/info帮助页面等文档。通常Debian包文件的后缀为.deb，因此称为“Deb软件包”。Ubuntu有两种类型的软件包：二进制软件包（deb）和源码包（deb-src）。

	  ●    二进制软件包（Binary Packages）：包含可执行文件、库文件、配置文件、man/info页面、版权声明和其他文档。

	  ●    源码包（Source Packages）：包含软件源代码、版本修改说明、构建指令以及编译工具等。先由tar工具归档为.tar.gz文件，然后再打包成.dsc文件。

	用户不确定一个软件包具体类型时，可以使用file命令查看文件类型。例如下面命令用于证实一个软件包的文件类型是否是Deb软件包文件。

	wdl@UbuntuFisher:~$ file  g++_4.1.2-9ubuntu2_i386.deb

	g++_4.1.2-9ubuntu2_i386: Debian binary package (format 2.0)

	在Ubuntu Linux中，需要说明一个概念——虚拟软件包。将系统中具有相同或相近功能的多个软件包作为一个软件包集合，称为虚拟软件包，并指定其中一个软件包作为 虚拟软件包的默认首选项。提出虚拟软件的意图就是为了防止软件安装过程中发生冲突。例如，exim、sendmail和postfix软件包都是用于邮件 传输代理，将“mail-transport-agent”指定为它们的虚拟软件包。当用户安装“mail-transport-agent”时，将选择 安装exim、sendmail和postfix其中的首选项。

### 软件包的命名

	在Ubuntu Linux中，软件包的命名遵循以下约定：

	Filename_Version-Reversion_Architecture.deb

	其中，Filename表示软件包文件名，Version表示软件版本号，Reversion表示修订版本号，Architecture表示适用计算机架构。通常，修订版本号是由Ubuntu开发者或创建这个软件包的人指定。在软件包被修改过之后，将修改版本号加1。

	以g++_4.1.2-9ubuntu2_i386.deb软件包为例，g++是软件包名，4.1.2是软件版本号，9ubuntu2是修订版本号，i386是适用的计算机架构。

### 配置部分

	Ubuntu 方便宜用，最值得让人称道的便是其安装软件的方式, 一条命令: sudo apt-get install xxx 就几乎能帮你搞定所有的软件安装难题。但是有时你可能有这样的需求，查看某个软件包是否安装、安装在哪..., 那我们就来介绍一下 Ubuntu 的软件包管理方式。

	一、Ubuntu 采用 Debian 的软件包管理器 dpkg 来管理软件包, 类似 RPM. 系统中所有 packages 的信息都在 /var/lib/dpkg/
	目录下, 其子目录 /var/lib/dpkg/info 用于保存各个软件包的配置文件列表:
	 (1).conffiles 记录了软件包的配置文件列表
	 (2).list 保存软件包中的文件列表, 用户可以从 .list 的信息中找到软件包中文件的具体安装位置.
	 (3).md5sums 记录了软件包的md5信息, 这个信息是用来进行包验证的.
	 (4).prerm 脚本在 Debian 包解包之前运行, 主要作用是停止作用于即将升级的软件包的服务, 直到软件包安装或升级完成.
	 (5).postinst 脚本是完成 Debian 包解开之后的配置工作, 通常用于执行所安装软件包相关命令和服务重新启动.

	/var/lib/dpkg/available 文件的内容是软件包的描述信息, 该软件包括当前系统所使用的 Debian 安装源中的所有软件包,
	其中包括当前系统中已安装的和未安装的软件包.

	/var/cache/apt/archives 目录是在用 apt-get install 安装软件时，软件包的临时存放路径

	/etc/apt/sources.list 存放的是软件源站点, 当你执行 sudo apt-get install xxx 时，Ubuntu 就去这些站点下载软件包到本地并执行安装

## 命令操作部分	

### apt和dpkg混合命令

 (1)查看某软件包的安装内容
	dpkg -L xxx

 (2)查找软件库中的软件包
    apt-cache search 正则表达式

 (3)显示系统安装包的统计信息
    apt-cache stats
 
 (4)显示系统全部可用软件包的名称
    apt-cache pkgnames

 (5)显示某软件包的详细信息
    apt-cache show xxx

 (6)查找某文件属于哪个包
    apt-file search xxx

 (7)查看已经安装了哪些软件包
    dpkg -l

 (8)查询某软件依赖哪些软件包
    apt-cache depends xxx

 (9)查询某软件被哪些软件包依赖
    apt-cache rdepends xxx

 (10)增加一个光盘源
    sudo apt-cdrom add
    注: 顾名思义, 就是安装更新软件包时让其优先从Ubuntu 光盘上找(如果你不能上网安装/更新, 但有 Ubuntu 的 DVD ISO, 这会对你非常有用)

 (11)系统升级
    sudo apt-get update

 (12)清除所有已删除软件包的残馀配置文件
    dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P

 (13)编译时缺少h文件的自动处理
    sudo auto-apt run ./configure

 (14)查看安装软件时下载软件包的临时存放目录
    ls /var/cache/apt/archives

 (15)备份当前系统安装的所有软件包的列表
    dpkg --get-selections | grep -v deinstall > ~/somefile

 (16)从上面备份的安装包的列表文件恢复所有包
    dpkg --set-selections < ~/somefile
    sudo dselect

 (17)清理旧版本的软件缓存
    sudo apt-get autoclean

 (18)清理所有软件缓存
    sudo apt-get clean

 (19)删除系统不再使用的孤立软件
    sudo apt-get autoremove

 (20)查看软件包在服务器上面的地址
    apt-get -qq --print-uris install ssh | cut -d\' -f2

 (21)查看安装软件的一些路径信息
 	dpkg -L 软件包名
 	
### deb包安装
	sudo dpkg -i  'deb文件路径加文件名'