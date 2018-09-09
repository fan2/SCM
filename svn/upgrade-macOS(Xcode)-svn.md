
[Update SVN-Client in XCode and OSX](http://stackoverflow.com/questions/19177699/using-svn-1-8-3-with-xcode-5)

## macOS 下的 svn 工具包

### which svn

在终端输入 `which svn` 查看系统默认使用的 svn 位置：

```shell
faner@MBP-FAN:~ $ which svn
/usr/bin/svn
```

> 取决于 PATH 中的顺序。

### whereis svn

在终端输入 `whereis svn` 查看系统的 svn 安装在哪里：

```shell
faner@MBP-FAN:~ $ whereis svn
/usr/bin/svn
```

### svn binUtils

```shell
faner@MBP-FAN:~ $ ls -1 /usr/bin/svn*
/usr/bin/svn*
/usr/bin/svnadmin*
/usr/bin/svndumpfilter*
/usr/bin/svnlook*
/usr/bin/svnserve*
/usr/bin/svnsync*
/usr/bin/svnversion*
```

`/usr/bin/` 下的 svn 工具包实际上是 Xcode command-line tools 的 [shims or wrapper](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools/) executables，真正的工具包 **实体** 是在 `Xcode.app` 里面：

```shell
~ $ xcrun -f svn
/Applications/Xcode.app/Contents/Developer/usr/bin/svn
```

查看终端默认的 svn 版本：

```shell
# 或者 $(which svn) --version
faner@MBP-FAN:~ $ `which svn` --version
svn, version 1.9.7 (r1800392)
   compiled Jun  4 2018, 17:48:35 on x86_64-apple-darwin16.1.0

Copyright (C) 2017 The Apache Software Foundation.
```

查看 `Xcode.app` 开发者工具包中的 svn 版本：

```shell
# Xcode 9.4.1
faner@MBP-FAN:~ $ /Applications/Xcode.app/Contents/Developer/usr/bin/svn --version
svn, version 1.9.7 (r1800392)
   compiled Jun  4 2018, 17:48:35 on x86_64-apple-darwin16.1.0

Copyright (C) 2017 The Apache Software Foundation.
```

> `/usr/bin/svn` 版本同此。

查看 `Xcode-beta.app` 开发者工具包中的 svn 版本：

```shell
# Xcode-beta 10.0
faner@MBP-FAN:~ $ /Applications/Xcode-beta.app/Contents/Developer/usr/bin/svn --version
svn, version 1.10.0 (r1827917)
   compiled Aug  8 2018, 02:29:47 on x86_64-apple-darwin17.0.0

Copyright (C) 2018 The Apache Software Foundation.
```

## 手动安装更新 svn 工具包

### 下载 subversion 安装包

可从 [WANdisco](http://www.wandisco.com/subversion/download#osx) 网站页面（Download WANdisco Certified Subversion Binaries）填写需要下载的工具包版本信息以及个人信息提交申请，WANdisco 将会向邮箱中发送下载链接，点击即可下载最新 subversion 的 pkg 格式安装包。

从 WANdisco 下载的 subversion pkg 程序默认安装到 `/opt/subversion`：

```shell
~ $ cd /opt/subversion/bin
/opt/subversion/bin $ ls
svn		svnadmin	svndumpfilter	svnlook		svnrdump	svnsync
svn-tools	svnbench	svnfsfs		svnmucc		svnserve	svnversion
```

### 配置环境变量使用新安装的 svn

一般在 `~/.bash_profile` 文件中添加用户级环境变量，当 ***bash*** shell 以 login 方式执行时，会读取此文件，且该文件仅仅执行一次。  
调用 `cat` 命令查看发现 Mac 上默认不存在该文件：

```shell
~ $ cat .bash_profile
cat: .bash_profile: No such file or directory
```

输入 `touch .bash_profile` 命令新建配置文件，调用 `vim` 编辑，输入 `export PATH=/opt/subversion/bin:$PATH`：

```shell
~ $ touch .bash_profile
~ $ vi .bash_profile 
~ $ cat .bash_profile
export PATH=/opt/subversion/bin:$PATH
```

执行 `:wq` 保存退出 vim，再执行 `source .bash_profile` 命令重新加载 bash 配置文件，使环境变量生效：

```shell
~ $ source .bash_profile
```

在当前终端窗口或新建终端tab窗口，重新输入 `which svn` 和 `svn --version` 命令，发现 svn 已经使用的是最新安装的 1.9.1 版本：

```shell
~ $ which svn
/opt/subversion/bin/svn

~ $ svn --version
svn, version 1.9.1 (r1698128)
   compiled Aug 28 2015, 15:51:18 on x86_64-apple-darwin14.0.0

Copyright (C) 2015 The Apache Software Foundation.
```

更新 svn 后，cd 到旧版 svn 控制的工程，输入 `svn info` 提示需要 `svn upgrade`：

```shell
~/path/to/svn_working_copy $ svn info
svn: E155036: Please see the 'svn upgrade' command
svn: E155036: The working copy at '~/path/to/svn_working_copy' is too old (format 29) to work with client version '1.9.1 (r1698128)' (expects format 31). You need to upgrade the working copy first.
```

执行一下 **`svn upgrade`** 命令，升级当前 svn working copy 即可使用最新的 svn 管理版本控制。

在对工程进行 upgrade 后，重新使用 Xcode 打开工程，发现 Xcode Source Control 自带的旧版 svn 已经不能再检测出代码变更。  
我们需要手动替换 Xcode Command Line Tools 自带的旧版 svn 工具包。  

### 替换更新 Xcode 自带的 svn 工具包

首先 cd 进入 `/Applications/Xcode.app/Contents/Developer/usr/bin` 目录，输入 `ls svn*` 命令查看 svn 工具包：

```shell
~ $ cd /Applications/Xcode.app/Contents/Developer/usr/bin
/Applications/Xcode.app/Contents/Developer/usr/bin $ ls svn*
svn           svnadmin      svndumpfilter svnlook       svnrdump      svnserve      svnsync       svnversion
```

接下来调用 **`sudo mv`** 命令将旧工具包重命名为带 `.org` 尾缀的备份：

```shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo mv svn svn.org 
```

依次备份其他七个文件，然后通过 **`ln -s`** 命令将新版 svn 工具包 `/opt/subversion/bin/svn*` 软链到该目录下：

```shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo ln -s /opt/subversion/bin/svn* ./
```

重启 Xcode，即可正常 Check Remote Repository Status，检测出变更。

除了上面的方法外，也可以直接设置 Xcode 的 `XCSubversionToolPath` 路径：

```shell
~ $ defaults write com.apple.Xcode XCSubversionToolPath /opt/subversion/bin
```

**`说明`**：鉴于 `/usr/bin` 目录下的 svn 工具包（svn*）实际上是 Xcode Command Line Tools 的影子，可省去第3步，安装新版 subversion 后，直接替换更新 Xcode 下的对应 binUtils 即可！

___

新版本多出三个文件和一个文件夹：

+ **svnbench**: Subversion benchmarking tool.  
+ **svnfsfs**: "[FSFS](http://www.zhihu.com/question/26085317)" is the name of a Subversion filesystem implementation, an alternative to the original Berkeley DB-based implementation.  
+ **svnmucc**: Subversion multiple URL command client.  
+ ***svn-tools***: diff，svnauthz，svnraisetreeconflict，x509-parser，...  

## 使用 brew 安装 subversion

执行 `brew search svn`（或 `brew search subversion`）从 brew 仓库查找 svn，执行 `brew desc svn` 查看 svn 概述，执行 `brew info svn` 查看 svn 详细信息。

执行 **`brew install svn`** 安装 svn，默认安装到 `/usr/local/Cellar/subversion/1.10.2`；安装完会调用 `brew link svn` 将 svn 系列工具包软链到 `/usr/local/bin/` 目录下。

可通过 `readlink` 或 `ls` 命令查验软链真身：

```shell
faner@MBP-FAN:~ $ readlink /usr/local/bin/svn
../Cellar/subversion/1.10.2/bin/svn

faner@MBP-FAN:~ $ ls -al /usr/local/bin/svn
lrwxr-xr-x  1 faner  admin    35B Jul 29 12:07 /usr/local/bin/svn@ -> ../Cellar/subversion/1.10.2/bin/svn
```

### 将安装路径配置到环境变量

sudo vim 编辑 `/etc/paths`，在其中行首添加 `/usr/local/bin`，或者在 `~/.zshrc` 中 `export PATH=/usr/local/bin:$PATH` 退出 source。  
由于在 PATH 中 `/usr/local/bin` 在 `/usr/bin` 前面，所以默认先找到 brew 最新安装的 git 工具包。  

重新执行 `which svn` 和 `svn --version` 可发现，默认执行路径已经变成了 brew 安装软链过来的 `/usr/local/bin/svn`：

```shell
faner@MBP-FAN:~ $ which svn
/usr/local/bin/svn

faner@MBP-FAN:~ $ svn --version
svn, version 1.10.2 (r1835932)
   compiled Jul 20 2018, 13:28:33 on x86_64-apple-darwin17.7.0

Copyright (C) 2018 The Apache Software Foundation.
```

### 软链替换 Xcode 下的 svn 工具包

至此，终端执行的 svn 已经是最新的了，但是 Xcode.app 的 Source Control 还是调用 `/Contents/Developer/usr/bin/` 下的 svn 工具包，若想 Xcode SCM 也使用最新版本的 svn，可参考手动安装第三步进行 **软链替换更新**。

> 关于环境变量，参考 `macOS Env PATH` 议题。
