# [new](https://juejin.cn/post/6844903937405878280)

## 描述

1. 创建一个空的简单JavaScript对象（即`{}`）；
2. 为新创建的对象添加属性`__proto__`，将该属性链接至构造函数的原型对象；
3. 将新创建的对象作为`this`的上下文；
4. 如果该函数没有返回对象，则返回`this`。

## 手动写一个new

```js
function _new(fn,...args){ 
    const newObj = Object.create(fn.prototype); // 基于fn的原型创建一个新的空对象

    const result = fn.apply(newObj,args); // 添加属性到新创建的newObj上, 并获取fn函数执行的结果

    //不论其是否写return，都会把新创建的实例返回，若用户自己返回内容，且返回的是一个引用类型值，则会覆盖默认返回的实例
    return result instanceOf Object ? result : newObj 
}
```
