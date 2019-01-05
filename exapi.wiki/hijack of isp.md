### 增加nonce字段验证
CSP level2开始，script-src的限制，除了https这样的协议限制，又增加了nonce标识符识别。比如：
```
<meta http-equiv="Content-Security-Policy" content="script-src https: 'nonce=H15192';">

<script nonce="H15192">
    //inline code
</script>
```
如上可以限制只有对应nonce的内联脚本才可以执行。
> 但是这个有兼容性问题。不只是低版本不生效而已，而是因为没有放行unsafe-inline，低版本又识别不了nonce，会导致低版本浏览器中所有内联脚本都无法执行的情况。

所以，使用nonce对指定的内联脚本放行，必须要服务端检测userAgent，满足要求的浏览器加上nonce，否则添加unsafe-inline的允许。

<br><hr><br>

### 开启Subresource Integrity验证
```
<script src="https://cdn.com/js/frameworks.js" integrity="sha256-+Ec97OckLaaBV8DyYYVpE=" ></script>
```
> 被请求的资源必须同域，或者配置了 Access-Control-Allow-Origin 响应

SRI这个属性较新，客户端支持情况是：IOS11默认不开启，Android5+。
但不支持也不会影响脚本运行，至少我们能让2016年后的Android系统用户受益，从有胜于无的角度，这也是一个不错的比例了。
<br>  
但是，我们是否真的要启用这个验证，却又要打个问号了。因为情况很特殊，我们面对的网络运营商，他们往我们的脚本里是添加广告代码，而并不是恶意攻击或执行不安全的绝对无法容忍的操作。而我们一旦开启了这个验证，那么运营商插入广告后，我们的整个脚本就无法执行了。在无法运行和容忍广告之间，我们又会选择后者。

<br><hr><br>
### 重写document.write
好吧，让我们再回头看看运营商往我们返回的js内容里加入了什么东西。以及它是怎么加的：
```
如果你请求的是 http://myweb/bus.js
变成
document.write('<script src="https://myweb/bus.js"></script>')
/*......*/
document.write('<script src="http://isp/adv7788.js"></script>').
```
嘿嘿，它通过document.write标签，在原来的脚本基础上，又夹杂了它的私货脚本。至于它为什么没有直接上具体功能代码，而是加入一个script引用，应该是频繁改写数据包是不方便的事情，而它频繁更改维护那个私货就可以了，更方便。

为什么不用appendChild呢？ 因为此时文档流还没有关闭，document.write输出script标签是同步的，因此可以保持上下脚本的依赖关系。如果定义了defer或async属性的tag，异步加载回来文档流已经关闭，这时再write就会出现白屏，也就是document.body被重写。在高版本浏览器里，异步加载回来的script里进行document.writ会默认无效并控制台出警告。这会导致我们的脚本和运营商或第三方（比如360的统计）的脚本一起失效。



点解。那么，我们可以移除document.write这个api，内部偶尔要使用的地方，改为使用一个私定义的_write方法。这个操作有一个麻烦，就是外部的一些第三方推广统计脚本，也让人无语地用到了document.write，也用这个来导入他们的脚本引用。如网页引入的360脚本 http://js.passport.qihucdn.com/11.0.1.js 中，它会像运营商插入脚本的方式一样，来这么一招：
```
document.write('<script charset="utf-8" src="http://s6.qhres.com/static/ab77b6ea7f3fbf79.js"></script>')
```
让人无语，360为什么不直接让我们引入后面那个js呢？非要这么绕一下。经过我的调试，发现和网上吐槽的一样，起作用的仅仅是后面那个js而已，后面那个js对location做了编码，通过创建img向统计站点发了一个get请求。

相对来说，百度的统计脚本，http://push.zhanzhang.baidu.com/push.js 就好很多，里面就直接是功能代码，没有document.write再绕一圈。

anyway，第三方代码的更新不归我们管辖，我们去掉了document.write这个api，可能会留下日后某个第三方统计脚本无效的隐患。

这点问题，很容易解决。我们可以利用保留原api引用，再重写document.write的方式实现：
```
document._write=document.write;
document.write=function(str){
   if(str.indexOf('white-list-mark')>-1){
      document._write('haha');
   }
}
```
如上，就达到了目的。这一招没有浏览器限制，针对性效果更好。