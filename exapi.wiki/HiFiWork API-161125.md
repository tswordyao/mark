# HiFIWork(HiSign FrameWork For Front-End)-API


## 约定术语

>str=字符串， bol=布尔值， obj=键值对象， arr=数组， fn=方法， cls=class

>cb=回调， url=网址， src=资源路径， act=接口地址， link=链接地址， win=窗口或弹窗

>ele＝元素， eve＝事件， err＝错误， 

>res=后台返回的整个对象， res.flag=是否成功的标志， res.totalCount=数据条数， res.data=数据主体， res.msg=后台返回的报错信息

> div class="query-block" 查询表单区域包裹容器， .query-result 查询结果区域包裹容器，  .new-color-bar 查询结果区域标题栏

<br/>


## 以下为全局可引用的变量及方法: 
>Number/String width 表示宽度参数可以是数字或字符串，  [ ]内表述可选参数
  
<br/> 


###  tomcat服务目录
```
String path
```

### 获取本地资源路径
```
getDistPath()
return String distPath
```

### 获取本地view路径(所有.html文件在此下级)
```
getViewPath(String pageFileName)
return String viewPath
```

### 获取ajax服务接口
```
makeAct(String actionKey,[String/Number needApiLog]) //需要记入日志的第二参数传1，不需要的传0，/api/后面直接跟aciton的传''
return String action
```

### 获取直接赋值给location的服务接口
```
act2link(String action)
return String actionLink
```

### 获取元素高宽及位置
```
getRect(ele)
return Object {top:Number， left:Number， bottom:Number， right:Number， width:Number， height:Number}
```

### 获取原生element
```
byid(String id， [Element parentEleOrDoc])
return navtiveEle
```
_byclass／bytag同理_
<br>

### json字符串转对象
```
str2obj(String str)
```

### json对象转字符串
```
obj2str(Object/Array obj)
```

### 解析querystring字符串中的参数
```
queryParse([String queryStr])
return Object{key:value,key2:value2,.....}
```

### 引入脚本及样式
```
importing([String pluginKey，...String pluginKeyN]，  Function cb， [Boolean doExist])
```

### 显示隐藏laoding遮罩
```
showLoading()/hideLoading()
```

### 吐司消息
```
toast(String msg，[Number holdtime]，[Function cb]) _可链式续接 .ok()/.err()/.warn() ，便加上对应icon_
return $ele **返回的$ele可链式续接.css()方法， 以手动更改提示框的宽度，适配文字不刚好换行**	
```


### 确认框
```
$alert(String str，[Function cb])
return $ele		
```


### 确认框2
```
$confirm(String str，Function cb(Boolean ifYes))
return $ele	
```


### 弹窗
```
$open(String id/url， Object{
		[width]:Number/String，
		[height]:Number/String，
		[modal]:Boolean，
		[onClose]:Function
	}，[Boolean needAjaxLoad, Function cb]) 
return $ele
```
_弹窗内容由第一个参数决定， 如为需ajax加载的片段html文件，needAjaxLoad设为true, 并准备cb_
_常用举例：$open(‘#editWin’,&nbsp;config);_
<br>


### 弹窗中的元素获取打开该弹窗的父window
```
window $(openwin).$opener **一般只在打开iframe弹窗中使用， 因为普通弹窗元素和其他元素同处一个window中**
```


### 弹窗关闭
```
$(selector).$close()
/* var win=$open();保存返回的弹窗元素
 * ### ....
 * win.$close();
 */
```
<br>
### 接口交互
```
$get/$post(String url，Object data/null，Function cb(Object res),[Boolean jsonStrWrap]);
return Object promise
```
 
### 模板渲染 
```
$(selector).template(Object/Array data， [Function helper(Object item， [Number index])]，[allowHTML])
return $ele
```

### 模板字符串
```
$compile(String templateStr，Object/Array，[Function helper(Object item， [Number index])]，[allowHTML])
return String compiledHtmlStr
/*
 * template的本质就是将指定的字符串和数据，用$compile编译为要展现的字符串， 赋值给发动的元素
 * template除了少传一个templateStr， 其余传参和$compile一致
 *
 * template少传一个templateStr是因为， 发动template的元素上，通过tpsource指定了模板字符串的位置，如:
 *
 * <div id="case-list" tpsource="#case-list-tp"></div>
 *
 * <script id="case-list-tp" type="text/template">
 *	 <p>{caseName}</p>
 *   <a href="/server/casedetail?id={caseId}">案件详情</a>
 * </script>
 *
 */
```


### 过滤器生成
```
$filter(String filterName， Function filter([Object runTimeItem，Number runTimeIndex])， [Class constructor])
```
<code>
 /*
 * 指定一个过滤器名，以便在template字符串方便转换， 当然在原生js中也能被使用，只是需要加上括号，如:
 *
 * 定义: $filter("asCnYes"， function(){ return this==true?"是":"否";}，Boolean);
 *
 * 模板中使用: <b>{checked.asCnYes}</b>
 *
 * 原生js中用: var checked=false; alert(checked.asCnYes()); 
 */
 </code>
 
 <br>

### 常用扩展
```
JSON.equal(Object/Array obj， Object/Array obj2) ### ==和===无法判断{a:1，b:2}是否和{a:1，b:2}相等，因此扩展
return Boolean isEqual
```

### 字符串扩展
```
str.format(val，val2......) ### str='my name is{0}， my age is{1}'; str=str.format('张三'，18);
return String newStr;
```

### 数字扩展
```
num.next()/num.prev() ### 数字的上一个和下一个， 一般用于模板中 <b>序号:{index.next}</b>
```



### 数组扩展 去重
```
arr.distinct([useJsonEqual]) 
return newArr
```

### 数组扩展 过滤
```
arr.where(Function/str filter) 
return newArr
/*
 * var arr=[{a:1} ， {a:2}， {a:3}];
 *
 * arr=arr.where('item => item.a>1')  //where条件字符串中不可代入局部变量
 *
 * 得到arr为:[{a:2}， {a:3}]
 *
 */
```

### 数组扩展 生成
```
arr.select(Function/str filter) 
return newArr
/*
 * 实例1:
 * var arr=[{a:1} ， {a:2}， {a:3}];
 *
 * arr=arr.select( 'item => {a:item.a， b:item.a>1}' ); 
 *
 * 得到arr为:[{a:1，b:false}， {a:2，b:true}， {a:3，b:true}]
 *
 *
 * 实例2:
 * var arr=[{a:1，b:false}， {a:2，b:true}， {a:3，b:true}];
 *
 * arr=arr.select( 'item => {a:item.a}' ); 
 *
 * 得到arr为:[{a:1} ， {a:2}， {a:3}]
 *
 *
 * (item是可以随意取名的，指代数组中被遍历的每一项)
 */
```

### 数组扩展 遍历
```
arr.each(function(item，i){})
```

### 本地存储(不分模块划分,可存对象)
```
localData.get(String key)
return Object/Array/String/Number/Boolean/Date

localData.set(String key,anyObject val)
return true
```

### 本地参数(分模块划分,不可存对象)
```
设置:
localParams.sys.testprama='test'
localParams.lvmark.testprama='test2'

获取:
localData.sys.testprama
localParams.lvmark.testprama
```


