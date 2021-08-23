# Async / Await

## 基本用法(Async)

- async作为`关键字`用在`函数`前面，表示不阻塞后面代码执行

```jsx
async function Foo(item){
    try{
        let res = await Bar();
        return res;
    }catch(err){
        /* handle error here */
    }
}

const Foo = async(item) => {
    try{
        let res = await Bar();
        return res;
    } catch(err){
        /* handle error here */
    }
}
```

- async函数返回一个`promise`对象，可以用`then`方法获取返回值
  
```jsx
async function timeout() {
    return 'hello world'
}
timeout().then(result => {
    console.log(result);
})
```

- async函数若有返回值，则调用时会调用`Promise.resolve()`转换为`Promise`对象；若抛出错误，会调用`Promise.reject()`，可以用`catch`捕获

```jsx
async function timeout(flag) {
    if (flag) {
        return 'hello world'
    } else {
        throw 'my god, failure'
    }
}
timeout(true);  // Promise {<resolved>}
timeout(false).catch(err => {}); // Promise {<rejected>}
```

## 基本用法(Await)