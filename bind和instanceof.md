## bind最主要是指定this,本身不会引起instanceof的bug
bind的核心是返回一个Function, 该Function始终apply(bind时指定的this，还有其他预定参数)。就这样。

<br> 


## 但有的时候bind产出的函数不使用指定的this
执行的时候就会无条件采用bind时指定的this----除了new----被当做构造函数时会有不同的处理。构造函数执行时，js解释器会先生成一个{},然后给它与构造函数的prototype建立好联系，再用this传给构造函数---也就是此时已经原生做了某些工作了。这个this，即使是bind产出的函数，也是不会扔的。所以这个时候，bind出来的方法不使用指定的this

<br> 


## 问题就在于bind创建后的方法执行时,判断是否被作为构造函数的时候
它会判断此时是否在作为构造函数执行---通过this instanceof fn来判断. 这里的fn是一个跟bind前的原始函数做了原型关联的空函数, 同时bind产出的函数, 也使用此函数的实例作为prototype, 实现三者的原型关联。
```
fnoop.prototype = bind原始函数.prototype
bind产出函数.prototype = new fnoop()
```
> 扩展问题: 为什么不直接让 bind产出函数.prototype = bind原始函数.prototype? 或其实例? 
```
前者交出原型引用，可能会导致一损俱损的不稳定变化
后者使用前者的实例，但前者并非一个空函数，产生的实例经过了构造函数的处理，已经不纯粹，更会和bind产生后的函数构造时处理冲突。
```
   
<br> 

## instanceof 判断的实现规范是用Function[Symbol.hasInstance]来比较
按照规范，需要比较前者的proto是否是后者的prototype， 当后者的prototype是undefined的时候，报typeError
 
<br> 

## 什么时候function.prototype会为undefined？
几乎所有的非原生构造函数的Function。也就是说，在native code内置的Function中，只有构造函数的Array、Function、String这些是有prototype的，其他如数组的slice方法、string的replace方法，都是不存在prototype的。
 
<br> 

## 谁会让原生函数或无prototype的函数执行bind?
es5shim. 某些库在内部操作时，会将slice，replace等方法简化缓存起来。比如:
```
Array.prototype.slice
// 会用
var slice = Array.prototype.slice;
// 然后
slice.call(arrLikelist)
// 更进一步
var slice = Function.prototype.call.bind(Array.prototype.slice);
// 然后直接
slice(arrLikelist)
```
<br>

## 悲剧是怎样衍生的？
库里面使用bind往往又是为了生成一些基本的方法。比如es5shim是在用在Array.isArray，当Array.isArray已经被polyfill，其他的库就会跳过它---就像错误的bind的polyfill写在前面就跳过了后面库里正确的bind实现一样。然后业务或框架代码跑起来的时候，就毫无防备地去使用这个被玩坏的bind玩坏的isArray，然后就在instanceof这一步全面爆发，block代码，所有都凉凉....

<br> 

## 如何判断是否具有原生的bind方法
```
Function.prototype.hasOwnproperty('bind')
&&
!Function.prototype.propertyIsEnumerable('bind')
// 因为es3不支持defineProperty, 所以无法模拟不可枚举属性
```

> es5shim中关于bind的polyfill是比较标准的。bind如果除了this还预传了参数，会改变其length。es5shim用很绕的办法的实现了一遍。（新建了一个函数并模拟形参传入）

> bind后的函数是[native code], 无法再通过toString解析函数字符串