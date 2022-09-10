> 题目描述-获取类型

```
手写一个getType函数，传入任意变量，可准确获取类型
如number string boolean等值类型
还有object array map regexp等引用类型
```

> 常见的类型判断

```
typeof - 只能判断值类型，其他就是function和object
intanceof - 需要两个参数来判断，而不是获取类型
```

> 枚举各种类型

```
通过typeof判断值类型和function
其他引用通过instanceof来逐个判断识别
如x instanceof Map，则返回map
```

> 枚举的坏处

```
可能忽略某些类型
增加了新类型，需要修改代码
```

> 答案

```
使用Object.prototype.toString.call(x)
注意，不能直接调用x.toString()
```

> 方法

```javascript
/**
 * 获取详细的数据类型
 * @param x x
 */
export function getType(x: any): string {
    const originType = Object.prototype.toString.call(x) // '[object String]'
    const spaceIndex = originType.indexOf(' ')
    const type = originType.slice(spaceIndex + 1, -1) // 'String'
    return type.toLowerCase() // 'string'
}
```

> 功能测试

```javascript
console.info( getType(null) ) // 'null'
console.info( getType(undefined) )
console.info( getType(100) )
console.info( getType('abc') )
console.info( getType(true) )
console.info( getType(Symbol()) )
console.info( getType({}) )
console.info( getType([]) )
console.info( getType(() => {}) )
```

> 单元测试

```javascript
import { getType } from './get-type'

describe('获取详细的数据类型', () => {
    it('null', () => {
        expect(getType(null)).toBe('null')
    })
    it('undefined', () => {
        expect(getType(undefined)).toBe('undefined')
    })
    it('number', () => {
        expect(getType(100)).toBe('number')
        expect(getType(NaN)).toBe('number')
        expect(getType(Infinity)).toBe('number')
        expect(getType(-Infinity)).toBe('number')
    })
    it('string', () => {
        expect(getType('abc')).toBe('string')
    })
    it('boolean', () => {
        expect(getType(true)).toBe('boolean')
    })
    it('symbol', () => {
        expect(getType(Symbol())).toBe('symbol')
    })
    it('bigint', () => {
        expect(getType(BigInt(100))).toBe('bigint')
    })
    it('object', () => {
        expect(getType({})).toBe('object')
    })
    it('array', () => {
        expect(getType([])).toBe('array')
    })
    it('function', () => {
        expect(getType(() => {})).toBe('function')
        expect(getType(class Foo {})).toBe('function')
    })
    it('map', () => {
        expect(getType(new Map())).toBe('map')
    })
    it('weakmap', () => {
        expect(getType(new WeakMap())).toBe('weakmap')
    })
    it('set', () => {
        expect(getType(new Set())).toBe('set')
    })
    it('weakset', () => {
        expect(getType(new WeakSet())).toBe('weakset')
    })
    it('date', () => {
        expect(getType(new Date())).toBe('date')
    })
    it('regexp', () => {
        expect(getType(new RegExp(''))).toBe('regexp')
    })
    it('error', () => {
        expect(getType(new Error())).toBe('error')
    })
    it('promise', () => {
        expect(getType(Promise.resolve())).toBe('promise')
    })
})

```