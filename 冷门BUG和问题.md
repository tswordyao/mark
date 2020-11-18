## 冷门技巧、BUG、 问题：
```
function workerify(fn) {
    var worker = new Worker(
        URL.createObjectURL(new Blob([
           'self.onmessage = ' + fn.toString()], {
           type: 'application/javascript'
        })
    ));
    return worker
  }

冷门Question

域名收敛 是否可以用ip代替域名解决？ 除入口主域名

getComputedStyle为什么会引发reflow？是否由于UI渲染后没有保存布局value，所以只有再次reflow才能再得到？  
那UI渲染后为什么不在element上保存value?  
是由于实现element各个接口方法的由宿主环境决定，element对象由js引擎控制？
那为什么getComputedStyle又能够获得了？
最终应该是因为没有设计挂载到元素上，获得也是只能通过方法获得。

如果获得后挂值在元素上，就要时刻监听元素的改变，更新这个值，否则就不准了，而这样开销又很大。
```

======================
冷门BUG
```
String(Object.create(null))
VM91251:1 Uncaught TypeError: Cannot convert object to primitive value
    at String (<anonymous>)
    at <anonymous>:1:1


Object.toString.call(n)
VM1546:1 Uncaught TypeError: Function.prototype.toString requires that 'this' be a Function
    at Object.toString (<anonymous>)
    at <anonymous>:1:17


JSON.stringify(n)
"{}"


Object.prototype.toString.call(Object.create(null))
"[object Object]"
```