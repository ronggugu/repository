```javascript
//for await ...of 用于遍历多个Promise
//作用和Promise.all作用一致
function createPromise(val) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(val)
        }, 1000)
    })
}
//同时执行
(async function () {
    const p1 = createPromise(100)
    const p2 = createPromise(200)
    const p3 = createPromise(300)

    // const res1 = await p1
    // console.log(res1)
    // const res2 = await p2
    // console.log(res2)
    // const res3 = await p3
    // console.log(res3)

    const list = [p1, p2, p3]
    // Promise.all(list).then(res => console.log(res))
    for await (let res of list) {
        console.log(res)
    }
})()
//顺序执行
(async function () {
    //const res1 = await createPromise(100)
    //console.log(res1)
    //const res2 = await createPromise(200)
    //console.log(res2)
    //const res3 = await createPromise(300)
    //console.log(res3)

    const arr = [10, 20, 30]
    for (let num of arr) {
        const res = await createPromise(num)
        console.log(res)
    }
})()
```