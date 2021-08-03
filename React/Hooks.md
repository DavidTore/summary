# Hook

> Hook 在class内部不起作用，需要在**函数组件**编写;
> 只能在**函数最外层**调用 Hook。不要在循环、条件判断或者子函数中调用

## useState

用法：**useState(initState)**

```jsx
/* 包含state可以使用Hooks，不包含的尽量用函数组件（无状态组件）*/
import React, {useState, useEffect, useReducer} from 'react';-
const Example = () => {
    const [count, setCount] = useState(0);
    return(
        <div>
        <button onClick={() => setCount(count+1)}>{count}</button>
        </div>
    )
}
```

利用`解构赋值`，useState返回值为： **当前state**和**更新state的函数**

## useEffect

用法：**useEffect( callback: Function [, propertyArray: Array] )**

> 可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。可以按照代码用途分离effects，React 将按照 effects 声明的顺序依次调用组件中的*每一个*effect。

```jsx
/* useEffect 第一个参数effect函数，此函数在执行 DOM 更新（包括第一次渲染之后和每次更新 即mount/update）之后会被调用；*/
 const Example = (props) => {
     const [count, setCount] = useState(0);
     useEffect(() => { console.log(count) })
 }

/* 第一个参数也能返回一个函数用来清除副作用；会在组件卸载的时候执行清除操作；同时，由于副作用函数默认每次渲染执行，所以每次更新后，调用副作用函数前也会执行清除操作*/
/* 当节点更新时，会触发清除操作 */
const Status = (props) => {
  const [status, setStatus] = useState(true);
  useEffect(() => {
    function handleFunction(status) {/* do sth */}
    api.doSth(props.id, handleFunction);
    return function cleanup() { api.doOther(props.id, handleFunction) }
    //或者返回箭头函数 return (() => { api.doOther(props.id, handleFunction) })
  });
}
/* 当节点卸载时，会触发清除操作 */
useEffect(() => { 
        console.log('update '+ count); 
        return () => { console.log('unMount/everyRender'); }
    })
return (<div>
            <button onClick={()=> setCount(count +1)}>{count}</button>   {/*unMount/everyRender*/}
        </div>)
ReactDOM.unmountComponentAtNode(document.getElementById("app")); // unMount/everyRender


/* 第二个参数为空数组时，表示仅只运行一次 effect（仅挂载卸载 mount/unMount）*/
// class组件常利用 componentDidUpdate
    componentDidUpdate(prevProps, prevState){}
// useEffect使用第二个可选参数，仅在propertyArray更改时更新；若为空数组，则仅在挂载和卸载时运行

```

useEffect第二个参数注意点
> 第二个参数一般情况下不要使用引用类型，因为JavaScript中 {}==={} 结果为false

| 参数情况     | 结果                                                                                |
| :----------- | :---------------------------------------------------------------------------------- |
| 不传参数     | 组件每次初始化/更新 都执行；可能导致性能问题                                        |
| 空数组       | 仅在 mount/unMount 执行；effect不依赖于props或state中任何值，所以永远不需要重复执行 |
| 一个值的数组 | 该值有*变化*就执行                                                                  |
| 多个值的数组 | 有一个值有*变化*就执行                                                              |

- useEffect中发起[网络请求例1](https://blog.csdn.net/wayne214/article/details/108799557)
- useEffect中发起[网络请求例2](https://blog.csdn.net/qq_29438877/article/details/100015581)
