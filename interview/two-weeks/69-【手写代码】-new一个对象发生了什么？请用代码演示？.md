> class是function的语法糖

> new一个对象的过程

```
创建一个空对象obj,继承构造函数的原型
执行构造函数(将obj作为this)
返回obj
```

> 实现

```javascript
export function customNew<T>(constructor: Function, ...args: any[]): T {
    // 1. 创建一个空对象，继承 constructor 的原型
    const obj = Object.create(constructor.prototype)
    // 2. 将 obj 作为 this ，执行 constructor ，传入参数
    constructor.apply(obj, args)
    // 3. 返回 obj
    return obj
}
```

> 功能测试

```javascript
const f = customNew<Foo>(Foo, '双越', 100)
console.info(f)
console.info(f.getName())
```

> 单元测试

```javascript
import { customNew } from './new'

describe('自定义 new',() => {
    it('new', () => {
        class Foo {
            // 属性
            name: string
            city: string
            n: number

            constructor(name: string, n: number) {
                this.name = name
                this.city = '北京'
                this.n = n
            }

            getName() {
                return this.name
            }
        }
        const f = customNew<Foo>(Foo, '双越', 100)
        expect(f.name).toBe('双越')
        expect(f.city).toBe('北京')
        expect(f.n).toBe(100)
        expect(f.getName()).toBe('双越')
    })
})

```

> 【连环问】Object.create和{}有什么区别？

```
{}创建空对象，原型指向Object.prortotype
Object.create创建空对象，原型指向传入的参数
```