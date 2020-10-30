# 用rollup打包vue

> rollup更适合库和组件的打包. webpack更适合整个web项目的打包.

```
rollup更纯粹,就是将模块化的代码通过插件编译整合输出为js.

webpack更综合, extractPlugin,chunkPlugin,publicPath这样的提取配置,index.html模板的编译整合标签插入输出, 都是从整个前端项目角度设计的.

新起项目, 用vue-cli,内含配置好的webpack,从组件到index一路俱全.
老项目用vue, 需将vue组件打包为一个单独可引用的模块, 用rollup.
```
<br><br>

> 苦恼由来已久

```
在browserify时代, 就苦恼于寻找项目局部使用es6的方案. 最终用babelify打包部分目录打包, 整合在其他gulp任务中实现.

如果你的项目不是整个从新技术栈设计的, 就要面对这种手动整配件的事情.

es6普及起来以后,逢人便是webpack,用babel的绝大部分都是讨论webpack配置和loader.关于独立编译的讨论,很少.
```

```
很多情况下, 我们只是需要一个工具:将一个个模块结构化的code文件组装起来,得到一个iife的单文件js, 顺便把code从高版本转到兼容版本.

browserify很强大, 更贴近gulp的使用方式, 但rollup更好用.前者是编码式的,后者是配置式的. 而且因为专注少, 配置比webpack少太多.
```

```
还有, browserify的目标就是编译为browser可用. 其实有些情况,如node下,我们并不是要把模块化的代码编译为browser可用,而仅仅是将一些环境尚未支持的新特性转换一下.

把典型的  
"presets": ["es2015"]
  改为:
"plugins": ["transform-es2015-modules-commonjs"]

比如如上, 就把es-module的方式变异为commonjs的方式就可以了.其他class和箭头函数等等都可以保留. 文件也不需要合并.
```

```
new Vue有template属性, 样式也可以通过createStyleTag实现, 所以用一个js来引入html和css(通过将html,css识别为文本模块的插件)完全可以解决.

然而,rollup-vue-plugin可以识别.vue单文件组件这种形式, 那就更赞了,更贴近vue工程的开发方式.
```

<br><br>

> 然后就碰到了一些坑

```
首先rollup-vue-plugin官网上的demo并不能跑. 因为vue-compile中间使用了module.exports=而不是es6方案的export default, 导致rollup找不到export报错.


所以这就需要安装rollup-commonjs-plugin. 接着还会遇到
Plugin/Preset files are not allowed to export objects，not function
还有Parse error等等莫名的报错,你会发现安装的babel插件似乎有点兼容问题, 你需要将.babelrc配置成如下这样的bebel7新魔符:
 "presets": [
        "@babel/preset-env",
        "@babel/preset-XXX"
    ]
 
还会莫名各种装完rollup-vue-plugin后找不到vue-template-compiler的报错, 或者在装rollup-babel-plugin后又报找不到babel-xx的错.
在你补装之后, 最好再重装一遍rollup-vue-plugin和rollup-babel-plugin

还有,如果你最后导出的模块是iife形式的, 入口主文件不能有export,否则你需要准备一个name为全局变量名, 以便rollup将你的export转换为var myModule=(iife)的形式
```

<br><br>

> 还要写个插件, 最后把生成的iife方式, 转为我们特有的模块提供方式,NEJ.define...
```
两个思路, 先不写, 最后用NEJ.define包裹;
先写NEJ.define, 然后提取并替换到顶部. 这时候顶部有个全局变量=iife()也是极好的, 方便定位.
```

<br><br>

> 我们不是不想用webpack, 只是我们的工程是老工程, 要接入新模块, 所以我们需要rollup. 不要再只关注webpack了, fuck :)
