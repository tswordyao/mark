react v15升级
15.3中新加了一个 PureComponent 

react v16升级
```
1、可以render返回数组
2、增加组件生命周期componentDidCatch方法，取代借助unstable_handleError来处理异常
3、使用了Set和Map数据结构
4、createClass彻底废弃, initialState这种也废弃
```

### 实例化
首次实例化
* getDefaultProps  用ClassName.defaultProps=替代
* getInitialState  用constructor 替代
* componentWillMount
* render
* componentDidMount

实例化完成后的更新
* getInitialState
* componentWillMount
* render
* componentDidMount

存在期
组件已存在时的状态改变
* componentWillReceiveProps(nextProps)
* shouldComponentUpdate(nextProps,nextState)  // 无此方法？是否pureComponent？浅比较相等？
* componentWillUpdate
* render
* componentDidUpdate

销毁&清理期
* componentWillUnmount 一些事件监听和定时器需要在此时清除

```
// 属性验证
  propTypes: {
    myTitle: React.PropTypes.string.isRequired,
  },

// 现有state
this.setState( state => {clickCount: state.clickCount++ }  );

// 整体替换state
replaceState(newState,cb)
与setState()类似，但只会保留newState中状态，原state不在newState中的状态都会被清除。

setProps、replaceProps、forceUpdate(cb)、 isMounted

如果 items 里面是引用类型数据，进行某一项删除操作，引起序位变化，子组件里会不必要re-render
（本来只要父组件自身re-render就可以了）
这个时候可以使用immutable-js函数库

<Route path="home"
    getComponent={(location, cb) => {
        require.ensure([], require => {
            cb(null, require('./containers/Home.js').default)
        },'home');
    }} 
onEnter={request} />

function request(nextState, replace, next){
   let auth = nextState.location.query.auth;
   if(auth === 'home'){
      next();
   } else if (auth === 'other') {
      replace('/other');
      next();
   } else {
      replace('/');
      next();
   }
}
```
注：nextState ： 表示跳转后的location 信息；replace 用于 更改下一个进入的页面地址，但是不会跳转；next : 用于跳转页面，没有其他操作则显示当前路由对应页面

### 如果使用路由拆分，如下

```
    module.exports = {
        path: 'home',

        getChildRoutes(partialNextState, cb) {
            require.ensure([], (require) => {
              cb(null, [
                require('./home/page'),
                require('./home/student'),
                require('./home/admin')
              ])
            })
        },

        getComponent(nextState, cb) {
            require.ensure([], (require) => {
              cb(null, require('../containers/Home').default)
            }, 'Home')
        },

        onEnter: (nextState, replace, next) => {
          console.log(nextState.location.state);
          let isAuth = nextState.location.state.isAuth;
          if (isAuth === 'student' || isAuth === 'teacher' || isAuth === 'admin') {
             next();
          } else {
             replace('/error');
             next();
          }
       }
    }
```

### 库的说明
```
react-router-redux 是将react-router 和 redux 集成到一起的库，让你可以用redux的方式去操作react-router。例如，react-router 中跳转需要调用 router.push(path)，集成了react-router-redux 你就可以通过dispatch的方式使用router，例如跳转可以这样做 store.dispatch(push(url))。本质上，是把react-router自己维护的状态，例如location、history、path等等，也交给redux管理。一般情况下，是没有必要使用这个库的。
```