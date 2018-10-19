# mocha-headless-chrome 测试工具使用

官网地址: https://www.npmjs.com/package/mocha-headless-chrome

比较坑的一点,安装这个会依赖安装Google的puppeteer, 后者是模拟浏览器进行测试的核心.  

* 事实上, 它就是一个浏览器调用程序. 

它下载Chromium.app从而进行真正的浏览器调用. 显然chrome和puppetter都是Google开发的,所以给人感觉更'官方'. PhantomJS这些被盖过了风头.

* 公司的网络是可以连Google的,然而,还是下载Chromium失败.

所以只好手动下载Chromium.zip,解压后放置在node_modules/puppeteer/.local-chromium却始终提示找不到文件, 最后才之知道要建一个mac-564778的子目录, 再放置到子目录下.

* 坑爹! 而且记得sudo全局安装mocha-headless-chrome后, 手动打开一次chromium.app, 让mac系统设置提示打开权限.

```
完整路径为:
/usr/local/lib/node_modules/mocha-headless-chrome/node_modules/node_modules/puppeteer/.local-chromium/mac-564778/chrome-mac/Chromium.app
```

```
调用命令为:
"test": "gulp coverage && mocha-headless-chrome -f test/coverage.html -o test/coverage.json -a disable-web-security",
```

```
直接调用puppeteer:
async function foo() {
      const browser = await puppeteer.launch({
        executablePath:'./node_modules/puppeteer/Chromium.app',
        headless: false
      });
      const page = await browser.newPage();
      console.log(page)
      await page.goto('http://music.163.com/');
      await page.screenshot({path: 'music.png'});
      browser.close();
}
foo();
```

```
node6无法用async:
const puppeteer = require('puppeteer');
var bw,pg;
puppeteer.launch()
.then(b=>
  (bw=b) && b.newPage()
)
.then(p=>
  (pg=p) && p.goto('http://music.163.com/')
)
.then(it=>
  pg.screenshot({path: 'music.png'})
)
.then(it=>
  bw.close()
)
```


```
coverage.html:
<html>
<head>
    <meta charset="utf-8">
    <title>Unit Test - Button</title>
    <link href="../lib/mocha/mocha.css" rel="stylesheet" />
    <link href="../lib/res-base/css/style.css" rel="stylesheet" />
</head>
<body>
<div id="mocha"></div>
<div id="parent"></div>

<script src="../lib/chai/chai.js"></script>
<script src="../lib/mocha/mocha.js"></script>
<script> mocha.setup('bdd'); </script>

<script src="../lib/regularjs/dist/regular.js"></script>
<script src="../lib/nej/src/define.js?pool=../lib/&pro=./src/"></script>
<script src="test.js"></script>
</body>
</html>
```


```
test.js:
NEJ.define([
    './base/test.js',
    './util/test.js'
],function () {
    mocha.run();
});
```