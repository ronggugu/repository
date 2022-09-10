> 前端统计范围

```
访问量PV
自定义事件
性能，错误
```

```javascript
 const PV_URL_SET = new Set()

class MyStatistic {
    constructor(productId) {
        this.productId = productId

        this.initPerformance() // 性能统计
        this.initError() // 错误监控
    }

    // 发送统计数据
    send(url, params = {}) {
        params.productId = productId

        const paramArr = []
        for (let key in params) {
            const val = params[key]
            paramArr.push(`${key}=${value}`)
        }

        const newUrl = `${url}?${paramArr.join('&')}` // url?a=10&b=20

        // 用 <img> 发送：1. 可跨域；2. 兼容性非常好
        const img = document.createElement('img')
        img.src = newUrl // get
    }

    // 初始化性能统计
    initPerformance() {
        const url = 'yyy'
        this.send(url, performance.timing) // 给最原始的、完整的结果，原始数据
    }

    // 初始化错误监控
    initError() {
        window.addEventListener('error', event => {
            const { error, lineno, colno } = event
            this.error(error, { lineno, colno })
        })
        // Promise 未 catch 住的报错
        window.addEventListener('unhandledrejection', event => {
            this.error(new Error(event.reason), { type: 'unhandledrejection' })
        })
    }

    pv() {
        const href = location.href
        if (PV_URL_SET.get(href)) return // 不重复发送 pv

        this.event('pv')

        PV_URL_SET.add(href)
    }

    event(key, val) {
        const url = 'xxx' // 自定义事件统计 server API
        this.send(url, {key, val})
    }

    error(err, info = {}) {
        const url = 'zzz'
        const { message, stack } = err
        this.send(url, { message, stack, ...info  })
    }
}
```