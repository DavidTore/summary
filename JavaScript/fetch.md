# Fetch

## 用法

- 使用Promise
- 采用模块化设计，包括Response/Request/Headers
- 通过数据流对象（stream）处理数据，分块读取，减少内存占用，而+XMLHttpRequest不支持数据流
  
    `fetch(url[, optionObj])`

``` javascript
fetch(url).then(...).catch(...)

fetch('htpps://api.github.com')
    .then(response => response.json())
        .then(json => console.log(json))
            .catch(err => console.log(err))

async function getJSON(url){
    try{
        let res = await fetch(url);
        return await res.json();
    } catch(err){}
}
/**
* fetch()请求成功后得到的是一个Response对象
*/
```

## Response对象

Response对象标头信息属性
| Props               | Notes                                                        |
| :------------------ | :----------------------------------------------------------- |
| Response.ok         | true对应 HTTP 请求的状态码 200 到 299，false对应其他的状态码 |
| Response.status     | 表示 HTTP 回应的状态码（例如200，表示成功请求）              |
| Response.statusText | HTTP 回应的状态信息（例如请求成功以后，服务器返回"OK"）      |
| Response.url        | /                                                            |
| Response.type       | 详见下列                                                     |

- basic：普通请求，即同源请求。
- cors：跨域请求。
- error：网络错误，主要用于 Service Worker。
- opaque：如果fetch()请求的mode属性设为no-cors，就会返回这个值，详见请求部分。表示发出的是简单的跨域请求，类似\<form\>表单的那种跨域请求。
- opaqueredirect：如果fetch()请求的redirect属性设为manual，就会返回这个值，详见请求部分。

>fetch()发出请求以后，只有网络错误，或者无法连接时，fetch()才会报错，其他情况都不会报错，而是认为请求成功。Promise 不会变为 rejected状态。

| Response 读取方法      | Notes                     |
| :--------------------- | :------------------------ |
| response.text()        | 文本字符串                |
| response.json()        | JSON 对象                 |
| response.blob()        | 二进制 Blob 对象          |
| response.arrayBuffer() | 二进制 ArrayBuffer 对象。 |

注意：读取方法都是`异步`的， 返回的都是`Promise对象`，stream对象`只能读取一次`，可以用 `Response.clone()`创建副本
`Response.body`返回一个`ReadaleStream对象`，可以用来分块读取内容（可用在显示下载进度）

## 第二个参数

fetch(url[, optionObj])

```js
const response = await fetch('/fetch/post/user',{
    method:'POST',
    headers: { },
    body: JSON.stringify({ name:'John'})
    //  body: 'foo=bar'
    //  body: new FormData(form)
    /*  let blob = await new Promise(resolve =>   
  canvasElem.toBlob(resolve,  'image/png') ); body: blob */
})
```

## 相关API

```js
const response = fetch(url, {
  method: "GET",
  headers: {
    "Content-Type": "text/plain;charset=UTF-8"
  },
  body: undefined,
  referrer: "about:client",
  referrerPolicy: "no-referrer-when-downgrade",
  mode: "cors", 
  credentials: "same-origin",
  cache: "default",
  redirect: "follow",
  integrity: "",
  keepalive: false,
  signal: undefined
});
```

## 取消fetch()请求

`fetch()`请求发送以后，如果中途想要`取消`，需要使用`AbortController`对象

```js
let controller = new AbortController();
setTimeout(() => controller.abort(), 1000);  // 1s后取消发送

try {
  let response = await fetch('/long-operation', {
    signal: controller.signal
  });
} catch(err) {
  if (err.name == 'AbortError') {
    console.log('Aborted!');
  } else {
    throw err;
  }
}
```