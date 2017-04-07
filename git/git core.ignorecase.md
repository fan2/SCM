core.ignorecase
===

[关于git不区分文件名大小写的处理](http://www.cnblogs.com/rollenholt/p/4060238.html)

[如何配置Git支持大小写敏感和修改文件名中大小写字母呢？](http://blog.hexu.org/archives/1909.shtml)


如何解决Git的大小不敏感问题呢？

- 方案一：配置git大小写敏感：

```Shell
 $ git config core.ignorecase false
```

- 方案二是先删除文件，再添加进去：

```Shell
$ git rm ; git add ;  git commit -m "rename file"
```
