# react lifeycycle

```

----render component 
----useFriendStatus
null
subscribe in effecct like update
null => false

----render component
----useFriendStatus
false
unSubscribe in effect like unmount
subscribe in effecct like update
false => false

----render component
----useFriendStatus
false
9false => false


进入纯函数组件流程
进入useCustomHook

组件mount完毕
useEffect中的注册代码开始触发

subscribe回调中更新数据

进入纯函数组件流程
进入useCustomHook
上次useEffect中返回的代码开始触发
本次useEffect中注册的代码开始触发

subscribe回调中更新数据

进入纯函数组件流程
进入useCustomHook
数据没变，本次useEffect不再触发
组件没卸，上次useEffect中的注册的监听一直有效

自己的变量和更改变量的方法不会触发函数组件的重新渲染，也就是没有setState的效果
const [count, setCount] = useState(0);
这个setCount函数不是吃素的，它有setState的效果，每次会重新运行函数进行渲染
而再次进入函数时，是保持了上次的值记录的，也就是并非每次函数调用都重新生成。


哪怕是使用了useState中得到的count，也必须用setCount来设置，自己改这个变量是无效的，也是错误的，因为在函数作用域内是声明为const
所以从useState中得到的，是直接连接React渲染机制的钩子和管道，是脱离了函数组件的调用堆栈保存的
这意味着在组件正在被渲染的时候，数据是被存储在组件外面的

另一方面，如果有两行代码
const [count, setCount] = useState(0);
const [count2, setCount2] = useState(0);
这两行又是互相独立的，各自有各自的记录

它是怎么实现的呢？ 一个多维数组。多处useState产生多个state记录列表，多次运行函数，每个state记录列表都会更新
（事实上多处useState记录了多个state和对应索引，并不是每个state一个列表记录每个state的所有历史过程，对比的时候，用当前传入的参数和索引处对比就可以了）

本来react就是通过setState去更新，记录state去对比，所谓的生命周期就是告诉你一些合适的时机你可以做些副作用
如果帮函数组件记录了state，提供了setState的方法，再用useEffect的入参和返回值，从产生副作用到关闭副作用的维度，代替或者说归纳了生命周期时机这个维度。

```