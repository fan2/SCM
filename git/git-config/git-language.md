
[Git Status 中文乱码解决](http://blog.crhan.com/2012/09/git-status-%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%A7%A3%E5%86%B3/)  

## 中文乱码

[git 正常显示中文文件名](https://www.jianshu.com/p/297ff9b730cf)  
[git status 显示中文和解决中文乱码](https://www.cnblogs.com/tielemao/p/9492638.html)  

不对0x80以上的字符进行quote，解决git status/commit时中文文件名乱码

```
git config --global core.quotepath false
```

## 日志 ESC

[How to fix ESC\[ in your terminal](http://excid3.com/blog/how-to-fix-esc-in-your-terminal)

[How to get rid of ESC\[ characters when using git diff on Mac OS X Mavericks?](https://stackoverflow.com/questions/20414596/how-to-get-rid-of-esc-characters-when-using-git-diff-on-mac-os-x-mavericks)

将 `~/.zshrc` 中的以下屏蔽掉即可：

```
LESS='-Pslines %lt-%lb bytes %bt-%bb %Pb\% of file %f';
export LESS
```
