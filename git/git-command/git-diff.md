
```
工作区           暂存区                本地仓库                          远程仓库
    \          /      \            /        \                         /
     \        /         \         /           \                      /
     git diff         git diff --cache     git diff origin/master master
```

## git help diff

```
GIT-DIFF(1)                                 Git Manual                                 GIT-DIFF(1)



NAME
       git-diff - Show changes between commits, commit and working tree, etc

SYNOPSIS
       git diff [<options>] [<commit>] [--] [<path>...]
       git diff [<options>] --cached [<commit>] [--] [<path>...]
       git diff [<options>] <commit> <commit> [--] [<path>...]
       git diff [<options>] <blob> <blob>
       git diff [<options>] --no-index [--] <path> <path>


DESCRIPTION
       Show changes between the working tree and the index or a tree, changes between the index
       and a tree, changes between two trees, changes between two blob objects, or changes between
       two files on disk.
```

## git diff

参考 [git不同阶段撤回](http://einverne.github.io/post/2017/12/git-reset.html)

`git diff`: 查看已修改，未暂存的内容  
`git diff --cached`: 查看已暂存，未提交的内容  
`git diff origin/master master`: 查看已提交，未推送的差异  

## git diff between branches

[git-diff - Show changes between commits, commit and working tree, etc](https://git-scm.com/docs/git-diff)  

[git 对比两个分支差异](https://blog.csdn.net/u011240877/article/details/52586664)  
[git diff 查看两个分支的区别并将结果输出到指定文件](https://segmentfault.com/q/1010000005033288)  

[How to compare files from two different branches?](https://stackoverflow.com/questions/4099742/how-to-compare-files-from-two-different-branches)

```
ifan@THOMASFAN-MC1:~/Projects/git.code.com/scenario|dev-checkpskey⚡
⇒  git diff remote-upstream-branch dev-substream-branch -- src/static/js/util.js
```

对比子流（右）针对父流（左）的某个文件修改。
