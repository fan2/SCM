
[git 生成patch和使用patch](https://blog.csdn.net/maybe_windleave/article/details/8703778)  

```
git diff > patch
git diff --staged > patch
```

```
git diff -- Classes > ~/bugfix/Classes.patch
git diff --staged -- Classes > ~/bugfix/Classes.patch

git stash show stash@{1} --patch > patch
```

[git format-patch 生成指定commit的补丁](https://blog.csdn.net/sinat_29891353/article/details/80803103)  
[使用 git format-patch 生成patch和应用patch](https://www.jianshu.com/p/814fb6606734)  

> 检查、测试、应用

检查patch文件

```
git apply --stat ~/bugfix/FAViewController.patch
```

查看是否能应用成功

```
git apply --check ~/bugfix/FAViewController.patch
```

应用 patch：

```
git am -s < ~/bugfix/FAViewController.patch
```

[如何使用 git 生成patch 和打入patch](https://blog.csdn.net/liuhaomatou/article/details/54410361)  
[**Git 打补丁-- patch 和 diff 的使用**](https://juejin.im/post/5b5851976fb9a04f844ad0f4)  

中途放弃：`git am --abort`
