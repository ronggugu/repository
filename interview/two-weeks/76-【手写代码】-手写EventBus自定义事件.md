> 手写EventBus自定义事件

```
on once
emit
off
```

> 分析

```
on和once注册函数，存储起来
emit时找到对应的函数，执行
off找到对应的函数，从对象中删除
```

> 注意区分on和once

```
on绑定的事件可以连续执行，除非off
once绑定的函数emit一次即删除，也可以未执行而被off
数据结构上要标识出on和once
```

```javascript
export default class EventBus {
    /**
     * {
     *    'key1': [
     *        { fn: fn1, isOnce: false },
     *        { fn: fn2, isOnce: false },
     *        { fn: fn3, isOnce: true },
     *    ]
     *    'key2': [] // 有序
     *    'key3': []
     * }
     */
    private events: {
        [key: string]: Array<{fn: Function; isOnce: boolean}>
    }

    constructor() {
        this.events = {}
    }

    on(type: string, fn: Function, isOnce: boolean = false) {
        const events = this.events
        if (events[type] == null) {
            events[type] = [] // 初始化 key 的 fn 数组
        }
        events[type].push({ fn, isOnce })
    }

    once(type: string, fn: Function) {
        this.on(type, fn, true)
    }

    off(type: string, fn?: Function) {
        if (!fn) {
            // 解绑所有 type 的函数
            this.events[type] = []
        } else {
            // 解绑单个 fn
            const fnList = this.events[type]
            if (fnList) {
                this.events[type] = fnList.filter(item => item.fn !== fn)
            }
        }
    }

    emit(type: string, ...args: any[]) {
        const fnList = this.events[type]
        if (fnList == null) return

        // 注意
        this.events[type] = fnList.filter(item => {
            const { fn, isOnce } = item
            fn(...args)

            // once 执行一次就要被过滤掉
            if (!isOnce) return true
            return false
        })
    }
}
```

> Event Bus - 拆分保存 on 和 once 事件

```javascript
export default class EventBus2 {
    private events: { [key: string]: Array<Function> } // { key1: [fn1, fn2], key2: [fn1, fn2] }
    private onceEvents: { [key: string]: Array<Function> }

    constructor() {
        this.events = {}
        this.onceEvents = {}
    }

    on(type: string, fn: Function) {
        const events = this.events
        if (events[type] == null) events[type] = []
        events[type].push(fn)
    }

    once(type: string, fn: Function) {
        const onceEvents = this.onceEvents
        if (onceEvents[type] == null) onceEvents[type] = []
        onceEvents[type].push(fn)
    }

    off(type: string, fn?: Function) {
        if (!fn) {
            // 解绑所有事件
            this.events[type] = []
            this.onceEvents[type] = []
        } else {
            // 解绑单个事件
            const fnList = this.events[type]
            const onceFnList = this.onceEvents[type]
            if (fnList) {
                this.events[type] = fnList.filter(curFn => curFn !== fn)
            }
            if (onceFnList) {
                this.onceEvents[type] = onceFnList.filter(curFn => curFn !== fn)
            }
        }
    }

    emit(type: string, ...args: any[]) {
        const fnList = this.events[type]
        const onceFnList = this.onceEvents[type]

        if (fnList) {
            fnList.forEach(f => f(...args))
        }
        if (onceFnList) {
            onceFnList.forEach(f => f(...args))

            // once 执行一次就删除
            this.onceEvents[type] = []
        }
    }
}
```

> 功能测试

```javascript
const e = new EventBus()

function fn1(a: any, b: any) { console.log('fn1', a, b) }
function fn2(a: any, b: any) { console.log('fn2', a, b) }
function fn3(a: any, b: any) { console.log('fn3', a, b) }

e.on('key1', fn1)
e.on('key1', fn2)
e.once('key1', fn3)
e.on('xxxxxx', fn3)

e.emit('key1', 10, 20) // 触发 fn1 fn2 fn3

e.off('key1', fn1)

e.emit('key1', 100, 200) // 触发 fn2
```

> 单元测试

```javascript
import EventBus from './event-bus-split-on-once'

describe('EventBus 自定义事件', () => {
    it('绑定事件，触发事件', () => {
        const event = new EventBus()

        // 注意
        const fn1 = jest.fn() // jest mock function
        const fn2 = jest.fn()
        const fn3 = jest.fn()

        event.on('key1', fn1)
        event.on('key1', fn2)
        event.on('xxxx', fn3)

        event.emit('key1', 10, 20)

        expect(fn1).toBeCalledWith(10, 20)
        expect(fn2).toBeCalledWith(10, 20)
        expect(fn3).not.toBeCalled()
    })

    it('解绑单个事件', () => {
        const event = new EventBus()

        const fn1 = jest.fn()
        const fn2 = jest.fn()

        event.on('key1', fn1)
        event.on('key1', fn2)

        event.off('key1', fn1)

        event.emit('key1', 10, 20)

        expect(fn1).not.toBeCalled()
        expect(fn2).toBeCalledWith(10, 20)
    })

    it('解绑所有事件', () => {
        const event = new EventBus()

        const fn1 = jest.fn()
        const fn2 = jest.fn()

        event.on('key1', fn1)
        event.on('key1', fn2)

        event.off('key1') // 解绑所有事件

        event.emit('key1', 10, 20)

        expect(fn1).not.toBeCalled()
        expect(fn2).not.toBeCalled()
    })

    it('once', () => {
        const event = new EventBus()

        let n = 1

        const fn1 = jest.fn(() => n++)
        const fn2 = jest.fn(() => n++)

        event.once('key1', fn1)
        event.once('key1', fn2)

        // 无论 emit 多少次，只有一次生效
        event.emit('key1')
        event.emit('key1')
        event.emit('key1')
        event.emit('key1')
        event.emit('key1')

        expect(n).toBe(3)
    })
})
```
