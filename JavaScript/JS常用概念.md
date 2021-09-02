
# JS常用概念

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

## 任务队列

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
