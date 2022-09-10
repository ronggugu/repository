> call和apply的应用

```
bind返回一个新函数（不执行），call和apply会立即执行函数
绑定this
传入执行参数
```

> 分析：如何在函数执行时绑定this

```
如 const obj = {x:100,fn(){this.x}}
执行obj.fn(),此时fn内部的this就指向obj
可借此来实现函数绑定this
```

```javascript
// @ts-ignore
Function.prototype.customCall = function (context: any, ...args: any[]) {
    if (context == null) context = globalThis
    if (typeof context !== 'object') context = new Object(context) // 值类型，变为对象

    const fnKey = Symbol() // 不会出现属性名称的覆盖
    context[fnKey] = this // this 就是当前的函数

    const res = context[fnKey](...args) // 绑定了 this

    delete context[fnKey] // 清理掉 fn ，防止污染

    return res
}

// @ts-ignore
Function.prototype.customApply = function (context: any, args: any[] = []) {
    if (context == null) context = globalThis
    if (typeof context !== 'object') context = new Object(context) // 值类型，变为对象

    const fnKey = Symbol() // 不会出现属性名称的覆盖
    context[fnKey] = this // this 就是当前的函数

    const res = context[fnKey](...args) // 绑定了 this

    delete context[fnKey] // 清理掉 fn ，防止污染

    return res
}
```

> 功能测试

```javascript
function fn(this: any, a: any, b: any, c: any) {
    console.info(this, a, b, c)
}
// @ts-ignore
fn.customCall({x: 100}, 10, 20, 30)
// @ts-ignore
fn.customApply({x: 200}, [100, 200, 300])
```

> 单元测试

```javascript
import './call-apply'

describe('自定义 call', () => {
    it('绑定 this - 对象', () => {
        function fn(this: any) {
            return this
        }
        // @ts-ignore
        const res = fn.customCall({x: 100})
        expect(res).toEqual({x: 100})
    })
    it('绑定 this - 值类型', () => {
        function fn(this: any) {
            return this
        }
        // @ts-ignore
        const res1 = fn.customCall('abc')
        expect(res1.toString()).toBe('abc')

        // @ts-ignore
        const res1 = fn.customCall(null)
        expect(res1).not.toBeNull()
    })
    it('绑定参数', () => {
        function fn(a: number, b: number) {
            return a + b
        }
        // @ts-ignore
        const res = fn.customCall(null, 10, 20)
        expect(res).toBe(30)
    })
})

describe('自定义 apply', () => {
    it('绑定 this - 对象', () => {
        function fn(this: any) {
            return this
        }
        // @ts-ignore
        const res = fn.customApply({x: 100})
        expect(res).toEqual({x: 100})
    })
    it('绑定 this - 值类型', () => {
        function fn(this: any) {
            return this
        }
        // @ts-ignore
        const res1 = fn.customApply('abc')
        expect(res1.toString()).toBe('abc')

        // @ts-ignore
        const res1 = fn.customApply(null)
        expect(res1).not.toBeNull()
    })
    it('绑定参数', () => {
        function fn(a: number, b: number) {
            return a + b
        }
        // @ts-ignore
        const res = fn.customApply(null, [10, 20])
        expect(res).toBe(30)
    })
})

```