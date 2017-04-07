
当前Shell窗口添加环境变量
----
下载`git-2.5.3-intel-universal-mavericks.dmg`，默认安装到`/usr/local/git/`，在当前bash（zsh） shell窗口添加环境变量。

```Shell
➜  ~  git version
git version 2.3.8 (Apple Git-58)
➜  ~  cd /usr/local/git/bin
➜  bin git:(master) ls
git                        git-receive-pack           git-upload-pack
git-credential-osxkeychain git-shell                  gitk
git-cvsserver              git-upload-archive
➜  bin git:(master) echo $PATH                           
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
➜  bin git:(master) export PATH=/usr/local/git/bin:$PATH
➜  bin git:(master) echo $PATH                          
/usr/local/git/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
➜  bin git:(master) git version
git version 2.5.3
```

该配置只对当前shell窗口生效；新建shell窗口，依旧是git version 2.3.8。

新建~/.bash_profile添加环境变量：
----

```Shell
export PATH=/usr/local/git/bin:$PATH
```

新建shell窗口/重启Terminal/重启Mac，依旧是git version 2.3.8。

新建~/.profile添加环境变量
----

```Shell
export PATH=/usr/local/git/bin:$PATH
```

新建shell窗口/重启Terminal/重启Mac，依旧是git version 2.3.8。

修改/etc/paths，添加环境变量
----
插入首行：

```Shell
/usr/local/git/bin
```

新建shell窗口/重启Terminal/重启Mac，依旧是git version 2.3.8。
删除/etc/paths.d/git，如故。

使用[ln](http://blog.chinaunix.net/uid-25445243-id-3206874.html)命令链接升级Xcode的git
----
最终还是参照[Mac Xcode svn的升级方法](http://blog.csdn.net/phunxm/article/details/40834427)，使用ln命令链接指向/usr/local/git/bin下新安装的git*。

### 进入查看 Xcode 的 binUtils 目录

```Shell
➜  ~  cd /Applications/Xcode.app/Contents/Developer/usr/bin/
```

```Shell
➜  bin  ls git*
git                git-receive-pack   git-upload-archive
git-cvsserver      git-shell          git-upload-pack
```

### 查看 /usr/local/git/bin/git*

```Shell
➜  bin  ls /usr/local/git/bin/git*
/usr/local/git/bin/git                        /usr/local/git/bin/git-shell
/usr/local/git/bin/git-credential-osxkeychain /usr/local/git/bin/git-upload-archive
/usr/local/git/bin/git-cvsserver              /usr/local/git/bin/git-upload-pack
/usr/local/git/bin/git-receive-pack           /usr/local/git/bin/gitk
```

备份原来的 binUtils 为 *.org。

```Shell
➜  bin  sudo mv ./git ./git.org
Password:
➜  bin  sudo mv ./git-receive-pack ./git-receive-pack.org
➜  bin  sudo mv ./git-upload-archive ./git-upload-archive.org
➜  bin  sudo mv ./git-cvsserver ./git-cvsserver.org
➜  bin  sudo mv ./git-shell ./git-shell.org
➜  bin  sudo mv ./git-upload-pack ./git-upload-pack.org
➜  bin  ls git*
git-cvsserver.org      git-shell.org          git-upload-pack.org
git-receive-pack.org   git-upload-archive.org git.org
```

### 超链 /usr/local/git/bin/ 到 Xcode 的 binUtils

```Shell
➜  bin  sudo ln -Ff /usr/local/git/bin/git git
➜  bin  sudo ln -Ff /usr/local/git/bin/git-receive-pack git-receive-pack
➜  bin  sudo ln -Ff /usr/local/git/bin/git-upload-archive git-upload-archive
➜  bin  sudo ln -Ff /usr/local/git/bin/git-cvsserver git-cvsserver
➜  bin  sudo ln -Ff /usr/local/git/bin/git-shell git-shell
➜  bin  sudo ln -Ff /usr/local/git/bin/git-upload-pack git-upload-pack
➜  bin  sudo ln -Ff /usr/local/git/bin/git-credential-osxkeychain git-credential-osxkeychain
➜  bin  sudo ln -Ff /usr/local/git/bin/gitk gitk
```

```Shell
➜  bin  ls git*
git                        git-receive-pack.org       git-upload-pack
git-credential-osxkeychain git-shell                  git-upload-pack.org
git-cvsserver              git-shell.org              git.org
git-cvsserver.org          git-upload-archive         gitk
git-receive-pack           git-upload-archive.org
```

```Shell
➜  bin  git version
git version 2.5.3
```

新建shell窗口/重启Terminal/重启Mac，都是最新git version 2.5.3。

**说明：**

> git 2.5.3 中包含 `git-credential-osxkeychain`、`gitk`
