[mac 电脑用终端生成 SSH key 访问自己的 Github](https://www.jianshu.com/p/5b34b7b34cae)  
[Error: Permission denied (publickey)](https://help.github.com/articles/error-permission-denied-publickey/)  
[解决方案 git@github.com 出现 Permission denied (publickey)](https://blog.csdn.net/samxx8/article/details/51497004)  

是否使用注册的用户名和邮箱？

## ssh-keygen 注册 rsa 公私钥对

```shell
❯ cd .ssh

~/.ssh
❯ ssh-keygen -t rsa -C "929683282@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/faner/.ssh/id_rsa): id_rsa_github
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa_github.
Your public key has been saved in id_rsa_github.pub.
The key fingerprint is:
SHA256:La7x**************************/hT********pk 929683282@qq.com

The key's randomart image is:
+---[RSA 2048]----+
|  . o  ....      |
|.. =  .    .     |
|ooB...    . .    |
|.O.B.    o .     |
|ooO o o S o      |
|EX   + o +       |
|B + . + o        |
| =   = B         |
|  o.. +.+        |
+----[SHA256]-----+
```

> 这里不输入 passphrase，设置密码为空。

## New SSH key for github

登录进入 github，在 `Settings | SSH and GPG keys` 中 **`New SSH key`** 新增 SSH 公钥。

填写 Title 为 `id_rsa_github`，填写 Key 为 `id_rsa_github.pub`。

执行 `cat id_rsa_github.pub` 可查看 pub 文件内容：

```shell
~/.ssh   34s
❯ cat id_rsa_github.pub
```

## ssh -T 

```shell
~/.ssh
❯ ssh -T git@github.com
git@github.com: Permission denied (publickey).
```

可执行 `ssh -vT git@github.com` 查看更详细的调试结果。

### 将 id_rsa_github 改名为 id_rsa

```shell
~/.ssh
❯ mv id_rsa_github id_rsa

~/.ssh
❯ mv id_rsa_github.pub id_rsa.pub
```

重新执行 `ssh -T git@github.com`，结果正常：

```shell
~/.ssh
❯ ssh -T git@github.com
Hi fan2! You've successfully authenticated, but GitHub does not provide shell access.
```

参考 [VS Code requires git ssh keys to be named as "id_rsa"](https://github.com/Microsoft/vscode/issues/14581)

### ssh-add

**`ssh-add ~/.ssh/id_rsa_github`**：将密钥 `id_rsa_github` 加入到 ssh-agent 中

> `ssh-add` -- adds private key identities to the authentication agent  

`ssh-add -l`：查看加入的密钥列表

> `-l` -- Lists fingerprints of all identities currently represented by the agent.  

[解决 Enter passphrase for key](https://blog.csdn.net/superbfly/article/details/75287741)

```shell
#Start SSH agent in the background
eval `ssh-agent`
ssh-add
```

ssh-agent 是用于管理密钥，**ssh-add** 用于将密钥加入到 ssh-agent 中，SSH 可以和 ssh-agent 通信获取密钥，这样就不需要用户手工输入密码了。  
顺序执行以上两条命令后，就可以用 ssh 免密登录远程机器了，但这个配置只对当前会话生效，会话关闭或机器重启后都需要重新执行这两条命令。  
将命令放到 `~/.bash_profile` 中，就可以免去每次输入的麻烦。  

[mac 配置完 ssh 依然提示"Enter passphrase for key"解决方法](https://blog.csdn.net/matrix_laboratory/article/details/75221305)

`ssh-add -K xxx`

## solutions

当 git repo 的 remote url 是 SSH 格式时，vscode git push 也会提示 [Git: Permission denied (publickey)](https://github.com/Microsoft/vscode/issues/42039)。

解决方案如下：

1. 在项目目录下执行 `start-ssh-agent`？  
2. 在项目目录下执行 `ssh-add ~/.ssh/id_rsa_github`；  
3. 在 `~/.bash_profile` 或 `~/.zshrc` 中输入以下两条命令：

	- eval `ssh-agent`
	- ssh-add