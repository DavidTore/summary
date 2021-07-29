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

用法：**useEffect( callback [, [propertyArray] ])**

> 可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。React 将按照 effect 声明的顺序依次调用组件中的*每一个* effect。

```jsx
/* useEffect 第一个参数effect函数，此函数在执行 DOM 更新（包括第一次渲染之后和每次更新 即mount/update）之后会被调用；*/
 const Example = (props) => {
     const [count, setCount] = useState(0);
     useEffect(() => { console.log(count) })
 }
/* 第一个参数也能返回一个函数用来清除副作用；会在组件卸载的时候执行清除操作 */
const Status = (props) => {
  const [status, setStatus] = useState(true);
  useEffect(() => {
    function handleFunction(status) {/* do sth */}
    api.doSth(props.id, handleFunction);
    return function cleanup() { api.doOther(props.id, handleFunction) }
    //或者返回箭头函数 return (() => { api.doOther(props.id, handleFunction) })
  });
}
/* 第二个参数为空数组时，表示仅只运行一次 effect（仅挂载卸载 mount/unMount）* /
 
```