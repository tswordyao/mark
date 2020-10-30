如果移动目录位置导致部分Git信息错误，分支信息不全
```
可以把一个本地没有的分支拉到本地  git pull origin publish:publish
```

**移动目录到SD卡可能会导致filemode变化，导致有很多文件显示变化，配置忽略**
```
git config --add core.filemode false
```


如果已经不小心commit，git fetch --all 再 git reset --hard origin/master