十五. Git branch 分支
查看当前有哪些branch
$ git branch

新建一个branch xm2.x
$ git branch xm2.x

切换到一个branch
$ git checkout xm2.x

新建并且切换到该branch,例: mybra
$ git checkout -b mybra

添加一个文件到你的repo
$ git add ttt.txt

添加所有的文件 git add .
$ git add .

commit一个文件
$ git commit -m "bixiaopeng test case"

commit到本地
$ git commit -a -m "xm test case"

加了-a，在commit 的时候，能帮你省一步 git add ，但也只是对修改和删除文件有效， 新文件还是要git add，不然就是 UNtracked 

………….
查看几次commit的区别
$ git diff

撤销最近一次commit操作(文件内容更改依旧保存)
git reset HEAD~
----------------------重新琢磨后, 可复用旧的commit信息提交
git add .
git commit -c ORIG_HEAD  

git reset HEAD~        (波浪号后面加数字n可以指代最近的n次)
如果是带了--sort参数,仅仅是修改commit备注, add操作保留,
如果是带了--hard参数,完全是撤销commit修改, 文件内容也被改变,


将branch push到远程
$ git push origin xm2.x

查看本地分支
$ git branch 

查看远程分支
$ git branch -r

查看本地和远程所有的分支
$ git branch -a

如果看不到remote新增的分支
git fetch 更新下即可

修改branch的名字
$ git branch -m bra bra2

删除本地分支   git branch -d xxxxx

删除远程分支
$ git push origin --delete xm

删除本地的远程分支：
git branch -r -D origin/BranchName  这句靠谱


十六. Git 合并分支

首先切换到想要合并到的分枝下，运行git merge 要合进来的branch

例如本例中将test2分支合并到xm3分支的话，
进入xm3分支，运行git merge test2
如果合并顺利的话：git status查看下，确保当前分支为xm3.0

$ git status
 On branch xm3.0

$ git branch
  master
  test2.x
* xm3.0

$ git merge test2.x
Already up-to-date. 

合并冲突处理：
Automatic merge failed; fix conflicts and then commit the result.
修改冲突的文件后，git add 文件 然后，git commit

拉取远程分支到本地
git fetch origin someBranch  
，
git pull origin someBranch:localBranch  拉取远程分支并与本地任意分支合并，如果没有则会新建

git push --set-upstream origin someBranch  你第一拉取该分支，push的时候需要set上游分支先

如果push时并没有与远程建立up-stream关联, bash会提示:
git push --set-upstream origin feature/2c_buyone_getone__mxx_20180527

推荐的新方式
git branch --set-upstream-to=origin/feature/xxx

这种方式一步到位
git checkout -b 本地分支名x origin/远程分支名x
git checkout -b publish origin/publish





有个坑点, 如果远程刚建了分支, 会找不到,.   git branch -r也发现不了, 还是需要:
git fetch 更新下分支列表

其实可以直接master下git pull  再git checkout 远程分支名就可以了[无需origin]


$ git push origin test:master         // 提交本地test分支作为远程的master分支
$ git push origin test:test              // 提交本地test分支作为远程的test分支

如果:左边的分支为空，那么将删除:右边的远程的分支。
$ git push origin :test              // 刚提交到远程的test将被删除，但是本地还会保存的，不用担心

-------------------------------------------------------------

标签（tag）操作

1、列出所有tag
$ git tag

2、打轻量标签
$ git tag [tag name]

3、附注标签
$ git tag -a [tag name] -m [message]

例如，打v1.0标签
$ git tag -a v1.0 -m 'v1.0 release'

4、后期打标签
$ git tag -a [tag name] [version]
$ git tag -a v1.2 9fceb02 -m"test~~"

5、删除本地tag
$ git tag -d [tag]

例如，删除本地v1.0 标签
$ git tag -d v1.0

6、删除远程tag
$ git push origin --delete tag <tagname>
1
还有另外一种方式来删除，推送一个空tag到远程
$ git tag -d <tagname>
$ git push origin :refs/tags/<tagname>

7、 查看tag信息
$ git show [tag]
		1
9、提交指定tag
$ git push [remote] [tag]

例如，将v1.0标签推送到远程服务器上
$ git push origin v1.0

10、提交所有tag
$ git push [remote] --tags

-------------------------------------------------------------

