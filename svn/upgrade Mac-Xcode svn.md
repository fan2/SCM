
[Update SVN-Client in XCode and OSX](http://stackoverflow.com/questions/19177699/using-svn-1-8-3-with-xcode-5)

##Mac下的svn工具包
在终端输入`whereis svn`查看系统的svn安装在哪里：

```Shell
~ $ whereis svn
/usr/bin/svn
```

`/usr/bin`目录下的svn工具包（svn\*）实际上是Xcode command-line tools 的 [shims or wrapper](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools/) executables，真正的工具包**实体**在Xcode.app里面：

```Shell
~ $ xcrun -f svn
/Applications/Xcode.app/Contents/Developer/usr/bin/svn
```

在终端输入`svn --version`查看系统自带的svn版本比较旧，为1.7.20：

```Shell 
~ $ svn --version
svn, version 1.7.20 (r1667490)
   compiled Aug 17 2015, 15:24:52

Copyright (C) 2014 The Apache Software Foundation.
```

##安装新版svn工具包
可从[WANdisco](http://www.wandisco.com/subversion/download#osx) Download Subversion Binaries网站页面填写个人信息提交下载申请，然后WANdisco会向邮箱中发送download链接，点击即可下载最新subversion的pkg格式安装包。
WANdisco下载的subversion pkg程序默认安装到/opt/subversion目录：

```Shell
~ $ cd /opt/subversion/bin
/opt/subversion/bin $ ls
svn		svnadmin	svndumpfilter	svnlook		svnrdump	svnsync
svn-tools	svnbench	svnfsfs		svnmucc		svnserve	svnversion
```

##配置用户环境变量更新svn
一般在~/`.bash_profile`文件中添加用户级环境变量，当***bash*** shell以login方式执行时，会读取此文件，且该文件仅仅执行一次。
调用`cat`命令查看发现Mac上默认不存在该文件：

```Shell
~ $ cat .bash_profile
cat: .bash_profile: No such file or directory
```

使用`touch`命令新建`.bash_profile`文件，使用`vi`编辑，输入`export PATH=/opt/subversion/bin:$PATH`，最后使用`source`命令更新刚配置的环境变量使之加载生效：

```Shell
~ $ touch .bash_profile
~ $ vi .bash_profile 
~ $ cat .bash_profile
export PATH=/opt/subversion/bin:$PATH
~ $ source .bash_profile
```

在当前终端窗口或新建终端tab窗口，重新输入`svn --version`命令，发现svn已经更新到1.9.1：

```Shell
~ $ svn --version
svn, version 1.9.1 (r1698128)
   compiled Aug 28 2015, 15:51:18 on x86_64-apple-darwin14.0.0

Copyright (C) 2015 The Apache Software Foundation.
```

更新svn后，cd到旧版svn控制的工程，输入`svn info`提示需要`svn upgrade`：

```Shell
~/path/to/svn_working_copy $ svn info
svn: E155036: Please see the 'svn upgrade' command
svn: E155036: The working copy at '~/path/to/svn_working_copy' is too old (format 29) to work with client version '1.9.1 (r1698128)' (expects format 31). You need to upgrade the working copy first.
```

执行一下`svn upgrade`命令，升级当前svn working copy，即可使用最新的svn管理版本控制。
在工程进行upgrade后，重新使用Xcode打开工程，发现Xcode Source Control自带的旧版svn已经不能再检测出代码变更。我们需要手动替换Xcode Command Line Tools自带的旧版svn工具包。

##替换更新Xcode自带的svn
首先cd进入`/Applications/Xcode.app/Contents/Developer/usr/bin`目录，输入`ls svn*`命令查看svn工具包：

```Shell
~ $ cd /Applications/Xcode.app/Contents/Developer/usr/bin
/Applications/Xcode.app/Contents/Developer/usr/bin $ ls svn*
svn           svnadmin      svndumpfilter svnlook       svnrdump      svnserve      svnsync       svnversion
```

接下来调用`sudo mv`命令将旧工具包重命名为带.org的备份：

```Shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo mv svn svn.org 
```

依次备份其他七个文件，然后通过`ln -s`命令将新版svn工具包`/opt/subversion/bin/svn*`软链到该目录下：

```Shell
/Applications/Xcode.app/Contents/Developer/usr/bin $ sudo ln -s /opt/subversion/bin/svn* ./
```

重启Xcode，即可正常Check Remote Repository Status，检测出变更。

除了上面的方法外，也可以直接设置Xcode的XCSubversionToolPath路径：

```Shell
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
