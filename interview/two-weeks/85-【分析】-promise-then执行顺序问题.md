> promise.then执行顺序

```javascript
Promise.resolve().then(() => {
    console.log(0)
    return Promise.resolve(4)
}).then((res) => {
    console.log(res)
})

Promise.resolve().then(() => {
    console.log(1)
}).then(() => {
    console.log(2)
}).then(() => {
    console.log(3)
}).then(() => {
    console.log(5)
}).then(() =>{
    console.log(6)
})
```

> then交替执行

```
如果有多个fulfilled promise实例，同时执行then链式调用
then会交替执行
这是编译器的优化，防止一个promise占据太久时间
```

> then中返回promise实例

```
相当于多出一个promise实例
也会遵循“交替执行”
但和直接申明一个promise实例，结果有差异
```

> 差异- “慢两拍”

```
then中返回promise实例，会出现“慢两拍”的效果
第一拍：promise需要由pending编程fulfilled
第二拍：then函数挂载到MicroTaskQueue(参考Event Loop)
```

> 模拟“慢两拍”

```javascript
 Promise.resolve().then(()=> {
    // 第一拍
    const p = Promise.resolve(100)
    Promise.resolve().then(() => {
        // 第二拍
        p.then(res => console.log(res))
        .then(() => {console.log(200)})
    })
})
```