# HiFIWork(HiSign FrameWork For Front-End)-API

HIFI2.0 global


## 一，目录结构
```
package.json(nw,node配置文件)
gulpfile.js(gulp任务配置文件)
data(编译前的配置等JSON数据)
script(编译前的脚本)
style(编译前的样式)
view(编译前的页面)
img(编译前的图片) 

dist(编译后的资源访问目录)
  dist>plugin(插件)
  dist>js(脚本)
  dist>css(样式)
  dist>font(字体)
  dist>view(页面)
  dist>img(图片)
  dist>mock(模拟数据)
  dist>login.html,index.html(登录页和主框架页)
```

  
<br/> 


## 二，引入方式
```
1,在mock中-menu.json或在系统管理模块添加页面；
2,在view目录下创建对应html或htm(分别对应iframe加载和单页加载)；
3,在页面头部引入base.css
4,在页面尾部引入base.js
5,引入该页面的脚本.js，按如下展开：

  //设置页面所属子系统
  setSubPrj('wz');

  //按需引入，完成后自动注入scope
  importing('依赖插件','依赖资源2',.....function(scope){

    //业务逻辑代码...

  });
```



## 三，结构及引入说明
```
打包部分：
编译前的script文件夹中所有默认扩展、函数、组件等，都用browserify打包为 base.js ，因此js基础只需一个base.js
编译前的style文件夹中所有的样式表，都通过less编译为 base.css, 因此样式基础只需引入一个base.css

未打包部分：
依赖插件——处于dist/plugin目录下，里面有base中未包含的各类插件，通过impotring函数按需加载；
业务脚本——处于dist/js目录下，里面按页面模块划分各个js文件，通过页面script标签引入；
业务样式——处于dist/css目录下，里面按页面模块划分各个css文件，通过页面link标签或业务脚本中importing引入；

（如果是单页模式的新页面，无需引入base，index页面已包含。）
```


## 四，原型扩展
```
JSON.equal
JSON.clone

Number.prototype.next
Number.prototype.prev
Number.prototype.toStr

String.symbol
String.prototype.format
String.prototype.isEmpty
String.prototype.lowEqual
String.prototype.valAt

Date.now
Date.format
Date.getDayAs
Date.prototype.format
Date.prototype.getDayAs
Date.prototype.addMonth

Array.prototype.each
Array.prototype.fire
Array.prototype.where
Array.prototype.select
Array.prototype.distinct
Array.prototype.orderby
Array.prototype.indexAs

（filter.js中定义的系列）
```


## 五，全局扩展（对象）
```
配置对象
config

DOM操作：
jQuery2.2

cookie操作
$.cookie

本地存储管理
localData

本地参数管理
localPramas

模块变量管理
top.registry

导航路由器
$state

作用域绑定对象(以上都可全局引用，但scope对象只会以importing回调函数的参数形式自动注入)
$scope

```

## 六，全局扩展（方法）1

```
设定当前页面所属子系统
setSubPrj

依赖资源加载器
importing

静态资源获取器：
$source 

部件定义器
$widget

行为定义器
$behavior

过滤器定义器／原型扩展
$filter

析值定义器
$extractor

赋值定义器
$injector

重置定义器
$resetter

模版编译器
$compile
```



## 七，全局扩展（方法）2
```
真实类型判断
typeOf

网址参数解析
queryParse

接口定义器
makeAct

接口交互：
$post/$get/$ajax

异步promise
$will

单例promise
$try

确认框
$alert/$confirm

弹窗：
$open

吐司消息
toast

加载loading
showLoading/hideLoading

语法糖 获取元素
byid byName

语法糖 JSON转换
obj2str str2obj

```




## 八，jQuery原型扩展
```
模版实例化
template

获取及设定元素的数据
xData

设定析值方法
extractor

设定赋值方法
injector

设定重设方法
resetter

手动绑定scope
bind2

手动初始化部件
widget

手动初始化行为
behavior

(plugin中各类插件对jQuery原型的扩展另计)
```

## 九，DOM 属性扩展
```
自定绑定声明(只写在<html>上即可)
x-app

动态数据模版（单向）
x-tp

动态列表（单向）
x-list

表格（单向）
x-table

树（单向）
x-tree

静态数据模版（单向）
x-map

静态数据模版 下级属性元素（单向）
x-key

表单（双向）
x-form

表单控件（双向）
x-name

条件隐藏
x-hide

条件显示
x-show

远程数据映射
x-act

局部页面引用
include

局部页面替换
replace

组件声明
widget

行为声明
behavior
```


## 十，插件扩展
```
"plugins":{
	//行内select选择
    "inline-select":"selects/inline-select",
	//地图
    "hsmap":"hsmap/hsmap.js",
	//浏览器全屏
    "fullScreen":"fullscreen/jquery.fullscreen.js",
	//自动完成智能补全
    "autocomplete":"autocomplete/autocomplete.js",
	//地图坐标选取
    "mappicker":"mappicker/mappicker.js",
	//时间日期选取
    "datepicker":"date/my97/datepicker.js",
	//树
    "xtree":"ztree/ztree.js",
    "ztree":"ztree/ztree.js",
	//图表
    "echarts":"echarts/echarts3110.js",
    "echarts2":"echarts/echarts225.js",
    "echarts3":"echarts/echarts3110.js",
	//中国地图
    "china":"echarts/china.js",
	//表格
    "jqgrid":"jqgrid/jquery.jqGrid.min.js",
    "dataTable":"dataTable/jquery.dataTables.js",
    "fixDataTable":"dataTable/dataTables.fixedColumns.js",
	//字典控件
    "dict":"dict/dict.js",
	//websocket通信
    "socket":"socket/socket.io.js",
	//富文本编辑器
    "ckeditor":"ckeditor/ckeditor.js",
	//时间计算
    "moment":"date/moment.js",
	//时间日期区域选取
    //"datepicker2":"daterangepicker/daterangepicker.js",
    "daterangepicker":{
      "path":"date/daterangepicker/daterangepicker2124.js",
      "depending":["moment"]
    },
	//时间区域计算
    "currentDate":"date/currentDate.js",
	//bootstrap系列
    "bs-popover":"bootstrap/bs-popover.js",//依赖bootstrap.css
    "bs-tooltip":"bootstrap/bs-tooltip.js",//依赖bootstrap.css
    "bootstrap":{
      "depending":["bootstrap-css","bootstrap-js"]
    },
    "bootstrap-js":"bootstrap/bootstrap.3.31.js",
    "bootstrap-css":"bootstrap/bootstrap.3.31.css",
	//bootstrap日期选择插件
    "datetimepicker":{
      "path":"date/bs.datetimepicker/bootstrap-datetimepicker.min.js",
      "depending":["moment"]
    },
	//adminDesign面板等控制插件
    "adminDesign.main":"admindesign/main.js",
    "colorBox":"admindesign/colorbox.js",
    "panelCtrls":"admindesign/panel.ctrls.js",
	//轮播插件
    "slick":"slick/slick.min.js",
	//浏览器监测插件
    "bowser":"bowser.min.js",
	//鼠标提示插件
    "tooltips":"tooltips/tooltipster.bundle.min.js",
	//jqueryUI
    "jui":"jui/jquery-ui.js",
	//popover弹层插件
    "popover":"popover/jquery.webui-popover.min.js",
	//图片预览编辑
    "previewBox":"preview/previewbox.js",
    "previewPro":{
      "path":"preview/previewpro.js",
      "depending":["plugin/preview/previewpro.htm","jui"]
    },
	//条形码生成
    "barcode":"barcode/jsbarcode-all.js"
  }

  //动态更新...
```


## 十一，示例1（importing的使用）
```
importing('some.css' , 'some.js', 'somePluginName', needTest?'test.js':null, function(scope){

	//页面逻辑代码都在此

});
```

## 十二，示例2（importing与$source的配合使用）
```
importing( 'mock/menu.json', '_temp/pa.htm', 'http://293.12.26/a.js','bootstrap',function(scope){

	//这里可以直接取到加载的资源，同步使用
	var menuData=$source('mock/menu.json');
	var htmlStr=$source('_temp/pa.htm');

});
```

## 十三，示例3（importing与scope的配合使用）
```
importing('bootstrap','ztree'，function(scope){

	//声明绑定
	var $formEle=$('[x-form]').bind2(scope,'queryData');
	var $listEle=$('[x-list]').bind2(scope,'listData');


	//获取数据，接口交互，并刷新元素
	var url=makeAct('user/basic');

	$post(url, scope.get('queryData'), function(res){
		scope.set('listData',res.data);
	});

});
```

## 十四，示例4（importing与scope的配合使用2）
```
<html x-app>
  <body>
    
	<!--查询区域-->
	<div x-form="queryData">
	  年龄范围:<input type="text" x-name="age"/><br>
	  身高范围:<input type="text" x-name="height"/>
	  <!--按钮组件-->
	  <p widget="query-btns"></p>
	</div>

	<!--结果区域-->
	<ul x-list="usersData">
	  <li>
	    <img src="{photoSrc}"/> 
        <p>姓名:{name},年龄:{age}</p>
	  </li>
	</ul>


importing('bootstrap','autocomplete'，function(scope){

	//html中已经绑定，无需再手动声明绑定

	//获取数据，接口交互，并刷新元素
	$post(url, scope.get('queryData'), function(res){
		scope.set('usersData',res.data);
	});



	//更改数据
	scope.set('usersData',function(usersData){

		usersData.push( {name:'张三',age:18} );
		usersData[2].name='测试姓名';

		//元素将自动刷新...

	});



	//或也可以将数据拿出，做些操作，再放回去
	var arr=scope.get('usersData');

	arr=arr.concat(....);

	scope.set('usersData', arr ,true); //true表示不用检测是否与原值有差异，肯定变了，scope直接刷吧

});
```

## 十五，示例5（使用表格）
```
importing('bootstrap','ztree'，function(scope){

    //表格配置
    var tbSetting={
        check:true,
        fixed:false,
        cols:[
            {title:'序号', key:'rowNum'},
            {title:'照片', key:'src', cls:'tcenter'},  // class类名
            {title:'姓名', key:'name'},
            {title:'年龄', key:'age'},
            {title:'已婚', key:'flag.asCnYes'}  //跟了过滤器
        ]
    };

    //绑定和配置
	var $tbEle = $('[x-table]') .bind2(scope,'listData') .table(tbSetting);


    //做好这些后，就只需要刷数据了
    scope.set('listData',res.data)

    scope.set('listData',otherData)

    ...

});
```