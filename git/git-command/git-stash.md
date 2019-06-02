

## [git help stash](https://git-scm.com/docs/git-stash)

[7.3 Git Tools - Stashing and Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

执行 `git help stash` 查看 stash 命令帮助：

```
GIT-STASH(1)                                Git Manual                                GIT-STASH(1)

NAME
       git-stash - Stash the changes in a dirty working directory away

SYNOPSIS
       git stash list [<options>]
       git stash show [<stash>]
       git stash drop [-q|--quiet] [<stash>]
       git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
       git stash branch <branchname> [<stash>]
       git stash [push [-p|--patch] [-k|--[no-]keep-index] [-q|--quiet]
                    [-u|--include-untracked] [-a|--all] [-m|--message <message>]
                    [--] [<pathspec>...]]
       git stash clear
       git stash create [<message>]
       git stash store [-m|--message <message>] [-q|--quiet] <commit>
```

## [git-stash commands](https://www.oschina.net/translate/useful-tricks-you-might-not-know-about-git-stash)

1. `git stash (push)`: **Save** your local modifications to a new stash entry and roll them back to HEAD  
2. `git stash list`: **List** the stash entries that you currently have.  
3. `git stash show`: **Show** the *changes* recorded in the stash entry as a diff between the stashed contents and the commit back when the stash entry was first created.  

    > 默认相当于 `--stat` 只显示 diff 概要，可以指定 `-p` 输出详细差异内容。

4. `git stash drop`: **Remove** a single stash entry from the list of stash entries.  
5. `git stash apply`: Like pop, but do **not** remove the state from the stash list.  
6. `git stash pop`: apply+drop, **Remove** a single stashed state from the stash list and **apply** it on top of the current working tree state.  
7. `git stash clear`: **Remove** *all* the stash entries.  

**说明**：

`git stash` 默认是 push 操作，可以指定 `-u | --include-untracked`。
`git stash save` 已经 deprecated，使用 `git stash push -m` 替代。

### stash index

引用 stash 版本号格式：`stash@{<revision>}`

- `stash@{0}` is the most recently created stash,  
- `stash@{1}` is the one before it,  
- `stash@{2.hours.ago}` is also possible.  

Stashes may also be referenced by specifying just the stash index
(e.g. the integer `n` is equivalent to `stash@{n}`).

- `git stash show 1`：显示索引为1的 stash 改动信息；  
- `git stash drop stash@{0}`: 移除最近一个 stash，也可忽略 `stash@{0}`；  

### branch with stash

`git stash branch <branchname> [<stash>]`  

Creates and checks out a new branch named `<branchname>` starting from the commit at which the `<stash>` was originally created, applies the changes recorded in `<stash>` to the new working tree and index.

具体应用场景分析，参考《[git stash 详解](https://blog.csdn.net/stone_yw/article/details/80795669)》和《[git-stash 用法小结](https://www.cnblogs.com/tocy/p/git-stash-reference.html)》。

## git stash 用法详解

[git stash 用法](https://www.cnblogs.com/yanghaizhou/p/5269899.html)  
[git stash 作用及理解](https://www.jianshu.com/p/14afc9916dcb)  
[git stash 详解](https://blog.csdn.net/stone_yw/article/details/80795669)  
[git-stash 用法小结](https://www.cnblogs.com/tocy/p/git-stash-reference.html)  

## [使用 git stash 解决 git pull 时的冲突](https://wenku.baidu.com/view/6a25f40653d380eb6294dd88d0d233d4b14e3f35.html###)

git stash 可用来暂存当前正在进行的工作，比如想 pull 最新代码，又不想加新 commit；
或者另外一种情况，为了 fix 一个紧急的 bug，先 `stash` 使返回到自己上一个 commit，改完 bug 之后再 `stash pop`，继续原来的工作。

[使用 git stash 让突如其来的分支切换更加美好](https://blog.csdn.net/qq_32452623/article/details/76100140)  

**基础流程**：

```
$git stash
$do some work
$git stash pop
```

在使用 `git pull` 代码时，经常会碰到有冲突的情况，提示如下信息：

```
error: Your local changes to 'c/environ.c' would be overwritten by merge.
Aborting.Please, commit your changes or stash them before you can merge.
```

意思是说更新下来的内容和本地修改的内容有冲突，先提交你的改变或者先将本地修改暂时存储起来。
处理的方式非常简单，使用 `git stash` 命令进行处理，分成以下几个步骤进行处理。

1. 执行 **`git stash`** 将本地修改暂时存储起来；执行 `git stash list` 可以看到保存的信息，其中 `stash@{0}` 就是刚才保存的标记。  
2. 暂存了本地修改之后，就可以执行 **`git pull`** 了。   
3. 还原暂存的内容 `git stash pop stash@{0}`  
