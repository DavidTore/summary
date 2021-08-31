# Promise

## 使用Promise原因

### 解决回调地狱

> 回调地狱：发送多个异步请求时，每个请求之间相互都有关联，会出现第一个请求成功后再做下一个请求的情况。这时候往往会用嵌套的方式来解决这种情况，但是这会形成“回调地狱”。如果处理的异步请求越多，那么回调嵌套的就越深。
> 出现的问题：
> 1.代码逻辑顺序与执行顺序不一致，不利于阅读与维护；
> 2.异步操作顺序变更时，需要大规模的代码重构；
> 3.回调函数基本都是匿名函数，bug追踪困难。

### 解决异步问题

> JavaScript是单线程执行代码，导致JavaScript的很多操作都是异步执行的，以下是解决异步的几种方式:
> 1.回调函数(定时器)
> 2.事件监听
> 3.发布/订阅
> 4.`Promise`对象(将执行代码和处理结果分开)
> 5.`Generator`
> 6.ES7的`async/await`


## 基本用法

- `Promise`有三种状态: `pending`, `resolved`, `rejected`；
  
> 1.初始化，状态：pending
> 2.当调用resolve(成功)，状态：pengding=>fulfilled
> 3.当调用reject(失败)，状态：pending=>rejected

- Promise的结构

```js

class Promise{
  constructor(exector) {
  function resolve(){}
  function reject(){}
  exector(resolve,reject)}

  then(){} 
}
```

- `Promise`对象常用`then`方法用来执行回调函数，`then`方法包含成功的`resolved`回调，失败的`rejected`回调（可选）
  
```js
    let prom = (time) => {
        return new Promise((resolve,reject) => {
            setTimeout(()=>{resolve('Success')},time)
        })
    }
    prom(100).then(res => {console.log(res)})
```

## API

- `Promise.resolve()` 将现有对象转为`Promise`对象,状态为`resolved`;
  
```js
    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))

    // 参数是一个 Promise 实例
    // 如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。

    // 参数是一个thenable对象
    let thenable = {
    then(resolve, reject) {
        resolve('success'); }};

    let p1 = Promise.resolve(thenable);
    p1.then(value => { console.log(value);}); // success
```

- `Promise.reject()` 返回一个`Promise`对象,状态为`rejected`；

- `Promise.prototype.then()` 最多需要两个参数，包含成功的`resolved`回调，失败的`rejected`回调，

```js

```

- `Promise.prototype.catch()` 发生错误的回调函数。

- `Promise.all()` 适合用于所有的结果都完成了才去执行`then()`成功的操作
  
## Promise优缺点

### 优点

1. `Promise`分离了异步数据获取和业务逻辑，有利于代码复用。
2. 可以采用链式写法
3. 一旦`Promise`的值确定为`fulfilled`或者`rejected`后，不可改变。

### 缺点

- 代码冗余，语义不清

## 手写一个Promise

