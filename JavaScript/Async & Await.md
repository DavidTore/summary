# Async / Await

## 基本用法(Async)

- `async`作为`关键字`用在`函数`前面，表示不阻塞后面代码执行

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

- `async`函数返回一个`promise`对象，可以用`then`方法获取返回值
  
```js
async function timeout() {
    return 'hello world'
}
timeout().then(result => {
    console.log(result);
})
```

- `async`函数若有返回值，则调用时会调用`Promise.resolve()`转换为`Promise`对象；若抛出错误，会调用`Promise.reject()`，可以用`catch`捕获

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
- `await`从右向左先执行后面代码，当发现有await关键字，让出线程，阻塞代码
  
```js
// await 并不会直接阻塞紧跟await后面的表达式，而是先执行表达式，再由右向左发现await
async function async1() {
    console.log( 'async1 start' )
    await async2()
    console.log( 'async1 end' )
}
function async2() {
    console.log( 'async2' )
}
async1()
console.log( 'script start' )
// async1 start
// async2
// script start
// async1 end
```

- 若`await`之后是一个`Promise`对象，则会阻塞后面代码，等待`resolve`值

> - 如果不是`promise`, `await`会阻塞后面的代码，先执行`async`外面的同步代码，同步代码执行完，再回到`async`内部，把这个非`promise`的东西，作为`await`表达式的结果。
> - 如果它等到的是一个`promise`对象，`await`也会阻塞后面的代码，先执行`async`外面的同步代码，等着`Promise`对象`fulfilled`，然后把`resolve`的参数作为`await`表达式的运算结果。特别注意：如果`promise`没有一个成功的值传入，对`await`来说就算是失败了，`await`后面的代码就不会执行

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