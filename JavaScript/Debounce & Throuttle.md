# Debounce & Throttle

```js
// 实时搜索 拖拽
function debouce(fn,ms){
    let timer = null;
    return function(){
        clearTimeout(timer);
        timer = setTimeout(()=>{
            fn.apply(this,arguments)
        },ms)
    }
}

// 页面滚动，抢购按钮
function throttle(fn, ms){
    let lastTime = 0;
    return function(){
        let newTime = new Date().getTime();
        if(newTime - lastTime > ms){
            fn.apply(this, arguments);
            lastTime = newTime;
        }
    }
}
```