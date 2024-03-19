# Git 基础

我们首先理解了Git中最重要的理论知识，知道了Git的目的以及基本的文件状态，现在我们需要使用啦。

[TOC]



## 获取文件仓库

最初始的，我们需要一个Git项目，有时候我们需要自己创建一个空项目，有时候我们需要把一个已有的项目作为Git项目，有时候我们想从远端拉取一个Git项目，这些都是可以用Git命令实现的。

### 在现有目录初始化仓库

对于一个非Git项目，我们想用Git来管理，很简单，运行以下命令就可以了：

> git init

该命令将创建一个名为 `.git` 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。

但是呢，我们这个时候只是做了一个初始化的操作，其实仓库里啥都没有，如果你在这个时候打开.git/objects 你不会发现任何校验和文件，因为就没有快照。

当前项目中的文件没有被跟踪（暂存区域没有该文件，仓库也没有），此时运行 `git status`

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        "Concurrency-\347\272\277\347\250\213\345\244\204\347\220\206\345\231\250\346\216\245\345\217\243\345\237\272\347\241\200\346\212\275\350\261\241.txt"
        "DB-\344\272\213\345\212\241\347\232\204\351\232\224\347\246\273\346\226\271\345\274\217\344\273\245\345\217\212\344\275\234\347\224\250.txt"
        "DB-\346\225\260\346\215\256\345\272\223\347\232\204\346\226\207\344\273\266\347\251\272\351\227\264\347\256\241\347\220\206.txt"
        "DB-\346\225\260\346\215\256\345\272\223\347\232\204\346\227\245\345\277\227\346\226\207\344\273\266.txt"
        "DB-\346\225\260\346\215\256\345\272\223\347\232\204\347\211\251\347\220\206\345\255\230\345\202\250\347\273\223\346\236\204.txt"
        "DB-\346\225\260\346\215\256\345\272\223\347\274\223\345\255\230\346\261\240.txt"
        "DB-\346\225\260\346\215\256\347\274\223\345\206\262\346\261\240\347\232\204\351\242\204\350\257\273.txt"
        JS-Ajax.txt
        git/
        github.md
        imgs/
        ruoyi/
        servlet.md
        spring/
        tomcat/
        vue/
        "\346\227\245\345\277\227/"

nothing added to commit but untracked files present (use "git add" to track)
```

我们发现，没有提交过文件，所有项目文件都是`Untracked files`未跟踪状态。

好吧~那我们将他们跟踪这些文件然后提交吧

> git add . 
>
> git commit -m 'initial repository'

这时，仓库中有且唯一的一个**当前最新的快照**

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
nothing to commit, working tree clean
```

可以看到，当前没啥可提交的，工作目录是未修改的。但是如果我们更改一个提交过的文件，比如这篇markdown文件。

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"

no changes added to commit (use "git add" and/or "git commit -a")
```

我们能看到当前文件已经被跟踪，而且显示被修改但是没有移入暂存区。之后我们可以提交这篇修改过的文档，然后作为最新版本。



### 克隆现有仓库

我们不可能每次都从零开始构建我们的项目，当我们看到一个框架模板，并希望从这个模板开发。那我们就需要获取这个项目

> git clone  \<url\>

如我们先前说的，Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。当你执行 `git clone` 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。

通过url指向需要克隆的git仓库，目前的话url支持`https://` 协议、`git://` 协议和 SSH 传输协议。



## 记录每次更新到仓库

首先需要介绍工作目录中的文件的两种状态：

- 已跟踪：被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。
- 未跟踪：未被纳入版本控制的文件，他们既不存在于仓库快照，也不存在于暂存区

![](image/lifecycle.png)



如果你创建了一个新的文件，那这个文件就是未跟踪的。要想让这个文件被跟踪，需要将它放入暂存区，然后提交到最新快照中。

### 检查文件状态

我们可以用 `git status`查看当前工作目录的文件的状态

比如说我们对这篇文档执行了修改操作，还向`/image`目录添加了上图，那么执行结果如下

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git/image/lifecycle.png

no changes added to commit (use "git add" and/or "git commit -a")
```

其中 **on branch master** 指示当前分支为master(默认)

**Changes not staged for commit** 指的是未放入暂存区的已修改的已跟踪文件，这里指示的是本篇文档。

**Untracked files **指的是未跟踪的文件，也就是上图。

**no changes added to commit** 是提醒我们暂存区目前为空。



### 跟踪新文件

可以采用 `git add *.png` 跟踪png图片，放入暂存区，这时候再查看文件状态，已经没有未跟踪文件啦

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git/image/lifecycle.png

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
```



### 暂存已修改文件

要想提交我们的修改，通常就是将修改用 `git add` 放入暂存区

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
        new file:   git/image/lifecycle.png
```

现在我们已经把所有修改的文件放入了暂存区（实际上放在了.git/index文件中），准备提交了。

有些人可能觉得说——这个状态信息太冗余了，简短些吧

那你可以采用`git status -s`，或 `git status --short` 命令，你将得到一种更为紧凑的格式输出。在此之前我们再次保存本篇文档（指做出修改）

```
PS D:\系统默认\桌面\学习笔记> git status -s
MM "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
A  git/image/lifecycle.png
```

`MM`，第一个M表示暂存了已修改文件，第二个M表示工作目录下对文件的修改没有暂存，实际上就是快照不同嘛。

`A`，暂存添加的文件

当然也有其他标识，比如说 `??`就表示工作目录下的未跟踪文件。



### 忽略文件

不是每个文件都需要被跟踪，这样太笨了。比如说编译生成的日志文件，我们就不需要做出对它的快照。那么如何让Git不注意到日志文件呢？

嗯，我们可以在主目录下创建一个 `.gitignore`文本文件，里面的内容表示我们不希望被跟踪的文件名称（采用shell正则表达式书写）

文件 `.gitignore` 的格式规范如下：

- 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob （ shell 所使用的简化了的正则表达式）模式匹配。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

```
# 不跟踪以.log结尾的日志文件
*.log

# 不跟踪log 目录下的所有文件（文件格式未知）
log/
```

这样就能达到我们的目的啦



### 查看已暂存和未暂存的修改

这里介绍 `git diff` 指的是对于文件的修改情况的查看

- **git diff** 查看未暂存文件与已暂存文件（暂存区和最后一次提交文件）的差异
- **git diff --staged** 查看暂存区文件和最后一次提交的文件的差异



```
PS D:\系统默认\桌面\学习笔记> git diff
diff --git "a/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md" "b/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
index 87ffec5..0a209a2 100644
--- "a/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
+++ "b/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
@@ -82,21 +82,105 @@ no changes added to commit (use "git add" and/or "git commit -a")
... 下面的修改省略了
```

当前文档（未暂存）与已暂存文件的差异

**index 87ffec5..0a209a2 100644**  校验和为 87ffec5... 的普通文件（源文件） 和0a209a2... 的普通文件（新文件）

**@@ -82,21 +82,105 @@** 变化行信息，源文件的82行开始共21行，新文件的82行开始共105行，实际上就是说新文件做出了修改，添加了内容

另外提一下，这里的校验和87ffec5... 在objects目录下不存在，因为**比较对象是存储在暂存区**的。



```
PS D:\系统默认\桌面\学习笔记> git diff --staged
diff --git "a/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md" "b/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
index 3e6dfe1..87ffec5 100644
--- "a/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
+++ "b/git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
@@ -51,13 +51,38 @@ nothing added to commit but untracked files present (use "git add" to track)
... 修改内容省略

diff --git a/git/image/lifecycle.png b/git/image/lifecycle.png
new file mode 100644
index 0000000..1ef9e68
Binary files /dev/null and b/git/image/lifecycle.png differ
```

已暂存和仓库最新快照的差异。

**index 3e6dfe1..87ffec5 100644**  校验和3e6dfe1...（仓库文件）和87ffec5 ... （暂存区文件）

**new file mode 100644** 新添加文件，png图

**index 0000000..1ef9e68** 校验和0000000...表示空文件，1ef9e68... （暂存区文件）



### 提交更新

扯了这么多，最终还是得提交

`git commit`命令会打开一个文本编辑器（由`git config --global core.editor`指定），然后你可输入本次提交的**message**

```

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#	modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
#	new file:   git/image/lifecycle.png
#
# Changes not staged for commit:
#	modified:   "git/ProGit\347\254\224\350\256\260-Git\345\237\272\347\241\200.md"
#
# Untracked files:
#	.gitignore
#
```

第一行空着，让你从那里输入信息，其他行都是注释，指明当前`status`。

当然你也可以直接采用 `git commit -m "message"`直接输入你的`message`，这样更方便。

你还可以使用 `git commit -a`将工作区已跟踪文件也一并提交



### 移除文件

我们手动移除某个文件会发生什么呢？会告诉我们移除操作没有添加到暂存区中。

```
PS D:\系统默认\桌面\学习笔记> git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test

no changes added to commit (use "git add" and/or "git commit -a")
```

然后我们`git add`修改，那么之后就不会再去跟踪test文件啦。之后出现的test会被认为是新文件的（校验和很敏锐的）。



如果我不想本地工作目录删除test文件，只是想着把它设置为未跟踪。

可以使用 `git rm --cached test`



### 移动文件

可以用`git mv file1 file2`移动或者重命名文件file1。



## 查看提交历史

`git log` 这个用的太多了。我们使用`git log`会查看到历次提交版本的信息。

```
PS D:\系统默认\桌面\学习笔记> git log
commit 75e446fe250222a21689a3f050634b46b006141a (HEAD -> master)
Author: Jiam Zhang <xxx@qq.com>
Date:   Fri Dec 1 16:48:22 2023 +0800

    rm test

commit 3a549905a1d1b2810265ca39cdf2bbe5a7dfe9b6
Author: Jiam Zhang <xxx@qq.com>
Date:   Fri Dec 1 16:36:54 2023 +0800

    a
```

**commit 75e446fe250222a21689a3f050634b46b006141a (HEAD -> master)** 当前分支指向master，提交校验和给出

**Author: Jiam Zhang <xxx@qq.com>**		提交人信息
**Date:   Fri Dec 1 16:48:22 2023 +0800**  	提交具体日期

**rm test** 	提交信息



常用选项

| 选项              | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| `-p`              | 按补丁格式显示每个更新之间的差异。                           |
| `--stat`          | 显示每次更新的文件修改统计信息。                             |
| `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                 |
| `--name-only`     | 仅在提交信息后显示已修改的文件清单。                         |
| `--name-status`   | 显示新增、修改、删除的文件清单。                             |
| `--abbrev-commit` | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。            |
| `--relative-date` | 使用较短的相对时间显示（比如，“2 weeks ago”）。              |
| `--graph`         | 显示 ASCII 图形表示的分支合并历史。                          |
| `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。 |
| --------------------- | ---------------------------------- |
| `-(n)`                | 仅显示最近的 n 条提交              |
| `--since`, `--after`  | 仅显示指定时间之后的提交。         |
| `--until`, `--before` | 仅显示指定时间之前的提交。         |
| `--author`            | 仅显示指定作者相关的提交。         |
| `--committer`         | 仅显示指定提交者相关的提交。       |
| `--grep`              | 仅显示含指定关键字的提交           |
| `-S`                  | 仅显示添加或移除了某个关键字的提交 |

format 格式参考 https://www.progit.cn/#pretty_format



## 撤销操作

做了很多工作，难免有一部分工作是我们会反悔的，这时候就会需要撤销操作。

### 补加遗漏操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。可以运行带有 `--amend` 选项的提交命令尝试重新提交：

>  git commit --amend

这个命令是将当前暂存区的内容提交，但是不会添加新的快照，而是替换当前分支的顶端。简单说就是

​	**创建一个新的提交，覆盖了之前的提交。**

如果暂存区为空，即修改commit的message后提交，那么实际上仓库的的 tree 校验和不会变化，变化的只是commit object的校验和。

```
PS D:\git_forks\hello> git log --pretty=oneline -1
7705048a00a1ebe0edeac1200a1f34204e93ecf0 (HEAD -> main) rename file at 22:00

PS D:\git_forks\hello> git cat-file -p d17e0c513a4805f0921f8fb7c60849f4a1573daf // 被替换掉的 commit 校验和
tree cdc9bcb17bec4fedf117a0cec4c0dc6566030f48
parent c2747b29ad4df16cdab8a56fd498aa2e2091f92f
author Jiam Zhang <2386484961@qq.com> 1701328395 +0800
committer Jiam Zhang <2386484961@qq.com> 1701328395 +0800

rename file

PS D:\git_forks\hello> git cat-file -p 7705048a00a1ebe0edeac1200a1f34204e93ecf0 // 当前顶端 commit 校验和
tree cdc9bcb17bec4fedf117a0cec4c0dc6566030f48
parent c2747b29ad4df16cdab8a56fd498aa2e2091f92f
author Jiam Zhang <2386484961@qq.com> 1701328395 +0800
committer Jiam Zhang <2386484961@qq.com> 1701439233 +0800

rename file at 22:00
```



当然如果暂存区中有修改的文件，那么和git commit 区别只在于更改了commit 校验和

```
PS D:\git_forks\hello> git log --pretty=oneline -1
5984787d28eff2352a9ce15a13ac1bd1d37812b8 (HEAD -> main) amend at 22:10

PS D:\git_forks\hello> git cat-file -p 5984787d28eff2352a9ce15a13ac1bd1d37812b8
tree 2bc43bf9fb30b26cddff94fd04219f13c5d8b916
parent c2747b29ad4df16cdab8a56fd498aa2e2091f92f
author Jiam Zhang <2386484961@qq.com> 1701328395 +0800
committer Jiam Zhang <2386484961@qq.com> 1701439826 +0800
```



### 取消暂存的文件

如果一个modified file 不想提交，那就从暂存区stage撤销掉

> git reset HEAD <file>...

`git reset HEAD <file>...` 的命令用于将指定文件从暂存区（Index）中移除，但保留在工作目录中的修改。

- `git reset HEAD <file>...` 中的 `<file>` 是你想要从暂存区中移除的文件列表。
- `<file>...` 表示你可以指定一个或多个文件，用空格分隔。

或者使用

> git restore --staged <file>...

这也是**从暂存区将指定文件移除，且保留工作目录中的修改**



### 撤消对文件的修改

> git checkout -- <file>...

> git restore <file>...

**从工作目录将对指定文件的修改移除**，恢复到最后一次提交的状态。



## 远程仓库的使用































