
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