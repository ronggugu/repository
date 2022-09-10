> bind应用

```
返回一个新函数，但不执行
绑定this和部分参数
如果是箭头函数，无法改变this,只能改变参数
```

```javascript
// @ts-ignore
Function.prototype.customBind = function (context: any, ...bindArgs: any[]) {
    // context 是 bind 传入的 this
    // bindArgs 是 bind 传入的各个参数

    const self = this // 当前的函数本身

    return function (...args: any[]) {
        // 拼接参数
        const newArgs = bindArgs.concat(args)
        return self.apply(context, newArgs)
    }
}
```

> 功能测试

```javascript
function fn(this: any, a: any, b: any, c: any) {
    console.info(this, a, b, c)
}
// @ts-ignore
const fn1 = fn.customBind({x: 100}, 10)
fn1(20, 30)
```

> 单元测试

```javascript
import './bind'
describe('自定义 bind', () => {
    it('绑定 this', () => {
        function fn(this: any) {
            return this
        }
        // @ts-ignore
        const fn1 = fn.customBind({x: 100})
        expect(fn1()).toEqual({x: 100})
    })
    it('绑定参数', () => {
        function fn(a: number, b: number, c: number) {
            return a + b + c
        }
        // @ts-ignore
        const fn1 = fn.customBind(null, 10, 20)
        expect(fn1(30)).toBe(60)
    })
})
```