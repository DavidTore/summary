# Promise

## 使用Promise原因

### 解决回调地狱

> 回调地狱：发送多个异步请求时，每个请求之间相互都有关联，会出现第一个请求成功后再做下一个请求的情况。这时候往往会用嵌套的方式来解决这种情况，但是这会形成“回调地狱”。如果处理的异步请求越多，那么回调嵌套的就越深。
> 出现的问题：
> 1.代码逻辑顺序与执行顺序不一致，不利于阅读与维护；
> 2.异步操作顺序变更时，需要大规模的代码重构；
> 3.回调函数基本都是匿名函数，bug追踪困难。

### [解决异步问题](https://www.cnblogs.com/zuobaiquan01/p/8477322.html)

> JavaScript是单线程执行代码，导致JavaScript的很多操作都是异步执行的，以下是解决异步的几种方式:
> 1.回调函数(定时器)
> 2.事件监听
> 3.发布/订阅
> 4.`Promise`对象(将执行代码和处理结果分开)
> 5.`Generator`
> 6.ES7的`async/await`

## 基本用法

- `Promise`有三种状态: `pending`, `fulfilled`, `rejected`；
  
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

- `Promise`对象常用`then`方法用来执行回调函数，`then`方法包含成功的`resolved`回调，失败的`rejected`回调
  
```js
    let prom = (time) => {
        return new Promise((resolve,reject) => {
            setTimeout(()=>{resolve('Success')},time)
        })
    }
    prom(100).then(res => {console.log(res)})
```

## API

- `Promise()` 构造器主要用于包装不支持`Promise`（返回值不是`Promise`）的函数
  
```js
new Promise((resolve, reject) => {
  // 调用异步操作，并最终调用以下两种函数之一：
  //
  resolve(someValue)        // fulfilled
  // 或者
  reject("failure reason")  // rejected
});
```

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

    // 参数不是具有then方法的对象，或根本就不是对象
    // 如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved
    const p = Promise.resolve('Hello');
    p.then(function (s){ console.log(s) }); // Hello

    // Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象
    Promise.resolve().then(function () { console.log('two') });
```

**立即`resolve`的`Promise`对象，是在本轮“`事件循环`”（`event loop`）的结束时执行执行，不是马上执行,也不是在下一轮“`事件循环`”的开始时执行**，这是因为，传递到`then()`中的函数被置入了一个微任务队列，而不是立即执行，这意味着它是在`JavaScript`事件队列的所有运行时结束了，事件队列被清空之后，才开始执行

> **resolve()本质作用**
>
> - `resolve()`是用来表示`promise`的状态为`fullfilled`，相当于只是定义了一个有状态的`Promise`，但是并没有调用它；
> - `promise`调用`then`的前提是`promise`的状态为`fullfilled`；
> - 只有`promise`调用`then`的时候，`then`里面的函数才会被推入微任务中；

- `Promise.reject()` 返回一个`Promise`对象,状态为`rejected`；

- `Promise.prototype.then(onFulfilled[, onRejected])` 最多需要两个参数，包含成功的`resolved`回调，失败的`rejected`回调，其返回一个`promise`对象；
  
```js
p.then(value => { /* fulfillment */ }, 
       reason => { /* rejection */ });

```

> `then`里的参数是可选的，`catch(failureCallback)`是`then(null, failureCallback)`的缩略形式，但是当使用`then(resolveHandler, rejectHandler)`，`rejectHandler`不会捕获在`resolveHandler`中抛出的错误。
>
> - `then`方法提供一个供自定义的回调函数，若传入非函数，则会忽略当前`then`方法。
> - 回调函数中会把上一个`then`中返回的值当做参数值供当前`then`方法调用。
> - `then`方法执行完毕后需要返回一个新的值给下一个`then`调用（没有返回值默认使用`undefined`）。
> - 每个`then`只可能使用前一个`then`的返回值。

[相关例子](https://segmentfault.com/a/1190000010420744)节选^1^ [相关例子](https://www.cnblogs.com/honghong87/p/6216453.html)节选^2^

```js
    
    // 链式调用 返回doSomethingElse的值
    doSomething().then(function () {
        return doSomethingElse();
    }).then(finalHandler); // doSomethingElse

    // 由于第一个then不返回，默认return undefined
    doSomething().then(function () {
        doSomethingElse();
    }).then(finalHandler); // undefined

    // 由于第一个then传入非函数， 会忽略当亲方法，传入doSomething的值
    doSomething().then(doSomethingElse())
                .then(finalHandler); // doSomething

    // 直接把 doSomethingElse 当作回调函数，直接把doSomethingElse返回值作为下一个then传参值
    doSomething().then(doSomethingElse)
                .then(finalHandler); // doSomethingElse
```

- `Promise.prototype.catch()` 发生错误的回调函数，返回一个`promise`对象。

- `Promise.all()` 接收一个`promise对象数组`为参数，处理并行异步操作会用到，但是需要全部为`resolve`才能调用;

- `Promise.race()` 接收一个`promise`对象`数组`为参数，只要有一个`promise`对象进入`Fulfilled`或者`Rejected`状态的话，就会进行后面的处理。这可以解决多个异步任务的容错

- `Promise.prototype.finally()` 用于指定不管`Promise`对象最后状态如何，都会执行的操作

```js
promise
    .then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
```

## 约定

- 在本轮`事件循环`运行完成之前，回调函数是不会被调用的。
- 即使异步操作已经完成（成功`resolved`或失败`rejected`），在这之后通过`then()`添加的回调函数也会被调用。
- 通过多次调用`then()`可以添加多个回调函数，它们会按照插入顺序进行执行。
  
## Promise优缺点

### 优点

1. `Promise`分离了异步数据获取和业务逻辑，有利于代码复用。
2. 可以采用链式写法
3. 一旦`Promise`的值确定为`fulfilled`或者`rejected`后，不可改变。

### 缺点

- 代码冗余，语义不清

## 手写一个Promise

```js
const PENDING = "pending";//Promise会一直保持挂起状态，直到被执行或拒绝。
const FULFULLED = "fulfilled";
const REJECTED = "rejected";
class Promise{
   constructor(exector){
  let self = this;//缓存当前promise对象
  self.status = PENDING;//初始状态,对promise对象调用state(状态)方法，可以查看其状态是“pending"、"resolved"、还是”rejected“
       self.value = undefined;// fulfilled状态时 返回的信息
       self.reason = undefined;// rejected状态时 拒绝的原因
       self.onResolveCallBacks = [];// 存储resolve(成功)状态对应的onFulfilled函数
       self.onRejectCallBacks = [];// 存储rejected(失败)状态对应的onRejected函数
       let resolve = (value) => {//成功
           if(self.status === PENDING){//如果成功则，状态由pending=>fulfilled
               self.status = FULFULLED;
               self.value = value;
               self.onResolveCallBacks.forEach(cb=>cb(self.value));//执行发布
          }
      }
       let reject = (reason) => {//失败
           if(self.status === PENDING){//如果失败，则状态由pending=>rejected
               self.status = REJECTED;
               self.reason = reason;
               self.onRejectCallBacks.forEach(cb=>cb(self.reason));//执行发布
          }
      }
       
       try{
   exector(resolve,reject)              
      }catch(e){
           reject(e)
      }
}
then(onFulfilled,onRejected){
       let self=this;
       if(self.status === FULFULLED){
           onFulfilled(self.value);//成功值
      }
       if(self.status === REJECTED){
           onFulfilled(self.reason);//拒绝原因
      }
       if(self.status === PENDING){
           self.onResolveCallBacks.push(onFulfilled);//订阅发布
           self.onRejectCallBacks.push(onRejected);//订阅发布
      }
}
   //promise的决议结果只有两种可能：完成和拒绝，附带一个可选的单个值。如果Promise完成，那么最终的值称为完成值；如果拒绝，那么最终的值称为原因。Promise只能被决议（完成或拒绝）一次。之后再次试图完成或拒绝的动作都会被忽略。
}
​
new Promise((resolve,reject)=>{
   resolve("success")；
   //异步处理
   //处理结束后、调用resolve或reject
}).then((data)=>{
   console.log(data);//"success"
},(reason)=>{
   console.log(reason);
})
```
