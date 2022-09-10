> 进程 process VS 线程 thread

```
进程，OS进行资源分配和调度的最小单位，由独立内存空间
线程，OS进行运算调度的最小单位，共享进程内存空间
JS是单线程的，但可以开启多进程执行，如WwebWorker
```

> 为什么需要多进程？

```
多核CPU，更适合处理多进程
内存较大，多个进程才能更好的利用（单进程有内存上限）
总之，“压榨”机器资源，更快，更节省
```

> nodejs多进程-fork

```javascript
//主进程process-fork.js
const http = require('http')
const fork = require('child_process').fork

const server = http.createServer((req, res) => {
    if (req.url === '/get-sum') {
        console.info('主进程 id', process.pid)

        // 开启子进程
        const computeProcess = fork('./compute.js')
        computeProcess.send('开始计算')

        computeProcess.on('message', data => {
            console.info('主进程接受到的信息：', data)
            res.end('sum is ' + data)
        })

        computeProcess.on('close', () => {
            console.info('子进程因报错而退出')
            computeProcess.kill()
            res.end('error')
        })
    }
})
server.listen(3000, () => {
    console.info('localhost: 3000')
})

//子进程compute.js
function getSum() {
    let sum = 0
    for (let i = 0; i < 10000; i++) {
        sum += i
    }
    return sum
}

process.on('message', data => {
    console.log('子进程 id', process.pid)
    console.log('子进程接受到的信息: ', data)

    const sum = getSum()

    // 发送消息给主进程
    process.send(sum)
})
```

> nodejs多进程-cluster(集群)

```javascript

```