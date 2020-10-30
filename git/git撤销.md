## git 撤回

```
git status 先看一下add 中的文件 
git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
git reset HEAD XXX/XXX/XXX.java 就是对某个文件进行撤销了
```

```
git add . 
git reset HEAD


git commit -m"update"
git reset --soft HEAD~1


一顿操作完全混乱之后
git reset --hard HEAD^    	等同于~1
git reset --hard HEAD~3  	恢复到第n次更改前的样子
git reset --hard origin/master 当前目录恢复成origin/master的样子
git reset --hard XXXXXX    	当前目录恢复成某次提交记录的样子

此命令会重置本地文件, 上面两个只是撤销提交状态
```



> 如果你没有commit你的本地修改（甚至于你都没有通过git add追踪过这些文件，当他们被删除，git reset –hard对于这些没有被commit过也没有git add过的修改来说就是具有毁灭性的

```
gitee tsword/hisign666

git rm -r --cached .   删除已add进缓存区的内容, 这玩意不怎么用

#注释
!x.js  例外
/underRoot
underThis/
doc/*.txt
*.js
```