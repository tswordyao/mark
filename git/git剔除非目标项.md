git clean 删除不想被track的文件

git clean -n
是一次clean的演习, 告诉你哪些文件会被删除. 记住他不会真正的删除文件, 只是一个提醒.
 
git clean -f
删除当前目录下所有没有track过的文件. 他不会删除.gitignore文件里面指定的文件夹和文件, 不管这些文件有没有被track过.
 
git clean -f <path>
删除指定路径下的没有被track过的文件.
 
git clean -df
删除当前目录下没有被track过的文件和文件夹.
 
git clean -xf
删除当前目录下所有没有track过的文件. 不管他是否是.gitignore文件里面指定的文件夹和文件.
 

讨论
git reset --hard和git clean -f是一对好基友. 结合使用他们能让你的工作目录完全回退到最近一次commit的时候. 
 
git clean对于刚编译过的项目也非常有用. 如, 他能轻易删除掉编译后生成的.o和.exe等文件.  这个在打包要发布一个release的时候非常有用. 


当不小心git add -A 添加了不需要的文件时
$ git rm -r -cached .    清空当前文件夹的所有缓存（本地的依然保存）
