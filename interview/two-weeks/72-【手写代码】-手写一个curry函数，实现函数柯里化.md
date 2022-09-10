```javascript
export function curry(fn: Function) {
    const fnArgsLength = fn.length // 传入函数的参数长度
    let args: any[] = []

    // ts 中，独立的函数，this 需要声明类型
    function calc(this: any, ...newArgs: any[]) {
        // 积累参数
        args = [
            ...args,
            ...newArgs
        ]
        if (args.length < fnArgsLength) {
            // 参数不够，返回函数
            return calc
        } else {
            // 参数够了，返回执行结果
            return fn.apply(this, args.slice(0, fnArgsLength))
        }
    }

    return calc
}

```

> 功能测试

```javascript
function add(a: number, b: number, c: number): number {
     return a + b + c
}
// add(10, 20, 30) // 60

const curryAdd = curry(add)
const res = curryAdd(10)(20)(30) // 60
console.info(res)
```

> 单元测试
```javascript
import { curry } from './curry'

describe('curry', () => {
    it('curry add', () => {
        function add(a: number, b: number, c: number): number {
            return a + b + c
        }
        const res1 = add(10, 20, 30)

        const curryAdd = curry(add)
        const res2 = curryAdd(10)(20)(30)

        expect(res1).toBe(res2)
    })
})
```