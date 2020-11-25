```
nuxt , node的一些配置和调试

推荐的安装方法

一、卸载已安装到全局的 node/npm
如果之前是在官网下载的 node 安装包，运行后会自动安装在全局目录，其中
node 命令在 /usr/local/bin/node ，npm 命令在全局 node_modules 目录中，具体路径为 /usr/local/lib/node_modules/npm
安装 nvm 之后最好先删除下已安装的 node 和全局 node 模块：
npm ls -g --depth=0 #查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node 版本重新进行全局安装

sudo rm -rf /usr/local/lib/node_modules #删除全局 node_modules 目录
sudo rm /usr/local/bin/node #删除 node
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm #删除全局 node 模块注册的软链


二、安装 nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
安装完成后请重新打开终端环境，Mac 下推荐使用 oh-my-zsh 代替默认的 bash shell。 安装完成后,发现使用nvm install stable 安装node速度很慢,原因嘛,大概大家都知道我大天朝的国情。 接下来介绍如何使用国内镜像快速安装node: 把环境变量 NVM_NODEJS_ORG_MIRROR, 那么我建议你加入到 .bash_profile 文件中:
# nvm
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
然后你可以继续非常方便地安装各个版本的 node 了, 你可以查看一下你当前已经安装的版本:
$ nvm ls
         nvm
     v0.8.26
    v0.10.26
    v0.11.11
->  v4.3.2


可能需要激活nvm:
$ source ~/.nvm/nvm.sh



安装完后，如果是用xshell连远程主机的话，先重连一次，不然会发现提示找不到nvm命令
可能出现依旧提示找不到nvm命令，那么请使用source命令，如下
source ~/.bashrc

nvm install v7.9.0  #命令后加版本号就可以进行安装，字母v可以不写，如下图
nvm use v7.8.0
nvm current


npm i node@lts
npx node@4 myscript.js
~/v/enuxt> node -v
v8.4.0

~/v/enuxt> npm run dev

> enuxt@1.0.0 dev /Users/evans/vue/enuxt
> nuxt




 INFO  Building project

✔ success Builder initialized
✔ success Nuxt files generated


 ERROR  Failed to compile with 1 errors                                                                                                                                                                                                                         14:48:54

This dependency was not found:

* /Users/evans/vue/enuxt/.nuxt/client.js in multi webpack-hot-middleware/client?name=client&reload=true&timeout=30000&path=/__webpack_hmr ./.nuxt/client.js

To install it, you can run: npm install --save /Users/evans/vue/enuxt/.nuxt/client.js
events.js:182
      throw er; // Unhandled 'error' event
      ^

Error: getaddrinfo ENOTFOUND localhost
    at errnoException (dns.js:53:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:95:26)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! enuxt@1.0.0 dev: `nuxt`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the enuxt@1.0.0 dev script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

```