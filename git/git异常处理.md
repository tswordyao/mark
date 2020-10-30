如果移动目录位置导致部分Git信息错误，分支信息不全
```
可以把一个本地没有的分支拉到本地  git pull origin publish:publish
```

**移动目录到SD卡可能会导致filemode变化，导致有很多文件显示变化，配置忽略**
```
git config --add core.filemode false
```


- 如果已经不小心commit，git fetch --all 再 git reset --hard origin/master

```
不想这个目录跟git有任何关系 删除git删除git目录联系
rm -rf .git
```


* 当git记录过多，尤其历史中有大文件删减时，Git项目将变得非常大，.git/objects/ 文件太大

```
git clone ssh://git@g.h.com/study.git --depth 99
或使用这个https://git-lfs.github.com/
或git rebase

git clone ssh://git@g.xxxx.com:22222/yooc.git --depth 99
```