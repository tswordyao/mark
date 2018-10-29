# node和npm 升级的填坑之旅

今天，在家准备尝试typesciprt结合vue-cli搭建项目。

在安装vue-cli过程中有个警告，说某依赖项的需要node版本在6.x以上，而当前是5.x之类云云。

本来警告也不是错误，试了下vue-cli安装成功，vue init webpack-simple myprj 也是轻松加顺利。

但有那样的提示总归是不爽，恐怕隐患，便直接升级node到最新稳定版，用的是n.

>sudo npm install -g n

>sudo n stable

完事后，node -v就是8.4了。

顺便升级npm
>sudo npm install npm@latest -g

一切仿佛轻松+愉快。然而装完发现，npm跑不起来了，报错：
* npm ERR! code MODULE_NOT_FOUND 
* npm ERR! Cannot find module 'internal/fs'

这个就郁闷了，连npm -v都会报错：
```
npm update check failed 
Try running with sudo or get access          
to the local update config store via   
sudo chown -R $USER:$(id -gn $USER) /Users/myname/.config
```
执行了上面最后一句后，npm -v不报错了，然而npm install任何报错：
* npm ERR! code MODULE_NOT_FOUND 
* npm ERR! Cannot find module 'internal/fs'
还是那俩句


用n选择node低版本：
> sudo n 6.9.1

> sudo npm -g install npm@next

> sudo n stable

仿佛好了，但提示：
* re-evaluating native module sources is not supported. If you are using the graceful-fs module, please update it to a more recent version..

太老看来也不行，改用node7.6版本:
> sudo n 7.6

然后就又回到了npm install任何东西报错：
* npm ERR! code MODULE_NOT_FOUND 
* npm ERR! Cannot find module 'internal/fs'
还是那俩句

网上说：
> npm cache clean

然而执行这句都报错，加-f强制执行，会警告：
* using --force I sure hope you know what you are doing

当然加不加-f,报错还是一样。

怎么办？版本更换过了，缓存清理不能，npm install任何报错，node运行本身正常


在StackOverflow上找到一个这句：
> curl -0 -L https://npmjs.org/install.sh | sudo sh

不管了，放进去，半天提示worked。

然而并没有，输入npm anything...报错不同了而已：

* No such file or directory 

又在博客上找到一个整ubuntu服务器的哥们，他也遇到一样的问题，他用手动软链接：

> find / -name "npm-cli.js"

依样葫芦，找到位置：
```
usr/local/Cellar/node/8.1.2/libexec/lib/node_modules/npm/bin/npm-cli.js
/usr/local/lib/node_modules/cnpm/node_modules/npm/bin/npm-cli.js
/usr/local/lib/node_modules/cordova/node_modules/cordova-lib/node_modules/npm/bin/npm-cli.js
/usr/local/lib/node_modules/ionic/node_modules/ionic-app-lib/node_modules/ionic-cordova-lib/node_modules/npm/bin/npm-cli.js
/usr/local/lib/node_modules/ionic/node_modules/npm/bin/npm-cli.js
/usr/local/n/versions/node/6.9.1/lib/node_modules/npm/bin/npm-cli.js
/usr/local/n/versions/node/7.6.0/lib/node_modules/npm/bin/npm-cli.js
/usr/local/n/versions/node/8.4.0/lib/node_modules/npm/bin/npm-cli.js
```

三个主要的版本
```
/usr/local/n/versions/node/6.9.1/lib/node_modules/npm/bin/npm-cli.js -v
3.10.8
/usr/local/n/versions/node/7.6.0/lib/node_modules/npm/bin/npm-cli.js -v
4.1.2
/usr/local/n/versions/node/8.4.0/lib/node_modules/npm/bin/npm-cli.js -v
5.3.0
```

然后做一个软链接

> sudo ln -s /usr/local/n/versions/node/8.4.0/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm

然而提示：
* File exists

再跑到/usr/local/bin/目录下，把npm这个文件给干掉，再次执行ln -s....

成功了。

npm -v 显示3.10.8，就是链接的node6.9.1下的npm-cli.js
npm install xx 测试下，也成功了。

还不满意，要升级这个npm到@latest, 因为5.2增加了npx包执行器。

失败，噼里啪啦一堆，还有句：
* ENOENT: no such file or directory, access '/usr/local/lib/node_modules/npm'

算了，直接软链到node8.4下那个npm-cli.js(5.3)试试，还是老规矩，先删除/usr/local/bin/下的npm文件，再ln -s....

如果不行, 提示:
* checkPermissions Missing write access to /usr/local/lib/node_modules/npm

再删除/usr/local/lib/node_modules/npm, 再npm i npm@latest -g

npm -v成功是新版本了，再一样搞定npx。

看来是升级node或npm的过程中哪步搞错了（加了不该加的sudo？）链接关系都丢失了，path找不到

再试试在任意目录npm install xx,没毛病（）分享者说他还要执行下npm clean cache，我木有这步）


网上后来看到这样关联路径,没试
```
export NODE_PATH=/usr/local/lib/node_modules/  
echo $NODE_PATH 
```


也考虑重整整个node和npm

原来老版本的全局安装的先备份下usr/local/lib/node_module

尝试用nvm来做node版本管理，homebrew install nvm 报错
``
Error: Could not link:
/usr/local/share/man/man1/brew.1

Please delete these paths and run `brew update`.
Error: Could not link:
/usr/local/share/doc/homebrew

Please delete these paths and run `brew update`.
Error: Your Xcode (7.0) is too outdated.
Please update to Xcode 8.3.3 (or delete it).
Xcode can be updated from the App Store.
``
看来xcode好久没升级了


---
```
Error: Cannot find module 'internal/fs'
如果运行bower或gulp等出现这个错误, npm cache clean后, 重装bower或gulp即可
一般是因为node版本高了, 所以装新版的bower或gulp
```