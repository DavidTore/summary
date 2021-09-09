[1](https://segmentfault.com/a/1190000008338987)
[2](http://caibaojian.com/es6/class.html)

```js
//es5 模拟类 组合继承
function Parent(name){
    this.name = name;
}
Parent.prototype.getName = function(){ return(this.name)};

function Children(name){
    Parent.call(this, name);
}
Children.prototype = new Parent();
Children.prototype.constructor = Children
let child = new Children();

// es5 模拟类 原型链
function Children(){}
Children.prototype = new Parent();
Children.prototype.constructor = Children;

// es5 模拟类 构造函数继承
function Children(name){
    Parent.call(this, name)
}
```

```js
// es6 class
class Parent{
    constructor(name){
        this.name = name;
    }
    getName(){return this.name}
}

class Children extends Parent{
    constructor(name,age){
        super(name);
        this.age = age;
    }
    getInfo(){
        return super.getName() + this.age
    }
}
let instance = new Children('Name', 11)
```