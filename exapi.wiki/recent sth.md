## EXT今何在? 不见当年YUI.

前端界最近又是风起云涌. 
```
一两个月前,facebook发布了一个防竞争性的许可协议, 类似react这样它出的'开源'产品, 如果第三方要使用, 不可以用来跟facaebook竞争.

这就很扯淡了. 竞争这个词可以被洗地人说得轻描淡写,也可以无限延伸. facebook这么大, 做的东西又不偏门, 谁能不保证做大了就和它有竞争呢?

于是Apache下面的开源软件被要求都去facebook化,也就是不能使用涉及facebook该协议的产品或技术.

随后,wordpress也声明放弃使用react.

今天,看到百度内部也在安排用半年时间改写用react的产品, 新产品当然也不再使用react.
```

```
angular去掉js也已经一个多月了(官方撤掉了关于js的文档,被视为全面默认ts), 不知不觉angular4发布都小半年了. 

当初4月1日看到infoQ推送的时候,还以为是愚人节的玩笑. 毕竟angular2这样对1的拥趸如此绝情的断代升级才刚刚结束.

步子走得太快, 蛋都跟不上了.
```

```
Vue正当时. 

论上手,其实react的jsx也没有什么难度,跟vue的模板一样简单. 然而搭建react全家桶之路, 其实是比较复杂善变的.

单向数据流的管理方案, 从简单的flux到redux到modx什么的, 反而说明了对于单向数据流这条路(业务逻辑足够复杂下), react根本没有想好怎么走.

另外样式相关复杂到一定情况下, 各大框架类库都开始考虑除了less,sass预编译之外的作用域管理方案. 同时也是为了组件程序化,all in js嘛, 想要摆脱一个组件必须有对异步css文件或动态生成dom style的依赖.

这方面css context, css obj的方案, react生态上也有不少轮子有待统一, 整个前端圈也是.

Vue,一个scoped属性,给组件内各元素生产data-v-编号,结合:not选择器, 非常优雅简单明了地实现了组件的内部样式作用域.

```

* 当然也可以只给组件根节点设置一个css-scope属性,声明在组件文件内的style如下生成:

> [css-scope="f8xa2"]  .red{color:red;}

或

> [widget="widgetName"] .red{color:red;}

```
其实选择angular对前端有好处,因为angular比较难, 做成了一个platform显得太重. 这样后端很难介入进来. 

否则像jquery那样, 随便一个后端他粗也能写, 但写完一段后续还是前端来整合,优化和维护, 那就恶心了. 非常不利于前端的价值体现和地位.

其实angular借鉴的后端框架思想,也是javaer发明的. 不过,这跟java没关系, 就是同样用java的安卓,也不是web后端的java开发说做就能做的. 
语言是基础工具, 是否熟悉开发框架及涉及的一套api和环境,周边插件,底层,工具,经验,理念,坑...wtf, 才是能否上手开发某套东西的真正的决定项.

所以angular虽然是java开发者整的,但反而一般java开发人员很难介入angular前端代码,反而最前端的jquery,后端喜闻乐见.
```

```
然而angular由于比较难又先流行, 忽悠了一波后玩断代升级. 
这个时候,后流行的react和vue就显得更流行, 重新学习更重也难的angular2/4就成了大家要顾虑的事情了. 

而且听起来angular是流行过了的东西了.

这就让angularJs1.x和angular2/4的同学都很尴尬.

前不久,也就一个月内,刚发生了angular官方聘用的中国推广者,与vue框架作者开喷的事情.
```

* 所以说前端故事多啊, 轮子反复造, 互相制造选择困难.

```
其实玩来玩去,不就是组件化么? 不就是数据键值表单绑定么? 不就是模板和路由么? 没有什么真正有编程乐趣和所谓高大上的东西.

这套东西,当年winform和webform被成为土炮C井都玩了不知道多少年了. 就像一个lambda goes to 被多少前端反复箭头函数的关注一样, 太搞笑了.

路由更是web服务器里最基本的了.
```

```
说白了还是前端技术js,css..没有前瞻到今天的发展规模, web GUI上产品的脑洞程度, 而自身还一直受限于web端的异步加载,网络流量不稳定, 浏览器安全沙盒, 先天不足后天补.

一个import一个require这种基本的导入, 又是es6又是node又是webpack又是wtf, 一个都发布了N久语言的标准支持还要靠babel这样的转译编码, 
babel还可以支持语言尚未标准化的特性, 事实上这个插件几乎成了一个语言标准,
而js这样本身的语言居然用上两个版本的es5来作为一种编译后代码的存在.

活久见,呵呵
```

```
babel除了es6,还要支持ts的编译. 因为ts支持es6, babel不反击只能像coffee一样被淘汰. 

真心可怜学coffee的同学又浪费了不少积累. 当然积累还是有的, 可是继承比呢 残值率呢? 这几乎是所有前端的痛处.

说到ts, ts绝对不是那么老实想要当个js的类型预编译版, 当然它一开始表现像这样. 

实际是当ts被大部分jser接受的时候, ts就要自己成为一门语言了.

谁规定ts一定就只能编译成js? 谁又规定浏览器或node v8或wtf里不能直接跑ts或ts编译后的其他代码呢?

君不见, 还有web assembly呢
```

* 其实顶层WEB标准圈的大佬们, 早就对js不满了, 无非是现在利用web广大的开发资源量优势, 不得已选择web, 选择js这样目前的前端技术,继续扩大web的战果.

```
es6这一步,里面真正复杂的一些概念,其实是把前端开发往真正的程序员方面引导了.

讲真,以前大部分前端真不算程序员. 多少年来谁操作过二进制?知道数据结构? 之前连ajax都没有, 就呵呵是一个word都能发布的超文本图文文档而已.

即使h5,es5(有了计算属性和只读属性等)后, 理论上可以操作二进制后,大多数界面端的js还是不算合格的程序员的,node另说.

当然不算真正合格的程序员，并不代表不是合格的员工,不是合格的开发. 因为前端本身就是一个ui页面实现者和交互逻辑编写者的混合的角色.

本来就是半个程序员嘛, 但同时另外半个也是有在干活的.
```

```
到了ts,到了web assembly,到了css scope的考虑,实际前端圈的程序员氛围已经起来了. 

随着kotalin这种还能编译成js的后端语言出现,swift也能啊,其实都能啊, 前后的界限会复古地再一次模糊.

另外现在AVR三大组件方案,实际是对jquery通过简洁灵巧攻占了EXT,YUI之流后,面对越来越大型,越来越复杂的需求后, 表现得难以管控的一次封装复辟.

只是这一波不再直接直接提供组件库,而是提供开发组件库的框架.

只懂一点js,css, 真的不行了.
```
* 以上说的是,如果你是找工作的话.

```
在职的,契合公司的技术路线,就是对的.
创业的,能把产品做出来的,就是好的.

前者更限于内部体制的技术选型,后者更实际于产品设计和业务.

讲真,大多数项目下,web前端技术,真的决定不了产品的生死.如果你是创业者,用啥实现了都一样.它甚至连技术核心竞争力也无关.

前端就是这样的地位,这就是它的定位. 前端开发者,要调正心态,好好达成实现,不给产品项目拖后腿,便是最大的成功.


```
