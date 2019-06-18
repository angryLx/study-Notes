# Something About Js

[JS编码优化](#JS编码优化)

[Promise](#Promise)


## JS编码优化
`1. 严格强类型检查`
```js
0 === false // false
0 == false // 可能会转为true
```
`2. 使用默认参数替代'||'操作`，但是默认参数并不能取代`空字符串、null`
``` js
function getName(name) {
    let firstName = name || 'Jhon'
    // ...
}
getName(null)   // Jhon
getName('')   // Jhon
getName(undefined)  // Jhon

// =============================

function getName(name = 'Jhon') {
    let firstName = name
    // ...
}
getName(null)   // null
getName('')  // 
getName(undefined)  // Jhon
```

## Promise

#### `1. 同步`
``` js
var x = true
while(x)
console.log('finish') // 不会执行该行代码
```
x为true，那么while就为死循环，同步造成 [**线程阻塞**] ，因此console将不会执行

#### `2. 异步`

可同时执行多个任务。
```js
setTimeout(function() {
    console.log('task A')
}, 0)
console.log('task B')
// while(true)
//======= console ===========
task B
task A
```
`异步任务会在当前脚本的所有同步任务执行完才会执行`，若上述代码的while取消注释了，那么setTimeout将不会执行，因为同步阻塞了进程。

#### `3. Promise基本用法`

Promise对象代表一个未完成、但预计将来会完成的操作。

注：`Promise`一旦新建就会**立即执行**，无法取消。

主要分为三种状态：

*  `pending`： 初始值，非fulfilled，也非rejected
*  `fulfilled`： 操作成功
*  `rejected`： 操作失败
```js
// 构建promise
var promise = new Promise((resolve, reject) => {
    if(true) {
        // 异步操作成功
        resolve()
    } else {
        // 异步操作失败
        reject()
    }
})

/* 写法promise.then(onFulfilled, onRejected);
 * 第二个入参非必选
 */
promise.then(function(data) {
    // 成功的操作，data为成功的结果
}, function(error) {
    // 失败的操作
})
```
`resolve` 异步操作成功后的回调，并将结果作为参数传出去；

`reject` 异步操作失败后的回调，并将结果作为参数传出去。

`4.1 .then()`

对promise添加onFulfilled和onRejected回调，并返回一个新的promise实例，且返回值将最为参数传入新的promise的resolve函数。
```js
function request(url, params, success, fail) {
     $.ajax({
        type: 'post',
        url: url,
        param: params,
        async: true,    //默认为true,即异步请求；false为同步请求
        success: success,
        error: fail
    });
}
function sendRequest(url, params) {
    return new Promise(function(resolve,reject) {
        request(url, params, resolve, reject)
    })
}
sendRequest('/getUserInfo', {}).then(data1 => {
    console.log('第一次请求成功，返回data1')
    return sendRequest('/getClassList', data1.uid).then(data2 => {
        console.log('第二次请求成功，返回data2')
    })
}).catch((err) => {
    console.log('请求失败，')
})
```

promise构造函数是同步执行的，then方法是异步执行的

```js
const promise = new Promise((resolve, reject) => {
    console.log(1);
    resolve(5)
    console.log(2);
}).then(val => {
    console.log(val);
})
promise.then(() => {
    console.log(3);
})
console.log(4);

// 执行结果：12453

```
#### `4.2 .catch()`

用于指定发生错误时的回调函数.

即promise.then(onFulfilled, onRejected)中的onFulfilled为undefined;
```js
promise.then(data => {
    console.log('success')
}).catch(err => {
    console.log('error' + err)
})

// ======== 等同于 =========

promise.then(data => {
    console.log('success')
}).catch(undefined, err => {
    console.log('error' + err)
})
```
#### `4.3 .all()`
用于将多个Promise实例，包装成一个新的。
```js
var p1 = new Promise((resolve,reject) => {
    setTimeout(()=> {
        resolve('first')
    }, 3000)
})
var p2 = new Promise((resolve,reject) => {
    resolve('second')
})
var p3 = new Promise((resolve,reject) => {
    setTimeout(()=> {
        resolve('third')
    }, 1000)
})
Promise.all([p1, p2, p3]).then((data) => {
    console.log(data)   // ["first", "second", "third"]
}).catch(err => {
    console.log(err)
})
```
#### `4.4 .race()`
