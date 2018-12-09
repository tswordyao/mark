
在根目录中输入命令 
```
nw .
```
运行没有问题，接下来就要开始打包了.

<br>

将~/nwjs-osx-32/nwjs.app 目录拷贝到根目录，并修改名字为edu.app，可以在根目录下输入下面命令：
```
cp -R ~/nwjs-osx-32/nwjs.app ~/mv nwjs.app edu.app
```

<br>

打包项目文件Test到edu.app/Contents/Resources/目录中，在Test目录中执行下面命令：
```
zip -r ./edu.app/Contents/Resources/app.nw *
```

<br>

现在在根目录下输入下面命令，应该可以看到和上面执行相同的效果：
```
open edu.app   
```
到此，Mac下的可执行文件已制作完成

> 如果想将test.app制作成可以安装的dmg文件，可以使用系统自带的磁盘工具，具体参考：http://blog.yorkgu.me/2015/05/12/how-to-make-dmg-files-in-mac-osx/
