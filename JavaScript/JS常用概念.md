
# JS常用概念

## ES6新特性

1. 块级作用域、块级变量`let`、块级常量`const`
1. 箭头函数
1. 参数处理（默认参数/...）
1. 模板字面量(模板字符串)
1. 对象的扩展
1. 解构赋值（[a,b] = [x,y]）
1. 模块（`import`/`export`）
1. 类（`class`/`extends`）
1. `Promise`

## JavaScript 数据类型和数据结构

- 6 种`原始类型`，使用`typeof`运算符检查：
  - `undefined` - `true`/`false`
  - `Boolean`
  - `Number`
  - `String`
  - `BigInt`
  - `Symbol`
- null
- Object

## 动态计算属性

```js
let subName = 'name';
let obj = { [subName] : 'David' } 
```

## [Reduce 和 Transduce](https://www.ruanyifeng.com/blog/2017/03/reduce_transduce.html)

## 函数柯里化

## 变量提升/函数提升

```js
// 在当前作用域中，JavaScript代码执行之前，浏览器首先会默认的把所有带var和function声明的变量进行提前的声明或者定义
    console.log(foo); //->undefined
{
    // EC(BLOCK)
    //  作用域链<EC(BLOCK,EC(G))>
    //  变量提升：
    //      foo = 0x001;[[scope]:EC(BLOCK)]
    //      foo = 0x002;[[scope]:EC(BLOCK)]
    //      ------
    //      foo=0x002;

    console.log(foo);//函数{2}
    function foo() {1}//把之前对foo的操作映射给EC(G)一份=>全局foo=0x002
    console.log(foo);//函数{2}
    foo = 1; //把私有的foo=1
    console.log(foo);//->1
    function foo() {2}//把之前对foo的操作映射给EC(G)一份=>全局foo=1
    console.log(foo); //->1
}
console.log(foo);//->1
```

## 闭包

### 定义

**「函数」和「函数内部能访问到的变量」的总和，就是一个闭包。**
闭包常常用来「间接访问一个变量」。换句话说，「隐藏一个变量」。也就是让父对象能获取到子对象的局部变量

```js
// local变量和bar函数组成一个闭包
function foo(){
  var local = 1
  function bar(){
    local++
    return local;
  }
  return bar;
}

var add = foo();
add() // 本来window是获取不到foo内local的值，这样就能获取到了
```

### 用途

> - 可以读取函数内部的变量；
> - 让这些变量的值始终保持在内存中。

### 注意点

- 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

- 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

## 深拷贝

```js
// 递归
function deepClone(obj){
  let objClone = Array.isArray(obj) ? [] : {};
  if(obj && typeof obj == 'object'){
    for(key in obj){
      if(obj.hasOwnProperty(key)){
        if(obj[key] && typeof obj[key] == 'object'){
          objClone[key] = deepClone(obj[key]);
        } else {
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone;
}

// JSON.parse(JSON.stringfy())
function deepClone(obj){
  let _obj = JSON.stringfy(obj);
  let objClone = JSON.parse(_obj);
  return objClone;
}
```

## 浅拷贝

```js
function shallowCopy(obj){
  let objClone = Array.isArray(obj) ? [] : {};
  for(let key in obj){
    objClone[key] = obj[key]
  }
  return objClone;
}

Object.assaign({},obj)
```

## Cookie/localStorage/sessionStorage

|存储方式|作用与特性|存储数量与大小|api|
| :--: | :--: | :--: | :--: |
| Cookie | 1.存储用户信息，获取数据需要与服务器建立连接。 2.可存储的数据有限，且依赖于服务器，无需请求服务器的数据尽量不要存放在cookie中，以免影响页面性能。3.可设置过期时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效| 1.最好将cookie控制在4095B以内，超出的数据会被忽略。 2. IE6或更低版本最多存20个cookie； IE7及以上版本最多可以有50个；Firefox最多50个；chrome和Safari没有做硬性限制。| document.cookie|
|localStorage|1.存储客户端信息，无需请求服务器。2.数据永久保存，除非用户手动清理客户端缓存。3.开发者可自行封装一个方法，设置失效时间。|5M左右|localStorage.setItem('key', 'value'); // getItem('key'); // removeItem('key'); // clear();|
|sessionStorage|1.存储客户端信息，无需请求服务器。2.数据保存在当前会话，刷新页面数据不会被清除，结束会话（关闭浏览器、关闭页面、跳转页面）数据失效。|5M左右|sessionStorage.setItem(key,val) // getItem('key') //removeItem('key') // clear();|

## JSON AJAX

- json 一种数据格式，类似 xml（按层级表示对象，及对象之间的层级关系)，也是表示对象，属性。轻量级，适合服务器与JavaScript之间的交互
- ajax(asynchronous javascript and xml)
  - 创建XMLHttpRequest对象,即创建一个异步调用对象.
  - 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
  - 设置响应HTTP请求状态变化的函数.
  - 发送HTTP请求.
  - 获取异步调用返回的数据.
  - 使用JavaScript和DOM实现局部刷新.
  
  ```js
  xmlHttpRequest = new XMLHttpRequest();
  xmlHttpRequest.onreadystatechange=function(){ // 设置响应http请求状态变化的事件
  console.log('请求过程', xmlHttpRequest.readyState);
  if(xmlHttpRequest.readyState == 4){ // 判断异步调用是否成功,若成功开始局部更新数据
  console.log('状态码为', xmlHttpRequest.status);
  if(xmlHttpRequest.status == 200) {
    console.log('异步调用返回的数据为：', xmlHttpRequest.responseText);
    document.getElementById("myDiv").innerHTML = xmlHttpRequest.responseText; // 局部刷新数据到页面
    } else { // 如果异步调用未成功,弹出警告框,并显示错误状态码
    alert("error:HTTP状态码为:"+xmlHttpRequest.status);
    }
    }
    }
    xmlHttpRequest.open("GET","https://www.runoob.com/try/ajax/ajax_info.txt",true); // 创建http请求，并指定请求得方法（get）、url（https://www.runoob.com/try/ajax/ajax_info.txt）以及验证信息
    xmlHttpRequest.send(null);
  ```

## v-if v-for优先级

```js
// v-for优先于v-if
<li v-for="todo in todos" v-if="!todo.isComplete"> {{ todo }} </li>
```

## 浏览器跨域

`协议，域名，端口` 任一个不同即为跨域
解决方案：

- 通过jsonp跨域（只能实现get一种请求）
  
  ```js
  this.$http.jsonp('url', {
    params: {},
    jsonp: 'handleCallback'
    }).then((res) => {
      console.log(res); 
    })
  ```

- document.domain + iframe跨域
- location.hash + iframe
- window.name + iframe跨域
- postMessage跨域
- 跨域资源共享（CORS）(服务端设置Access-Control-Allow-Origin)
  
  ```js
  axios.defaults.withCredentials = true; //设置是否可以带cookie
  Vue.http.options.credentials = true;
  ```

- nginx代理跨域
- nodejs中间件代理跨域
- WebSocket协议跨域

## 垃圾回收

## 盒模型

- 标准盒模型 `content`(width) `padding border margin`
- IE盒模型  `content padding border` (三者合为width) `margin`
`box-sizing: content-box` 是W3C盒子模型
`box-sizing: border-box` 是IE盒子模型
`block`模型的元素默认占据一行，允许通过css设置宽度、高度。
`inline`类型的元素的宽度与高度取决于它的内容的高度与宽度，在页面宽度允许的情况下，默认一行放多个元素直至放不下

## 回流和重绘

回流（布局改变）： render tree中一部分因为元素规模尺寸/布局/隐藏等改变而重新构建，每个页面至少一次回流，即第一次加载时；
重绘（元素更新属性）：render tree元素更新属性，不影响布局，则为重绘，如`background-color`；

## 事件冒泡和事件捕获

- 事件冒泡：事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发；
  
  ```js
  // div1 包裹 div2 包裹 div3
  div1.onclick = function(){console.log(this.id)}  // div1
  div2.onclick = function(){console.log(this.id)} // div2 div1
  div3.onclick = function(){console.log(this.id)} // div3 div2 div1
  ```

- 事件捕获：事件从最不精确的对象(document 对象)开始触发，然后到最精确的对象
  
  ```js
  // div1 包裹 div2 包裹 div3
  div1.addEventListener('click',fn,true); // div1
  div2.addEventListener('click',fn,true); // div1 div2
  div3.addEventListener('click',fn,true);// div1 div2 div3
  function fn(){console.log(this.id)}
  ```
  
  `element.addEventListener(event, function, useCapture)`
  第三个参数是boolean类型，默认是false，表示在事件冒泡的阶段调用事件处理函数,传入true，就表示在事件捕获的阶段调用事件处理函数。

- 阻止事件冒泡
  `event.stopPropagation()` 或者 `return false`

  ```js
  div3.onclick = function (e) {e.stopPropagation();};
  div3.onclick = function (e) {return false};
  ```

## import require

- require是运行时调用，require是赋值过程，所以require理论上可以运用在代码的任何地方
- import是编译时调用，import是解构过程，所以必须放在文件开头，且前面不允许有其他逻辑代码
  
```js
// 错误演示
export 1;
var a = 100;
export a;

// export在导出接口的时候，必须与模块内部的变量具有一一对应的关系
export {a}

export default function(){};
// equals to 
function a(){};
export {a as default};

import a from './d';
//equals to 
import {default as a}
```
