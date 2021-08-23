# 生命周期

## React 16.4及之后生命周期

- getDerivedStateFromProps(nextProps, prevState)
代替componentWillReceiveProps()

![ReactLifeCycle16.4.png](https://i.loli.net/2021/08/18/G48I67Zcu1noiW2.png)

## 旧版生命周期

- getDefaultProps()
用来设置组件默认的 `props`，组件生命周期**只会调用一次**。但是只适合 `React.createClass` 直接创建的组件， ES6/ES7 可以使用下面方式：

```jsx
// es7
class Component { static defaultProps = {} }
// 或者也可以
Component.defaultProps = {}
```

- getInitialState
设置`state`初始值，在这个方法中你已经可以访问到 `this.props`。 `getInitialState` 只适合 `React.createClass` 使用。使用 ES6 初始化state方法如下：

```jsx
class Component extends React.Component{
  constructor(props){
    super(props);
    this.state = { render: true }}}
```

- 挂载过程(实例化)
  
```jsx
// 数据初始化，需要写super
constructor(props, context)

// 组件已完成初始化数据，但是未渲染DOM时执行，主要用于服务端渲染
componentWillMount()

// 组件第一次渲染完成时执行的逻辑，此时DOM节点已经生成,需要请求外部接口数据，一般都在这里处理
componentDidMount()
```

- 更新过程(存在期)
  
```jsx
// 接收父组件新的props时，重新渲染组件执行的逻辑
componentWillReceiveProps(nextProps)

// 在setState以后，state发生变化，组件会进入重新渲染的流程时执行的逻辑。默认返回true。在这个生命周期中return false可以阻止组件的更新，组件不会再次渲染，主要用于性能优化
shouldComponentUpdate(nextProps,nextState)

// shouldComponentUpdate返回true以后，组件进入重新渲染的流程时执行的逻辑
componentWillUpdate (nextProps,nextState)

// 页面渲染执行的逻辑，render函数把jsx编译为函数并生成虚拟dom，然后通过其diff算法比较更新前后的新旧DOM树，并渲染更改后的节点
render()

// 重新渲染后执行的逻辑
componentDidUpdate(prevProps,prevState)
```

- 卸载过程
  
```jsx
// 组件的卸载前执行的逻辑，比如进行“清除组件中所有的setTimeout、setInterval等计时器”或“移除所有组件中的监听器removeEventListener”等操作
componentWillUnmount()
```


React生命周期（旧版）
![ReactLifeCycle](https://upload-images.jianshu.io/upload_images/16775500-8d325f8093591c76.jpg)