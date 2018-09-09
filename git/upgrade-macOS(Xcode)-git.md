
参考 [**upgrade-macOS(Xcode)-svn**](../svn/upgrade-macOS(Xcode)-svn.md)：

1. 执行 **`brew install git`** 安装最新 git，配置 PATH 替换默认命令行工具；  
2. 执行 `ln` 命令将 git 工具包软链到 `/Applications/Xcode.app/Contents/Developer/usr/bin/` 目录下实现替换更新。  

安装前，终端默认使用 `/usr/bin/git`：

```shell
faner@MBP-FAN:~ $ whereis git
/usr/bin/git

faner@MBP-FAN:~ $ which git
/usr/bin/git

faner@MBP-FAN:~ $ /usr/bin/git --version
git version 2.15.2 (Apple Git-101.1)

faner@MBP-FAN:~ $ /Applications/Xcode.app/Contents/Developer/usr/bin/git --version
git version 2.15.2 (Apple Git-101.1)
```

安装后，终端优先使用 `/usr/local/bin/git`：

```shell
faner@MBP-FAN:~ $ which git
/usr/local/bin/git

faner@MBP-FAN:~ $ git --version
git version 2.18.0

faner@MBP-FAN:~ $ /usr/local/bin/git --version
git version 2.18.0
```

可通过 `readlink` 或 `ls` 命令查验软链真身：

```shell
faner@MBP-FAN:~ $ readlink /usr/local/bin/git
../Cellar/git/2.18.0/bin/git

faner@MBP-FAN:~ $ ls -al /usr/local/bin/git
lrwxr-xr-x  1 faner  admin    28B Sep  7 08:02 /usr/local/bin/git@ -> ../Cellar/git/2.18.0/bin/git
```

## 手动安装更新 git

从官网 https://git-scm.com/downloads 下载 macOS 安装包 `git-2.x.x-intel-universal-mavericks.dmg`，默认安装到 `/usr/local/git/`。

```shell
➜  ~  git version
git version 2.3.8 (Apple Git-58)
➜  ~  cd /usr/local/git/bin
➜  bin git:(master) ls
git                        git-receive-pack           git-upload-pack
git-credential-osxkeychain git-shell                  gitk
git-cvsserver              git-upload-archive
```

**1. 当前 Shell 窗口添加环境变量**

```shell
➜  bin git:(master) echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
➜  bin git:(master) export PATH=/usr/local/git/bin:$PATH

➜  bin git:(master) echo $PATH
/usr/local/git/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
```

该配置只对当前 shell 窗口生效：

```shell
➜  bin git:(master) git version
git version 2.5.3
```

新建 shell 窗口，依旧是 git version 2.3.8。

**2. 新建 `~/.bash_profile` 添加环境变量**

```shell
export PATH=/usr/local/git/bin:$PATH
```

新建 shell 窗口 / 重启 Terminal / 重启 macOS，依旧是 git version 2.3.8？

**3. 新建 `~/.profile` 添加环境变量**

```shell
export PATH=/usr/local/git/bin:$PATH
```

新建 shell 窗口 / 重启 Terminal / 重启 macOS，依旧是 git version 2.3.8。

**4. 修改 `/etc/paths`，添加环境变量**

插入首行：

```shell
/usr/local/git/bin
```

新建 shell 窗口 / 重启 Terminal / 重启 macOS，依旧是 git version 2.3.8？

删除 `/etc/paths.d/git`，如故。

## 使用 [ln](http://blog.chinaunix.net/uid-25445243-id-3206874.html) 命令链接升级 Xcode 的 git

### 进入查看 Xcode 的 binUtils 目录

```shell
➜  ~  cd /Applications/Xcode.app/Contents/Developer/usr/bin/
```

```shell
➜  bin  ls git*
git                git-receive-pack   git-upload-archive
git-cvsserver      git-shell          git-upload-pack
```

### 查看 /usr/local/git/bin/git*

```shell
➜  bin  ls /usr/local/git/bin/git*
/usr/local/git/bin/git                        /usr/local/git/bin/git-shell
/usr/local/git/bin/git-credential-osxkeychain /usr/local/git/bin/git-upload-archive
/usr/local/git/bin/git-cvsserver              /usr/local/git/bin/git-upload-pack
/usr/local/git/bin/git-receive-pack           /usr/local/git/bin/gitk
```

备份原来的 binUtils 为 *.org。

```shell
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

```shell
➜  bin  sudo ln -Ff /usr/local/git/bin/git git
➜  bin  sudo ln -Ff /usr/local/git/bin/git-receive-pack git-receive-pack
➜  bin  sudo ln -Ff /usr/local/git/bin/git-upload-archive git-upload-archive
➜  bin  sudo ln -Ff /usr/local/git/bin/git-cvsserver git-cvsserver
➜  bin  sudo ln -Ff /usr/local/git/bin/git-shell git-shell
➜  bin  sudo ln -Ff /usr/local/git/bin/git-upload-pack git-upload-pack
➜  bin  sudo ln -Ff /usr/local/git/bin/git-credential-osxkeychain git-credential-osxkeychain
➜  bin  sudo ln -Ff /usr/local/git/bin/gitk gitk
```

```shell
➜  bin  ls git*
git                        git-receive-pack.org       git-upload-pack
git-credential-osxkeychain git-shell                  git-upload-pack.org
git-cvsserver              git-shell.org              git.org
git-cvsserver.org          git-upload-archive         gitk
git-receive-pack           git-upload-archive.org
```

```shell
➜  bin  git version
git version 2.5.3
```

新建shell窗口/重启Terminal/重启Mac，都是最新git version 2.5.3。

**说明：**

> git 2.5.3 中包含 `git-credential-osxkeychain`、`gitk`
