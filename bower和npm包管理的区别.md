# bower和npm包管理的区别

> 不对bower和npm做基础介绍.

<br>

### 都有dependencies，但bower有resolutions, npm没有.
原因：
<p> <b>npm的包是可以嵌套的</b>. npm的包查找机制, 是从当前项目node_modules目录, 再到上层项目node_modules目录, 最后到全局目录查找.
<p> <b>bower是扁平的</b>. 所以它无法像npm用嵌套处理冲突. 所以需要制定resolutions来制定冲突时采用的版本.

```
    // 举个例子:
    dependencies:{
        "a":...,
        "b":...
    }
    // 其中a、b也有自己的dependencies。
```

假设a依赖c的1.0以上版本，b依赖c的1.2以上版本，那么没问题，npm可以和bower一样扁平安装a、b、c@1.2。

假设a依赖c的1.0版本，b依赖c的1.2版本，那么npm不能扁平安装，但依然用子项目各自安装就解决这个问题。而bower就无解了，它需要你通过resolutions告诉它采用哪个版本。

 如果你没有，那么在本地会跳出命令行对话框让选择，而在服务器构建上就会直接报错：“找不到合适的版本”


<br><br><br>

###  都支持semver，但bower支持分支名简写，npm不支持
原因：
<p>bower服务器并没有直接存储项目，而是存储项目地址（往往是github或gitlab）。所以当只写分支名的时候，就自动找到该项目的地址#分支，和版本号处理一致。

```
  // 在bower私服中登记punch对应的地址:
  https://oauth2:sZJLWsAUhfYyhKtEdYbP@g.hz.netease.com/punch.git

  // 在bower.json中就可以直接引用:
  "punch":"myBranch"
```

<p>npm不存在分支。因此如果要使用某个库的分支，必须指定协议全路径#分支, 比如:

```
git+ssh://git@g.hz.netease.com:1024/eds/punch.git#mybranch
```
这样写全路径的话，就跟私服没有关系了（每一项的地址都必须写全，除非用命令行工具做占位符替换，比如在package中登记gitLabUrl，用@做占位符替换，但这样具体的目录层级还是要手写，除非也全部登记在配置文件，这样私服就完全没有了意义）。

<br><br><br>

### 都支持GitHub URLs，但bower的实现有bug
原因：
<p>bower通过识别字段中是否有斜杠来支持github的 username/modulename 的写法，#后边可以加后缀写明分支hash或tag。（这里的tag不仅仅是版本号，可以是具体commit的id）

<p>因为bower支持分支名，所以分支名中带斜杠就会误解为GitHub URL，导致问题。npm不支持分支名无此问题，全路径带斜杠但可以通过检测是否有协议头避开。

```
   // bower.json中可以通过#开头来指明这是一个分支或tag, 不是Github URL
  "dependencies": {
        "nej": "0.5.1",
        "regularjs": "#0.5.1",
        "res-base": "#feature/eduos_punch_zhh_20181020",
   
   // 然而resolutions时, 因为模块地址已经确定, 所以这里直接理解为分支或tag名, 不加#, 加了反而报错(连replace(/^#/,''))这步都没做
       "resolutions": {
        "nej": "0.5.1",
        "regularjs": "0.5.1",
        "cache-base": "feature/eduos_punch_zhh_20181020",         
```

所以bower中的项目分支最好不要加/斜杠, 而用中划线-来代替。这样就没有#号的问题了。

<br><br><br>

### npm是一个仓库，bower只是一个仓库映射
npm官服或私服都是代码仓库，在publish的时候，通过升级package.json的版本号来自动生成versionTag。而bower服务器只是一个依赖管理的映射。它只登记别名和其他代码仓库的联系。 官服和私服都是做这个事。GitHub上，有项目专门按bower路径格式给bower项目建立目录。比如: https://github.com/mbostock-bower/d3-bower

<p>但是开发工程依赖分支的话, bower+GitLab这种方式更好用。因为我们无法也不该将分支上传到npm服务器上。 


<br><br><br>

### 如果不依赖分支，只依赖版本号，会怎样？
那么我们的组件库就必须非常稳定。

<p>严格来说，这是一种正统的做法。业务开发需要在组件满足业务要求升级测试打tag完成后，再进行。因为这时候才可以依赖引用。

<p>而那些业务模块，比如module-cps则完全无法同步与工程开发。这种子工程的问题，原来是用subTree或subModule来解决的，但不得不说，这种subGitPrj的方式还是很多麻烦。依赖分支来同步进行业务模块的开发，分离得更彻底，也更容易实现。
