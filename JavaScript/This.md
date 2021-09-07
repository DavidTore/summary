# This

## [this 是什么](https://segmentfault.com/a/1190000014224541)

**`this`既不指向函数自身，也不指函数的词法作用域**，`this`具体指向什么，取决于怎么调用的函数；
`this`包含四种绑定规则，优先级从低到高分别是：

- 默认绑定
即没有其他绑定规则存在时的默认规则；在严格模式(`strict mode`)下，全局对象将无法使用默认绑定

    ```js
    function foo() {console.log( this.a );}
    var a = 2; 
    foo(); // 2
    ```

- 隐式绑定
  调用位置上存在上下文对象

  ```js
  function foo() {console.log( this.a );}
  var a = 2;
  var obj = {a: 3,foo: foo };
  obj.foo(); // 3  对foo的调用存在上下文对象obj，this进行了隐式绑定，即this绑定到了obj上
  
  var bar = obj.foo;
  bar(); // 2   obj.foo 是引用属性，赋值给bar的实际上就是foo函数

  setTimeout( obj.foo, 100 ); // 2  传参实际上传的就是foo对象本身的引用
  ```

- 显示绑定
  在显示绑定中，对于`null`和`undefined`的绑定将不会生效

  ```js
  function foo() {console.log( this.a );}
  var a = 2;
  var obj1 = {a: 3};
  var obj2 = {a: 4};
  var bar = function(){foo.call( obj1 );}
  
  bar(); // 3
  setTimeout( bar, 100 ); // 3
  bar.call( obj2 ); // 3
  ```

  - `Function.prototype.call(thisArgs, arg1, arg2, ...)` 调用函数

    ```js
    let obj1 = {
      name: 'obj1',
      foo(...args){console.log(this.name, ...args)}};
    let obj2 = {name: 'obj2'};
    obj1.foo.call(obj2, 'args1','args2')  // obj2, args1, args2


    //手写 call 函数
    Function.prototype.myCall = function(context, ...args){
      // 获取第一个参数上下文环境
      context = (context == undefined || context == null) ? window : context;
      // 将被调用的函数保存至第一个参数内
      context._fn = this;
      // 在第一个参数上下文环境内执行被调用的函数
      let res = context._fn(...args);
      // 删除第一个参数里的函数
      delete context._fn;
      return res;
    }

    ```

  - `Function.prototype.apply(thisArg, [argsArray])`调用函数
  
    ```js
    let obj1 = {
      name: 'obj1',
      foo(args){console.log(this.name, ...args)}};
    let obj2 = {name: 'obj2'};
    obj1.foo.call(obj2, ['args1','args2'])  // obj2, args1, args2

    // 手写 apply 函数
    Function.prototype.myApply = function(context,args){
      context = (context == null || context == undefined) ? window : context;
      context._fn = this;
      // 只有调用时传参需要更改
      let res = context._fn(...args);
      delete context._fn;
      return res;
    }
    ```

  - `Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])` 创建函数
  
    ```js
    let obj1 = {
      name: 'obj1',
      foo(args){console.log(this.name, ...args)}};
    let obj2 = {name: 'obj2'};
    obj1.foo.bind(obj2, ['args1','args2'])()  // obj2, args1, args2
    obj1.foo.bind(obj2, 'args1','args2')()  // foo(...args){console.log(...args)}

    //手写bind
    Function.prototype.bind2 = function(context, ...args1) {
      context = (context === undefined || context === null) ? window : context;
      let _this = this;
      return function(...args2) {
        context.__fn = _this;
        let result = context.__fn(...[...args1, ...args2]);
        delete context.__fn;
        return result
        }
      }
    ```

- `new`绑定
  > 如果函数没有返回其他对象,那么new表达式中的函数调用会自动返回这个新对象

  ```js
  function foo(a) {this.a = a;}
  var a = 2;
  var bar1 = new foo(3);
  console.log(bar1.a); // 3
  
  var bar2 = new foo(4);
  console.log(bar2.a); // 4
  ```

## 箭头函数中的this指向问题

> 箭头函数体内的this对象，就是**定义该函数时所在的作用域指向的对象**，而不是使用时所在的作用域指向的对象，其中作用域是指函数内部。

```js
var name = 'window'; 
var A = {
   name: 'A',
   sayHello: function(){
      return () => console.log(this.name)
   }
}
var B = { name: 'B'}
A.sayHello()() // A
A.sayHello.call(B)() // B
A.sayHello().call(B) // A
/*

分析：
A.sayHello定义了一个函数，
而该函数作用域里面包裹一个箭头函数，
则箭头函数的this指向和该函数作用域一致。

A.sayHello()则该函数此时this指向调用者A，所以箭头函数this也为A；
A.sayHello.call(B)，B调用A.sayHello方法，并更改this指向到B，则箭头函数为B；
A.sayHello().call(B)，虽然B调用箭头函数方法，但箭头函数不指向使用时作用域指向对象，因此仍为A

*/

// 若改为如下，则由于返回一个匿名函数，而匿名函数执行时默认作用域是在window
// 因此除非是对此匿名函数bind/apply/call，否则将为window
sayHello: function(){
  return function(){console.log(this.name)}
}

```

注意：箭头函数

- 不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
- 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用`rest`参数代替。
- 不可以使用`yield`命令，因此箭头函数不能用作`Generator`函数。

## 绑定规则优先级

- 函数是否在new中调用(`new`绑定)？如果是的话`this`绑定的是新创建的对象
- 函数是否通过`call`、`apply`(显式绑定)或者硬绑定调用？如果是的话，`this`绑定的是指定的对象
- 函数是否在某个上下文对象中调用(隐式绑定)？如果是的话，`this`绑定的是那个上下文对象
- 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到`undefined`，否则绑定到全局对象。
