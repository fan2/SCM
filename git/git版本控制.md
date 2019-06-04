
之前梳理介绍过[subversion版本控制](http://blog.csdn.net/phunxm/article/details/40834427)，本文承接《[Mac下git通过SSH进行免密码安全连接github](http://blog.csdn.net/phunxm/article/details/45083335)》，基于一个初级的git版本控制流程贯串示例讲解git日常操作，备忘查阅。

## git trends

![google trends of git](http://img.blog.csdn.net/20150925231127034)

![google trends of github](http://img.blog.csdn.net/20150925231200247)

## Mac/Xcode git

### whereis git

在 macOS Terminal 中执行 which 或 whereis 命令可以查看 git 的位置：

```shell
➜  bin  which git
/usr/bin/git
➜  bin  whereis git
/usr/bin/git
```

`which git` 返回的结果是 `/usr/bin/git`，同svn一样，它们是 Xcode 安装 Command Line Tools 时安装的 git wrapper，或者叫 [shims or wrapper](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools/) executables，真正的工具包**实体**在 Xcode.app 里面：

```shell
➜  ~  xcrun -f git
/Applications/Xcode.app/Contents/Developer/usr/bin/git
```

### git version

在终端输入 `git version` 或 `git --version` 查看系统自带的 git 版本比较旧，为 2.3.8：

```shell
➜  ~  git version
git version 2.3.8 (Apple Git-58)
```

执行 `ls git*` 可查看 git binUtils 组成：

```shell
➜  ~  cd /usr/bin
➜  bin  ls git*
git                git-receive-pack   git-upload-archive
git-cvsserver      git-shell          git-upload-pack
```

### upgrade git

可从 git 官网下载[git for Mac](http://git-scm.com/download/mac)下载安装包（git-2.5.3-intel-universal-mavericks.dmg），点击 `git-2.5.3-intel-universal-mavericks.pkg` 可安装 git binUtils，默认安装到 `/usr/local/git` 目录下:

```shell
➜  ~  cd /usr/local/git/bin
➜  bin git:(master) ls
git                        git-receive-pack           git-upload-pack
git-credential-osxkeychain git-shell                  gitk
git-cvsserver              git-upload-archive
```

+ 可在bash下运行 `setup git PATH for non-terminal programs.sh` 脚本设置新版 git 为非命令行程序的默认 git。
+ 可参照Mac下升级[subversion工具包升级方法](http://blog.csdn.net/phunxm/article/details/40834427)，直接软链新版binUtils的 git* 到`/Applications/Xcode.app/Contents/Developer/usr/bin`下即可。

### git manual

1. 输入 `git` 或 `git --help` 可以查看 git usage，一览 most commonly used git commands 要义：

![git --help](http://img.blog.csdn.net/20150908072930624)

2. 针对具体 commands，可以求助“男人”（man），以下示例查看 **`git add`** 帮助的两种方式：

```shell
➜  ~  git --help add（git help add）
```

或

```shell
➜  ~  man git add
```

按下`Q`键退出 manual。

## Git GUIs

[gitk - The Git repository browser](https://git-scm.com/docs/gitk)  
[Graphical Interfaces](https://git-scm.com/book/en/v2/Appendix-A:-Git-in-Other-Environments-Graphical-Interfaces)  
[**GUI Clients**](https://git-scm.com/downloads/guis)  

[Git各大平台（win／Linux／Mac）图形化界面客户端大汇总](https://www.cnblogs.com/tyxa/p/6135896.html)

### [TortoiseGit](https://tortoisegit.org/)

[github](https://github.com/TortoiseGit/TortoiseGit)  

The Power of Git – in a Windows Shell  
Windows Explorer Extension to Operate Git  

> TortoiseSVN 孪生兄弟，建议 Windows 采用。

[Documentation](https://tortoisegit.org/docs/)

### [SmartGit](https://www.syntevo.com/smartgit/)

Get your commit done.  
SmartGit runs on Windows, macOS and Linux.  
SmartGit is a graphical Git client with support for SVN and Pull Requests for GitHub and Bitbucket.   

> SmartSVN 孪生兄弟，建议 Windows/macOS 采用。

#### [Manual](http://www.syntevo.com/doc/display/SG/Manual)

[SmartGit初步使用](https://www.cnblogs.com/DebugLife/p/3772763.html)  
[SmartGit 安装及使用（一）：安装及配置](https://blog.csdn.net/guang_s/article/details/84257539)  

#### [non-commercial](https://www.syntevo.com/blog/?p=3669)  

- [Windows](https://blog.csdn.net/u014653854/article/details/86467436)  
- [macOS](https://www.jianshu.com/p/79ff2d63ddc6)  

[SmartGit一直强提示让确认“于非营利目的”解决步骤](https://blog.csdn.net/haha223545/article/details/86699820) - 注册非商业许可证  

### [Sourcetree](https://www.sourcetreeapp.com/)

A **free** Git client for Windows and Mac  
Simplicity and power in a beautiful Git GUI  

> 没法看整个仓库的文件和单个文件的历史，用命令行或 [gitk](https://blog.csdn.net/nujiah001/article/details/50676333) 辅助？

## [git config](http://blog.csdn.net/wirelessqa/article/details/8572928)

git 配置分为系统级别、用户级别和项目级别，*`--global`* 选项指定用户级别的配置。

```shell
➜  ~  git config --global user.name fan2
➜  ~  git config --global user.email xxx@qq.com
```

配置完成后，可以输入 `git config --list` 命令查看当前配置。
后面该用户的 git 提交信息中将自动嵌入用户信息，即 **git log** 查看到的每条 log 的 Author 及 Mail 等信息。
也可针对具体项目（cd到 `.git` 所在目录）执行 **git config** 命令，而无需指定 `--system` 或 `--global` 选项，配置适用于独立项目的作者信息或代理信息（http.proxy）。

---

## git init

### cd进入本地项目目录

```shell
➜  ~  cd Projects/Xcode/resizableImageWithCapInsets
```

查看当前目录：

```shell
➜  ~  pwd
/Users/faner/Projects/Xcode/resizableImageWithCapInsets
```

当前目录为空文件夹，`ls` 或 `ls -a` 列举无内容。

### 查看当前项目（目录）的git状态

使用 `git status` 命令可以查看当前工作目录的 git 版本控制状态（增删改等）。

```shell
➜ resizableImageWithCapInsets  git status
fatal: Not a git repository (or any of the parent directories): .git
```

`Not a git repository` 表明当前工程（项目）尚未纳入 git 版本控制。

### 基于当前项目（目录）创建初始化一个 git 仓库

创建初始化 git 版本库，需要使用 `git init` 命令：

```shell
➜  resizableImageWithCapInsets  git init --help

NAME
       git-init - Create an empty Git repository or reinitialize an existing one
       
DESCRIPTION
       This command creates an empty Git repository - basically a .git directory with
       subdirectories for objects, refs/heads, refs/tags, and template files. An
       initial HEAD file that references the HEAD of the master branch is also
       created.
```

对当前项目（目录）执行 `git init` 命令，即可初始化一个本地 git 仓库：  

```shell
➜  resizableImageWithCapInsets  git init  
Initialized empty Git repository in /Users/faner/Projects/Xcode/resizableImageWithCapInsets/.git/
```

此时运行 `ls -a` 列举当前目录：  

```shell
➜  resizableImageWithCapInsets git:(master) ls -a  
.    ..   .git
```

可以发现该目录下新增了一个隐藏的 `.git` 文件夹，即初始化了一个本地仓库（git repository）。
若是从 github 等远程仓库克隆（clone）下来的项目，则默认即是一个追踪了远程仓库的 git repository，无需再执行 `git init` 初始化。

此时，再次执行 **`git status`** 命令可查看初始化的本地仓库状态：

```shell
➜  resizableImageWithCapInsets git:(master) git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

由于当前 Git Repo 刚初始化，没有任何增删改操作，故没啥好提交的：`nothing to commit`。
它提示我们创建（create）或从别处拷贝（copy）文件过来，并调用 **`git add`** 命令添加追踪。

**`注意`**：以下 git 操作都是基于 *当前工作目录*，即包含 `.git` 的 **Working directory**/**Working Tree**。

## git add to Index

### 创建第一个文件——README.md

1. 执行 `touch README.md` 命令新建 README.md 文件： 

```shell
➜  resizableImageWithCapInsets git:(master) touch README.md
➜  resizableImageWithCapInsets git:(master) ✗ ls
README.md
```

2. 执行 `vim README.md` 打开 vim 进行编辑：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ vi README.md  
```

* `i`: insert模式编辑;  
* `esc`: 退出insert编辑模式;  
* `:wq`: 保存退出  

3. 编辑完成执行 `cat README.md`，可查看文件内容：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ cat README.md  
README for resizableImageWithCapInsets.  
```

4. 再次执行 **`git status`** 命令可查看当前本地仓库状态：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git status  
On branch master  

Initial commit  

Untracked files:  
  (use "git add <file>..." to include in what will be committed)  

        README.md  

nothing added to commit but untracked files present (use "git add" to track)  
```

+ `Untracked files`，即新建的README.md文件未被git追踪（Unversioned/Untracked）。  
+ `use "git add" to track`，提示使用`git add`命令将未追踪文件添加追踪（add to version control），纳入版本控制（tracked）。

### 将 README.md 文件添加到 git index

添加本地文件到git版本控制，需要使用 git add 命令：  

```shell
➜  ~  git add --help

NAME
       git-add - Add file contents to the index

DESCRIPTION
       This command updates the index using the current content found in the working
       tree, to prepare the content staged for the next commit. It typically adds the
       current content of existing paths as a whole, but with some options it can also
       be used to add content with only part of the changes made to the working tree
       files applied, or remove paths that do not exist in the working tree anymore.

       The "index" holds a snapshot of the content of the working tree, and it is this
       snapshot that is taken as the contents of the next commit. Thus after making
       any changes to the working directory, and before running the commit command,
       you must use the add command to add any new or modified files to the index.

```

It typically adds the current content of existing paths as a whole, but with some options it can also be used to add content with **only part** of the changes made to the working tree files applied, or remove paths that do not exist in the working tree anymore.  
The **"index"** holds a **snapshot** of the content of the working tree, and it is this snapshot that is taken as the contents of the next commit. <u>Thus after making **any changes** to the working directory, and before running the commit command, you must use the add command to add any <b><i>new</i></b> or <b><i>modified</i></b> files to the index.</u>

#### index 的概念

index 有时候也被称作 `Staging area` 或 `Cache`，`git add` 命令会将文件改动暂存到 index 中(.git/index)。

[![](http://i.stack.imgur.com/naws3.png)](http://stackoverflow.com/questions/3689838/difference-between-head-working-tree-index-in-git)

#### git add 基本用法

基本语法：`git add <pathspec>`

+ `<pathspec>` 可以是 `.`，表示添加当前目录下所有（**as a whole**）未追踪的文件或修改（unversioned/untracked files or modification）到git索引。
+ 也可以添加指定子目录层级的单个文件或多个文件列表（**only part**）。

**`添加README.md文件：`**

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git add README.md
```

由于当前文件只有一个 Untracked files，因此也可执行 `git add .`。  
再次执行 **`git status`** 命令可查看当前本地仓库状态：  

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

  new file:   README.md
```

提示有改动需要提交（Changes to be committed）：新增了文件（new file:   README.md）。

___

此时，若反悔了，想解除对 README.md 文件的追踪（Undo an Added new file），亦可执行 `git rm --cached`：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git rm --cached README.md
rm 'README.md'
```

则 README.md 文件又恢复到 `git add` 之前的状态——Untracked。

**`注意`**：

+ 执行 `git rm --cached README.md` 只是将 README.md 文件从暂存区（Index/Staging area）移除，文件依旧保留（keep the file）；  
+ 若执行`git rm -f README.md`，则强制物理删除（force removal）本地文件。

## git commit to local repository

将暂存区中的变更提交到到git仓库，需要使用 `git commit` 命令：

```shell
➜  ~  git commit --help

NAME
       git-commit - Record changes to the repository

DESCRIPTION
       Stores the current contents of the index in a new commit along with a log
       message from the user describing the changes.

OPTIONS

       -m <msg>, --message=<msg>
           Use the given <msg> as the commit message. If multiple -m options are
           given, their values are concatenated as separate paragraphs.
```

之前在 resizableImageWithCapInsets 这个WC（Working directory/Working tree）新增（git add）了 README.md 文件。
这个 `git add` 新增变更是记录在 index（staging area）中，下面将其添加到本地 git 仓库。

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git commit -m "This is the first commit."  
[master (root-commit) ee53e68] This is the first commit.  
  1 file changed, 1 insertion(+)  
  create mode 100644 README.md  
```

再次执行 **`git status`** 命令查看当前本地仓库状态：  

```shell
➜  resizableImageWithCapInsets git:(master) git status
On branch master
nothing to commit, working directory clean
```

`"nothing to commit, working directory clean"`表示变更已入库，当前工作目录是干净（clean）的。
执行 `git commit` 提交后，可通过 **`git log`** 命令查看提交日志：

```shell
commit ee53e68e599695f15b8e91d5eb3fd490f945a23e  
Author: fan2 <xxx@qq.com>  
Date:   Sun Sep 13 15:34:13 2015 +0800  

    This is the first commit.  
```

输入`Q`键退出log。

后续多次提交后，可通过 [`git log`](http://www.cnblogs.com/gbyukg/archive/2011/12/12/2285419.html) 的 `-n` 选项指定 Limit the number of commits to output，例如 `git log -n 2` 查看最近两次的提交log。

### git本地仓库

本地仓库由 git 维护的三棵“树”组成：  

+ 第一个是 物理工作目录（Working tree），它持有实际文件；
+ 第二个是 暂存区（Index），它像个缓存区域，临时保存你的改动；
+ 最后是 repository ，HEAD 指向你最后一次提交（commit）的结果。

[![git本地仓库](http://rogerdudler.github.io/git-guide/img/trees.png)](rogerdudler.github.io/git-guide/index.zh.html)

## git add modificaion / rm tracked file

### 修改文件

将 README.md 文件add添加到 git 版本控制并commit提交到 git 仓库后，稍微改动一下 README.md 文件，增加第二行空行，第三行增加"this is the content. "。
然后，使用 `git diff` 命令查看工作目录与索引区文件之间的差异：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git diff README.md
diff --git a/README.md b/README.md  
index 717aedf..0a1832d 100644  
--- a/README.md  
+++ b/README.md  
@@ -1 +1,3 @@  
&nbsp;README for resizableImageWithCapInsets.  
+  
+this is the content.  
```

按下`Q`键退出diff。

关于 [git diff](http://www.gitguys.com/topics/git-diff/?lang=zh) 命令，可参考 [git diff](http://www.cnblogs.com/gbyukg/archive/2011/12/13/2286884.html) 和 [读懂diff](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)。

再次执行 **`git status`** 命令查看当前本地仓库状态：  

```shell
➜  resizableImageWithCapInsets git:(master) git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working irectory)

    modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")  
```

提示修改了 README.md 文件（状态为modified），但是修改没有添加到缓存（Changes not staged）。
此种状态下，可以执行两种操作： 

- git checkout -- <file>：放弃本地修改，恢复到上次commit完的"干净"版本**Index**中的文件记录。
- git add <file> ：提交更新到缓存。

若执行 **`git add`** 命令可以将更改提交到暂存区（index/staging area）： 

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git add README.md  
➜  resizableImageWithCapInsets git:(master) ✗ git status  
On branch master  
Changes to be committed:  
  (use "git reset HEAD <file>..." to unstage)  

  modified:   README.md  
```

+ 正常情况下，对于通过 <b>`git add`</b> 添加到暂存区的变更，进一步执行 <b>`git commit`</b> 提交到本地git仓库，这两步操作也可糅合为 **`git commit -a`** 一步到位。  
+ 若在 `git commit` 提交到HEAD之前，想要放弃通过 `git add` 提交到暂存区中的修改（Unstaging a Staged File/Undo an Added new modification），可执行 `git reset HEAD <file>` 撤销提交到暂存中的修改：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git reset HEAD README.md  
Unstaged changes after reset:  
M	README.md  
```

执行 reset HEAD 后，Index/Staging Area 中记录的 README.md 文件恢复恢复到上次commit完 local repository 中的 HEAD 文件记录。
Working tree 中 README.md 文件的状态则恢复为 modified（Changes Unstaged relative to index），可执行 `git status` 查看最新状态。

### 删除文件

将README.md文件添加到git版本控制并且提交到git仓库后，在正式推送到远程仓库之前，若想撤销最近一次的commit怎么办呢？可参考《[git 撤销commit](http://www.miss77.net/804.html)》、《[Git 撤销修改1](http://www.cnblogs.com/mengdd/p/3586106.html)》、《[Git 撤销修改2](http://blog.csdn.net/baple/article/details/39640129)》。
另一方面，我们也可以删除本地git仓库中的tracked/versioned文件：

```shell
➜  resizableImageWithCapInsets git:(master) git rm README.md
rm 'README.md'
```

再次执行 **`git status`** 命令查看当前本地仓库状态：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git status  
On branch master  
Changes to be committed:  
  (use "git reset HEAD <file>..." to unstage)  

    deleted:    README.md  
```

可执行 `git reset HEAD <file>` 撤销提交到暂存中的 `git rm` 操作（recover by local repository's HEAD to resume unstage status），也可以进一步执行<b>`git commit`</b>提交缓存操作到本地git仓库生效。

### 重命名文件（夹）

**[Rename files and folders with git](http://www.patrick-wied.at/blog/rename-files-and-folders-with-git)**

```shell
mv oldfolder newfolder  
git add newfolder  
git remove oldfolder  
```

本质上就是备份重命名，然后 remove 旧的，add 新的，实现替换重命名。  
执行完以上命令后，可以进一步执行 `git commit` 提交缓存操作到本地git仓库生效。  

## git push ( merge local to remote )

前面我们本地 git 初始化了 resizableImageWithCapInsets 项目，并新增提交了 README.md 文件，所有改动已经提交到了本地仓库的 HEAD，但是还没到提交到 git 远程仓库。
执行 `git status` 命令可查看当前状态：

```shell
➜  resizableImageWithCapInsets git:(master) git status  
On branch master  
Your branch is ahead of 'origin/master' by 1 commit.  
   (use "git push" to publish your local commits)  
nothing to commit, working directory clean  
```

+ `Your branch is ahead of 'origin/master' by 1 commit`：提示我们的git本地仓库已经超过远程仓库一个版本。  
+ `use "git push" to publish your local commits`：建议我们使用**`git push`**命令发布本地仓库上提交的修改。  

git 是分布式版本控制系统。对于一个分布式节点来说，其它节点的 git 仓库都可以作为本地仓库的 **远程仓库**，可理解为 “git 服务器”。
当项目进行到一定的阶段时，要同别人分享目前的成果，可以使用 `git push` 命令将本地仓库中的数据推送到远程仓库。

在当前 Working tree 执行 `git remote -v` 查看其对应的远程仓库。

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git remote -v  
```

可以看到列出来的结果为空，实际上我们本地演示的git仓库尚未挂接到任何远端仓库。

这里移步[github](https://github.com/)，关于本地通过SSH与github连接请参考《[Mac下git通过SSH进行免密码安全连接github](http://blog.csdn.net/phunxm/article/details/45083335)》。

由于 `Github does not provide shell access`，因此需要去 github 个人主页手动创建仓库。Add a New Repository，名称为 resizableImageWithCapInsets，对应的URL为：

type          | URL
--------------|--------------------------------------------------------------
SSH clone URL |        git@github.com:fan2/resizableImageWithCapInsets.git
HTTPS clone URL |      https://github.com/fan2/resizableImageWithCapInsets.git
Subversion checkout URL | https://github.com/fan2/resizableImageWithCapInsets

由上可知 github 也提供了 subversion 支持，实际上 git 也可以与通过[git-svn](http://blog.chinaunix.net/uid-11639156-id-3077471.html)子命令与已有的SVN中心库进行同步。

### 连接远程仓库

git 对远程仓库的操作主要是基于 `git remote` 命令：

```shell
➜  ~  git remote --help

NAME
       git-remote - Manage set of tracked repositories

DESCRIPTION
       Manage the set of repositories ("remotes") whose branches you track.

COMMANDS

       add
           Adds a remote named <name> for the repository at <url>.

       remove, rm
           Remove the remote named <name>. All remote-tracking branches and
           configuration settings for the remote are removed.

       set-url
           Changes URLs for the remote. 

```

Adds a remote named <name> for the repository at <url>.

下面执行 `git remote add` 命令将当前本地git仓库挂接到指定的远程仓库（即追踪远程仓库）：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git remote add origin git@github.com:fan2/resizableImageWithCapInsets.git
```

这里将 github 远端仓库地址保存到 `origin` 变量中，后面 push、fetch、merge 等操作可直接引用 origin 变量指代远端地址。
此时再执行执行 `git remote -v`（--verbose：Be a little more verbose and show remote url after name），可查看 origin 具体代表的 fetch/push 远端地址：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git remote -v
origin	git@github.com:fan2/resizableImageWithCapInsets.git (fetch)
origin	git@github.com:fan2/resizableImageWithCapInsets.git (push)
```

如果执行 `git remote add` 命令时提示 `fatal: remote origin already exists.`，或者想更改挂接的远端地址（比如远程仓库重命名了或移动了），可以按照以下步骤操作：

> 先执行命令：`git remote rm origin`  
> 再执行命令：`git remote add origin <remoteGitRepoURL>`  

`git remote set-url`（Changes URLs for the remote）相当于 rm+add：

> [Changing a remote's URL](https://help.github.com/articles/changing-a-remote-s-url/)
> [How can I change the remote/target repository URL](http://stackoverflow.com/questions/1800859/how-can-i-change-the-remote-target-repository-url-on-windows)
> [Change the URI (URL) for a remote Git repository](http://stackoverflow.com/questions/2432764/change-the-uri-url-for-a-remote-git-repository)

一般公司可能会屏蔽掉 SSH 协议，此时可以 `rm & add` 切换为 HTTPS clone URL，但是每次 git push 时都会提示输入用户名和密码。
在 OS X 上可以使用 keychain 来缓存密码：

> git config --global credential.helper osxkeychain

### git推送更新到它所追踪的远程仓库

我们前面使用 `git commit` 命令将本地工作目录的变更提交（合并）到本地仓库，git 使用 `git push` 命令来将本地仓库的变更提交（合并）到它所追踪的远程仓库：

```
➜  ~  git push --help

NAME
       git-push - Update remote refs along with associated objects

DESCRIPTION
       Updates remote refs using local refs, while sending objects necessary to
       complete the given refs.

OPTIONS
       -u, --set-upstream
           For every branch that is up to date or successfully pushed, add upstream
           (tracking) reference, used by argument-less git-pull(1) and other commands.
           For more information, see branch.<name>.merge in git-config(1).

```

执行以下命令可以将本地仓库变更提交（合并）到它所追踪的远程仓库 origin 的 master 分支：

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git push -u origin master
```

## git rebase from remote ( pull = fetch + merge)

当本地仓库跟踪的远程仓库发生变更时，需要rebase（merge from remote to local）。

### fetch + merge

+ `git fetch` 取得两个版本的差异：

```shell
➜  ~  git fetch --help

NAME
       git-fetch - Download objects and refs from another repository

DESCRIPTION
       Fetch branches and/or tags (collectively, "refs") from one or more other
       repositories.

```

调用 `git fetch origin` 取得 remote 仓库 origin（master分支）相对 local repository HEAD 的变动 diff，可手工 merge。

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git fetch origin  
```

+ `git merge`命令合并两个版本：

```shell
➜  ~  git merge --help

NAME
       git-merge - Join two or more development histories together

DESCRIPTION
       Incorporates changes from the named commits (since the time their histories
       diverged from the current branch) into the current branch. This command is used
       by git pull to incorporate changes from another repository and can be used by
       hand to merge changes from one branch into another.
```

调用 `git merge origin/master` 把更新（远程origin/master相对local repository的diff）合并（patch）到本地分支HEAD。

```shell
➜  resizableImageWithCapInsets git:(master) ✗ git merge origin/master
```

如有多个分支，可调用 `git branch` 命令查看当前分支。

### pull

当我们在本地修改了文件准备 push 推送提交时，有人已经 push 改动过远程仓库，此时 git 会提示 failed，需要执行 pull before push：

![git pull & push](http://img.blog.csdn.net/20150925230344452)

git pull = fetch + merge

```
➜  ~  git pull --help

NAME
       git-pull - Fetch from and integrate with another repository or a local branch

DESCRIPTION
       Incorporates changes from a remote repository into the current branch. In its
       default mode, git pull is shorthand for git fetch followed by git merge
       FETCH_HEAD.

       More precisely, git pull runs git fetch with the given parameters and calls git
       merge to merge the retrieved branch heads into the current branch. With
       --rebase, it runs git rebase instead of git merge.
```

执行 `git pull` 之后，重新执行 `git push` 成功提交。

有时候，别人和自己改动了同一个文件，当自己 pull 时会提示 conflict：

![git conflict](http://img.blog.csdn.net/20150925230223046)

`Automatic merge failed`，此时，需要我们手动解决冲突再重新push提交（fix conflicts and then commit the result）。

## git rebase from upstream to sync a fork

1. [Configuring a remote for a fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/)  
2. [Syncing a fork](https://help.github.com/articles/syncing-a-fork/)  

参考：
---

&nbsp;&nbsp;&nbsp;&nbsp;《[写给Git初学者的7个建议](http://blog.jobbole.com/50603/)》  
&nbsp;&nbsp;&nbsp;&nbsp;《[我也用github](http://blog.csdn.net/xiahouzuoxin/article/details/9393119)》  
&nbsp;&nbsp;&nbsp;&nbsp;《[git/github初级运用自如](http://www.cnblogs.com/fnng/archive/2012/01/07/2315685.html)》  
