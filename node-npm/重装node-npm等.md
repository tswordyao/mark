# 重装node npm nvm
```

跑这条命令删除node所有
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}


依次跑这四条命令安装nvm
sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
[[ -s ~/.nvm/nvm.sh ]] && . ~/.nvm/nvm.sh


卸载后用nvm安装node
nvm install stable   //安装最新版 node
nvm install [node版本号]   //安装指定版本的node
nvm use [node版本号]   //切换到指定版本的node
nvm alias default [node版本号] //设置默认版本

详细介绍https://www.jianshu.com/p/ac6e4397c9f0

sudo npm i nrm -g
nrm ls是列出来现在已经配置好的所有的原地址
nrm use是切换到哪个源上
nrm add添加源
nrm del删除源
nrm test测试源的响应时间，可以作为使用哪个源的参考

有时候安装了gulp等全局不认识 unknown command
npm config set prefix /usr/local

gulp项目中使用全局
解决方法： 
第1步：先cd到当前目录中，如果是webstorm打开Terminal默认就是当前项目，使用以下命令，回车即可：npm link gulp 
第2步：配置node相关的环境变量，即node_modules的安装目录：使用以下命令：npm root -g 获得安装路径，然后 vim ~/.bash_profile 添加两行： 

NODE_PATH=/usr/local/lib/node_modules 
export NODE_PATH 


npm config list
点击查看 userconfig /Users/evans/.npmrc
.npmrc中有prefix=   如果之前乱设过的话, 删掉这行

npm root -g 显示正常
/usr/local/lib/node_modules

网上也有人说这样:
$ npm config delete prefix 
$ npm config set prefix $NVM_DIR/versions/node/v8.11.2
没试过
```