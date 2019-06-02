
## [git help log](https://git-scm.com/docs/git-log)

```
GIT-LOG(1)                                  Git Manual                                  GIT-LOG(1)



NAME
       git-log - Show commit logs

SYNOPSIS
       git log [<options>] [<revision range>] [[--] <path>...]


DESCRIPTION
       Shows the commit logs.

       The command takes options applicable to the git rev-list command to control what is shown
       and how, and options applicable to the git diff-* commands to control how the changes each
       commit introduces are shown.
```

## [git log 详细使用参数](https://blog.csdn.net/helloxiaozhe/article/details/80563427)

选项 | 说明
----|---------
`--graph` | 以 ASCII 字符图形显示分支提交合并历史。
`--abbrev-commit` | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符
`--relative-date` | 使用较短的相对时间显示，例如 “2 weeks ago”

### limit

选项 | 说明
----|---------
**`-(n)`** | 仅显示最近的 n 条提交: git log -n 2 或 git log -2  
`--skip` | 指定跳过前几条日志
`--since`, `--after` | 仅显示指定时间之后的提交  
`--until`, `--before` | 仅显示指定时间之前的提交  

### message

选项 | 说明
----|---------
`--name-only` | 仅显示修改的文件清单（不包含相对路径）
`--name-status` | 显示修改文件清单，行首以 M、D 等标记变更状态
`--shortstat` | 仅显示 --stat 最后的增删改概要信息
**`--stat`** | 显示修改的文件列表（相对路径）及增删改统计信息
`-p`  | 按补丁格式显示每个文件之间的差异（diff）

> `git log -p` 相当于多次调用 `git show [commit_hashid]`。

#### pretty

`git log --pretty`：使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。

#### decorate graph

[git查看各个branch之间的关系图](https://www.jianshu.com/p/2619830b6e3f)

```
git log --graph --decorate --oneline --simplify-by-decoration --all
```

**说明**：

`--decorate`：标记会让 git log 显示每个 commit 的引用(如 分支、tag 等)   
`--oneline`：一行显示  
`--simplify-by-decoration`：只显示被branch或tag引用的commit  
`--all`：表示显示所有的branch。这里也可以选择，比如指向显示分支 ABC 的关系，则将 --all 替换为 branchA branchB branchC 即可。

### search

选项 | 说明
----|---------
`--author` | 仅显示指定作者相关的提交  
`--committer` | 仅显示指定提交者相关的提交  
`--grep` | 仅显示含指定关键字的提交  
`-S` | 仅显示添加或移除了某个关键字的提交  

## 查看某个文件的日志

`git log [filename]`

```
faner@MBP-FAN ~/Projects/github/libNET/mars
0 master % git log mars/comm/messagequeue/message_queue.cc
```

## 查看某次提交的日志

git log 指定某次 commit 的 HASH 值，即可查看某次提交的日志：

```
faner@MBP-FAN ~/Projects/github/libNET/mars
0 master % git log c6938ef4c3b308c6aa423938678599b200100451
```

默认显示自某次提交之前的日志，添加 `-n 1` 选项限定只查看某一次的提交。

## 查看指定仓库的提交记录

`git log` 默认查看的是本地当前仓库的日志。

### 查看本地仓库的提交记录

查看 `git branch`(--list) 本地仓库中指定仓库的日志。

```
git log feature/8.0.8_PCSendMobileAlbum --author="fan2" --stat
```

### 查看远端仓库的提交记录

查看 `git branch -r` 远端仓库（origin）中当前仓库的日志，以便对比本地尚未同步的差异。

```
iq git/feature/8.0.8_PCSendMobileAlbum2
❯ git log origin/feature/8.0.8_PCSendMobileAlbum --author="fan2" --stat
```

指定其他仓库，查询相应 commit hash，以便执行 `git cherry-pick` 将某些提交合入当前分支。
