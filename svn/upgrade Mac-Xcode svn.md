
[Update SVN-Client in XCode and OSX](http://stackoverflow.com/questions/19177699/using-svn-1-8-3-with-xcode-5)

## Mac下的svn工具包

在终端输入 `whereis svn`查看系统的svn安装在哪里：

```shell
faner@MBP-FAN:~|⇒  whereis svn                   
/usr/bin/svn
```

```shell
faner@MBP-FAN:~|⇒  ls -1 /usr/bin/svn*
/usr/bin/svn
/usr/bin/svnadmin
/usr/bin/svndumpfilter
/usr/bin/svnlook
/usr/bin/svnserve
/usr/bin/svnsync
/usr/bin/svnversion
```

`/usr/bin/` 下的 svn 工具包实际上是 Xcode command-line tools 的 [shims or wrapper](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools/) executables，真正的工具包**实体**在Xcode.app里面：

```shell
~ $ xcrun -f svn
/Applications/Xcode.app/Contents/Developer/usr/bin/svn
```

在终端输入 `svn --version` 查看系统自带的svn版本比较旧，为1.7.20：

```shell 
faner@MBP-FAN:~|⇒  svn --version
svn, version 1.7.20 (r1667490)
   compiled Aug 17 2015, 15:24:52

Copyright (C) 2014 The Apache Software Foundation.
```

## 安装新版svn工具包

可从 [WANdisco](http://www.wandisco.com/subversion/download#osx) Download Subversion Binaries 网站页面填写个人信息提交下载申请，然后 WANdisco 会向邮箱中发送 download 链接，点击即可下载最新 subversion 的 pkg 格式安装包。

从 WANdisco 下载的 subversion pkg 程序默认安装到 `/opt/subversion` 目录：

```shell
~ $ cd /opt/subversion/bin
/opt/subversion/bin $ ls
svn		svnadmin	svndumpfilter	svnlook		svnrdump	svnsync
svn-tools	svnbench	svnfsfs		svnmucc		svnserve	svnversion
```

## 配置用户环境变量更新svn

一般在 `~/.bash_profile` 文件中添加用户级环境变量，当 ***bash*** shell 以 login 方式执行时，会读取此文件，且该文件仅仅执行一次。  
调用 `cat` 命令查看发现 Mac 上默认不存在该文件：

```shell
~ $ cat .bash_profile
cat: .bash_profile: No such file or directory
```

使用 `touch .bash_profile` 命令新建配置文件，使用 `vim` 编辑，输入 `export PATH=/opt/subversion/bin:$PATH`，最后使用 `source .bash_profile` 命令更新刚配置的环境变量使之加载生效：

```shell
~ $ touch .bash_profile
~ $ vi .bash_profile 
~ $ cat .bash_profile
export PATH=/opt/subversion/bin:$PATH
~ $ source .bash_profile
```

在当前终端窗口或新建终端tab窗口，重新输入 `svn --version` 命令，发现svn已经更新到1.9.1：

```shell
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

执行一下 `svn upgrade` 命令，升级当前 svn working copy 即可使用最新的 svn 管理版本控制。

在对工程进行 upgrade 后，重新使用 Xcode 打开工程，发现 Xcode Source Control 自带的旧版 svn 已经不能再检测出代码变更。  
我们需要手动替换 Xcode Command Line Tools 自带的旧版 svn 工具包。  

## 替换更新Xcode自带的svn

首先 cd 进入 `/Applications/Xcode.app/Contents/Developer/usr/bin` 目录，输入 `ls svn*` 命令查看 svn 工具包：

```shell
~ $ cd /Applications/Xcode.app/Contents/Developer/usr/bin
/Applications/Xcode.app/Contents/Developer/usr/bin $ ls svn*
svn           svnadmin      svndumpfilter svnlook       svnrdump      svnserve      svnsync       svnversion
```

接下来调用`sudo mv`命令将旧工具包重命名为带.org的备份：

```shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo mv svn svn.org 
```

依次备份其他七个文件，然后通过`ln -s`命令将新版svn工具包`/opt/subversion/bin/svn*`软链到该目录下：

```shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo ln -s /opt/subversion/bin/svn* ./
```

重启Xcode，即可正常 Check Remote Repository Status，检测出变更。

除了上面的方法外，也可以直接设置Xcode的XCSubversionToolPath路径：

```shell
~ $ defaults write com.apple.Xcode XCSubversionToolPath /opt/subversion/bin
```

**`说明：`**鉴于/usr/bin目录下的svn工具包（svn*）实际上是 Xcode Command Line Tools 的影子，可省去第3步，安装新版subversion后，直接替换更新Xcode下的对应binUtils即可！

___

新版本多出三个文件和一个文件夹：

+ **svnbench**  
Subversion benchmarking tool.
+ **svnfsfs**  
"[FSFS](http://www.zhihu.com/question/26085317)" is the name of a Subversion filesystem implementation, an
alternative to the original Berkeley DB-based implementation.
+ **svnmucc**  
Subversion multiple URL command client.
+ ***svn-tools***  
diff，svnauthz，svnraisetreeconflict，x509-parser，...
