---
title: git
---
## 为什么使用git

这个问题的答案就在于，集中式版本控制和分布式版本控制的的区别。

那么怎样算得上集中式版本控制呢？

比如说SVN就是集中式版本控制系统，版本库是集中放在中央服务器的,而干活的时候,用的都是自己的电脑,所以首先要从中央服务器哪里得到最新的版本,然后干活,干完后,需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作,如果在局域网还可以,带宽够大,速度够快,如果在互联网下,如果网速慢的话,就郁闷了。

![svn-active-stream](http://39.104.171.29/image/svn-1.png)

集中式版本控制存在如下缺陷： 依赖网络性强，由于只使用一个中央服务器，所以会有单点问题。

以上说了集中式版本控制，那么分布式版本控制工具又是怎么样的呢？

分布式版本控制的代表就是git

Git 是分布式版本控制系统,每个人的电脑就是一个完整的版本库,这样,工作的时候就不需要联网了,因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库,那多个人如何协作呢?比如说自己在电脑上改了文件A,其他人也在电脑上改了文件A,这时,如果是自己先push上传，别想要push时远程库就会阻止并提示他更新最新的远程版本库中的内容后再push。下图就是分布式版本控制工具管理方式:

![git-active-stream](http://39.104.171.29/image/git-1.png)

分布式版本控制：解决了集中式版本控制的单点问题，和网络依赖性问题，适用于互联网多地区协同开发，但是在只是在局域网下开发，个人认为还是使用svn会更方便，git虽然性能优越但是使用上没有SVN简便。

##  <span id = "jump-storageMode"> git 和svn等系统的存储差异 </span>
从概念上来说，其他大部分系统以文件变更列表的方式存储信息。这类系统将它们存储的信息看作是一组基本文件和每个文件随时间逐步积累的差异。（被称作基于差异的版本控制）

如下图：
![checking-over-time](git/checking-over-time.png)

Figure . 存储每个文件与初始版本的差异.

git对待数据的方式与上图不同，而git是把每个文件看做一个小的文件系统然后为其生成快照，从而管理一系列快照。在git中，每当你提交更新或保存项目的状态时，它基本上对待当前项目的文件创建一份快照，并保存这个快照指向的索引。为了效率，如果文件没有修改，git不再更新存储该文件，而是只保留一个链接指向之前存储的文件。git对待数据更像是一个快照流

![checking-over-time-git](git/checking-over-time-git.png)

Figure.存储项目随时间改变的快照.



## git简介

同生活中的许多伟大事件一样,Git 诞生于一个极富纷争大举创新的年代。Linux 内核
开源项目有着为数众广的参与者。绝大多数的 Linux 内核维护工作都花在了提交补丁和保
存归档的繁琐事务上(1991-2002 年间)。到 2002 年,整个项目组开始启用分布式版本
控制系统 BitKeeper 来管理和维护代码。
到 2005 年的时候,开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结
束,他们收回了免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区(特别是 Linux 的
缔造者 Linus Torvalds )不得不吸取教训,只有开发一套属于自己的版本控制系统才不至于
重蹈覆辙。他们对新的系统订了若干目标:
• 速度
• 简单的设计
• 对非线性开发模式的强力支持(允许上千个并行开发的分支)
• 完全分布式
• 有能力高效管理类似 Linux 内核一样的超大规模项目(速度和数据量)

### 工作流程

一般工作流程如下:
1.从远程仓库中克隆 Git 资源作为本地仓库。
2.从本地仓库中 checkout 代码然后进行代码修改
3.在提交前先将代码提交到暂存区。
4.提交修改。提交到本地仓库。本地仓库中保存修改的各个历史版本。
5.在修改完成后,需要和团队成员共享代码时,可以将代码 push 到远程仓库。
下图展示了 Git 的工作流程:

![git-active-stream](http://39.104.171.29/image/git-2.png)

### git 的工作目录介绍
![git-active-stream](http://39.104.171.29/image/git-3.png)

其中 ./git 文件夹下就是git的本地仓库

## git的使用

### 创建仓库
```linux
git init 要初始的文件夹路径
```

### 添加文件到暂存区

```Linux
git add 要加入的文件
```

### 将暂存区文件添加到本地仓库，并将本地仓库推送到远程仓库中

将缓存区域文件添加到本地仓库
```Linux
git commit
```
将本地仓库推送到远程仓库中
```Linux
spiderbao@spiderbao-CW65S:~/git_study$ git push ssh://git@39.xxx.171.29/home/git/git_study
fatal: 当前分支 master 没有对应的上游分支。
为推送当前分支并建立与远程上游的跟踪，使用

    git push --set-upstream ssh://git@39.XXX.171.29/home/git/git_study master

spiderbao@spiderbao-CW65S:~/git_study$ git push --set-upstream ssh://git@39.XXX.171.29/home/git/git_study master
git@39.XXX.171.29's password: 
Permission denied, please try again.
git@39.XXX.171.29's password: 
对象计数中: 6, 完成.
Delta compression using up to 4 threads.
压缩对象中: 100% (3/3), 完成.
写入对象中: 100% (6/6), 523 bytes | 523.00 KiB/s, 完成.
Total 6 (delta 0), reused 0 (delta 0)
To ssh://39.XXX.171.29/home/git/git_study
 * [new branch]      master -> master
分支 'master' 设置为跟踪来自 'ssh://git@39.XXX.171.29/home/git/git_study' 的远程分支 'master'。

```

### 查看修改历史
```linux
spiderbao@spiderbao-CW65S:~/git_study$ git log
commit 4cb2b4f87b03b6cf3301b000a3e14c44027e3f42 (HEAD -> master)
Author: spiderbao <13593265401@163.com>
Date:   Fri May 17 14:28:44 2019 +0800

    第二次修改

commit 1910c01353a8f12304e2a923164f69e33925a03a
Author: spiderbao <13593265401@163.com>
Date:   Fri May 17 14:21:19 2019 +0800

    first commit

```

### 还原修改

reset 还原，还原后历史版本不存在
```linux
spiderbao@spiderbao-CW65S:~/git_study$ git reset --hard  1910c01353a8f12304e2a923164f69e33925a03a
HEAD 现在位于 1910c01 first commit

```

### 删除文件

```Linux
spiderbao@spiderbao-CW65S:~/git_study$ git rm test 
rm 'test'
```

### 忽略文件或文件夹

在文件目录下创建gitignore 文件
```Linux
touch .gitignore
vim .gitignore
```
忽略语法

```
空行或是以 # 开头的行即注释行将被忽略。
可以在前面添加正斜杠 / 来避免递归,下面的例子中可以很明白的看出来与下一条的区别。
可以在后面添加正斜杠 / 来忽略文件夹,例如 build/ 即忽略 build 文件夹。
可以使用 ! 来否定忽略,即比如在前面用了 *.apk ,然后使用 !a.apk ,则这个 a.apk 不会
被忽略。
* 用来匹配零个或多个字符,如 *.[oa] 忽略所有以".o"或".a"结尾, *~ 忽略所有以 ~ 结尾
的文件(这种文件通常被许多编辑器标记为临时文件); [] 用来匹配括号内的任一字符,
如 [abc] ,也可以在括号内加连接符,如 [0-9] 匹配 0 至 9 的数; ? 用来匹配单个字符。
看了这么多,还是应该来个栗子:
# 忽略 .a 文件
*.a
# 但否定忽略 lib.a, 尽管已经在前面忽略了 .a 文件
!lib.a
# 仅在当前目录下忽略 TODO 文件, 但不包括子目录下的 subdir/TODO
/TODO
# 忽略 build/ 文件夹下的所有文件
build/
# 忽略 doc/notes.txt, 不包括 doc/server/arch.txt
doc/*.txt
# 忽略所有的 .pdf 文件 在 doc/ directory 下的
doc/**/*.pdf
```
### 解决文件冲突

编辑有冲突后的文件，再次提交即可



### 文件更新或拉取

```linux
spiderbao@spiderbao-CW65S:~/cloneToGit$ git clone ssh://git@39.xxx.xxx.29/home/git/git_study
正克隆到 'git_study'...
git@39.xxx.xxx.29's password: 
remote: Counting objects: 13, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 13 (delta 0), reused 0 (delta 0)
接收对象中: 100% (13/13), 完成.

spiderbao@spiderbao-CW65S:~/cloneToGit$ cd git_study/
spiderbao@spiderbao-CW65S:~/cloneToGit/git_study$ git pull
git@39.xxx.xxx.29's password: 
已经是最新的。

```

## git分支管理

为了真正理解 Git 处理分支的方式，我们需要回顾一下 Git 是如何[保存数据](#jump-storageMode)的。

Git 保存的不是文件的变化或者差异，而是一系列不同时刻的 **快照** 

在进行提交操作时，git会保存一个提交对象（commit object）。该提交对象会包含一个指向暂存快照的指针。但不仅仅是这样，该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。 首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象， 而由多个分支合并产生的提交对象有多个父对象

案例：

我们假设现在有一个工作目录，里面包含了三个将要被暂存和提交的文件。 暂存操作会为每一个文件计算校验和（使用我们在 [起步](https://git-scm.com/book/zh/v2/ch00/ch01-getting-started) 中提到的 SHA-1 哈希算法），然后会把当前版本的文件快照保存到 Git 仓库中 （Git 使用 *blob* 对象来保存它们），最终将校验和加入到暂存区域等待提交

```Linux
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
```

当使用 `git commit` 进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和， 然后在 Git 仓库中这些校验和保存为树对象。随后，Git 便会创建一个提交对象， 它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。 如此一来，Git 就可以在需要的时候重现此次保存的快照。

现在，Git 仓库中有五个对象：三个 *blob* 对象（保存着文件快照）、一个 **树** 对象 （记录着目录结构和 blob 对象索引）以及一个 **提交** 对象（包含着指向前述树对象的指针和所有提交信息）。

![branch-1](git/git-branch-1.png)

Figure 首次提交对象及其树结构

做些修改后再次提交，那么这次产生的提交对象会包含一个指向上次提交对象（父对象）的指针。

![branch-1](git/git-branch-2.png)

Figure 提交对象及其父对象

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 `master`。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 `master` 分支。 `master` 分支会在每次提交时自动向前移动。

** <span style="color:red">Note</span>**:   Git 的 `master` 分支并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 `git init` 命令默认创建它，并且大多数人都懒得去改动它。

![branch-3](git/git-branch-3.png)

### 创建分支

```linux
git branch 新分支名称
```
### 选择分支

```Linux
git checkout 分支名称
```

### 合并分支

```linux
spiderbao@spiderbao-CW65S:~/git_study$ git merge mybatis 
更新 8c65eb0..458de41
Fast-forward
 xiaoli | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 xiaoli
```

## git 贮藏(stash)

个人认为此功能用来间当前缓存区中的信息暂时贮藏，便于切换别的分支去处理事情。等处理完后再切换到当前分支并恢复贮藏信息并继续当前分支的开发。

使用场景：

由于分支的切换需要干净的暂存区（stage，也是就是这个区域内不能有信息）。所以导致想要切换版本就必须面临提交当前工作区信息到版本库中或者不提交。而这两者都具有弊端，前者能保存当前工作区中的内容，但是会对提交历史产生影响。后者虽然不会对仓库的历史产生影响，但是当前工作区中的信息也不会保存。使用git的贮藏功能可以兼备以上所期望的2点。实现过程如下案例

场景：

当前项目的分支情况如下：

```Linux
* commit 394e7afc1077df0bf4d399c8d6d1e6821eb7f49d (HEAD -> experiment)
| Author: spiderbao <13593265401@163.com>
| Date:   Thu Jun 4 14:29:01 2020 +0800
| 
|     test
|     
|     modified test
| 
* commit 49fd47ed235a96ebe7a353c016aa4602c3b8dfb5
| Author: spiderbao <13593265401@163.com>
| Date:   Wed Jun 3 10:05:10 2020 +0800
| 
|     add experiment
| 
* commit 214cbe6f166becc20309a70f35bbc43ff60ec7eb (master)
| Author: spiderbao <13593265401@163.com>
| Date:   Wed Jun 3 10:06:25 2020 +0800
| 
|     add test
| 
* commit 252d42a731d129478ecfc2882d691ab3ee6e5deb
  Author: spiderbao <13593265401@163.com>
  Date:   Wed Jun 3 10:02:57 2020 +0800
  
      add README.md

```

当用户在experiment开发时，接到了临时的紧急通知需要修改master分支上的几处BUG。

1.使用git status 查看当前项目的情况，将要保存的信息放入暂存区（stage）处

```Linux
位于分支 experiment
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	新文件：   index.html
	新文件：   lib/simplegit.rb

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	TODO

```

```Linux
$  git add TODO
```

```Linux
位于分支 experiment
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	新文件：   TODO
	新文件：   index.html
	新文件：   lib/simplegit.rb
```



2. 将暂存区中的内容通过stash 放入特定的一个栈中

   ```Linux
   $ git stash 
   保存工作目录和索引状态 WIP on experiment: 394e7af test
   ```

   

3. 切换分支工作，解决BUG并提交

   ```
   $ git checkout master
   切换到分支 'master'
   ......
   ```

   

4. 切换分支并恢复贮藏信息

   ```Linux
   $ git checkout experiment 
   切换到分支 'experiment'
   $ git stash apply stash@{0} 
   位于分支 experiment
   要提交的变更：
     （使用 "git reset HEAD <文件>..." 以取消暂存）
   
   	新文件：   TODO
   	新文件：   index.html
   	新文件：   lib/simplegit.rb
   
   ```

   

5. 清理贮藏区中的信息

   ```Linux
   $ git stash list
   stash@{0}: WIP on experiment: 394e7af test
   $ git stash drop stash@{0} 
   丢弃了 stash@{0} (b1cb57a6cb76585027a27eed6779bcab6ca4f400)
   
   ```

   