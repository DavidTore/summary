# Async / Await

## 基本用法(Async)

- async作为`关键字`用在`函数`前面，表示不阻塞后面代码执行

```js
async function Foo(item){
    try {
        let res = await Bar();
        return res;
    } catch(err){
        /* handle error here */
    }
}

const Foo = async(item) => {
    try {
        let res = await Bar();
        return res;
    } catch(err){
        /* handle error here */
    }
}
```

- async函数返回一个`promise`对象，可以用`then`方法获取返回值
  
```js
async function timeout() {
    return 'hello world'
}
timeout().then(result => {
    console.log(result);
})
```

- async函数若有返回值，则调用时会调用`Promise.resolve()`转换为`Promise`对象；若抛出错误，会调用`Promise.reject()`，可以用`catch`捕获

```js
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

- `await`关键字只能放在`async`函数里面
- `await`可以用在任意表达式前面，而不仅仅是`Promise`对象前面
- 若`await`之后是一个`Promise`对象，则会阻塞后面代码，等待`resolve`值

## async/await 的优势在于处理 then 链

```js
// Basic Steps Function
function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(m, n) {
    console.log(`step2 with ${m} and ${n}`);
    return takeLongTime(m + n);
}

function step3(k, m, n) {
    console.log(`step3 with ${k}, ${m} and ${n}`);
    return takeLongTime(k + m + n);
}
```

```js
// 使用Promise处理
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => {
            return step2(time1, time2)
                .then(time3 => [time1, time2, time3]);
        })
        .then(times => {
            const [time1, time2, time3] = times;
            return step3(time1, time2, time3);
        })
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}

doIt();
```
```js
// async/await处理
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time1, time2);
    const result = await step3(time1, time2, time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}

doIt();
```