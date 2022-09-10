> 单线程 和 异步

```
JS是单线程的（无论浏览器还是nodejs）
浏览器中JS执行和DOM渲染共用一个线程
解决办法 -> 异步
```

> 宏任务 和 微任务

```javascript
//宏任务，如setTimeout setInterval网络请求
//微任务，如promise asyn/awiat
//微任务在下一轮DOM渲染之前执行，宏任务在之后执行
console.log('start')
setTimeout(() => {
    console.log('timeout')
})
Promise.resolve().then(() => {
    console.log('promise then')
})
console.log('end')
//打印结果 同步-> 微任务队列 -> 宏任务队列
//start -> end -> promise then -> timeout

// Event Loop 继续监听...

// 宏任务 MarcoTask Queue
// () => {
//     console.log('timeout')
// }
// fn

// DOM 渲染

// 微任务 MicroTask Queue
// () => {
//     console.log('promise then')
// }
``` 

> nodejs 异步

```
nodejs同样使用ES语法，也是单线程，也需要异步
异步任务也分：宏任务 + 微任务
但是，它的宏任务和微任务，分不同类型，有不同优先级
```

- 宏任务优先级（由高到低）
    - Timers - setTimeout setInerval
    - I/O callbacks - 处理网络、流、TCP的错误回调
    - Idle,prepare - 闲置状态（nodejs内部使用）
    - Poll轮询 - 执行poll中的I/O队列
    - Check检查 - 存储 setImmediate 回调
    - Close callbacks - 关闭回调，如socket.on('close')

- 微任务优先级
    - 包括：promise,async/await,process.nextTick
    - 注意：process.nextTick优先级最高

```javascript
console.info('start')
setImmediate(() => {
    console.info('setImmediate')
})
setTimeout(() => {
    console.info('timeout')
})
Promise.resolve().then(() => {
    console.info('promise then')
})
process.nextTick(() => {
    console.info('nextTick')
})
console.info('end')
//执行结果
//start -> end -> nextTick -> promise then -> timeout -> setImmediate
```

> 答案

```
浏览器和nodejs中的event loop 流程基本相同
nodejs宏任务和微任务分类型，有优先级
注意：推荐使用setImmediate代替process.nextTick
```