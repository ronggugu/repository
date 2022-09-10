> 使用JSON.stringify 和 parse

```
局限性：
无法转换函数
无法转换Map,Set
无法转换循环引用
```

> 普通深拷贝

```javascript
//只考虑Object Array
//无法转换Map Set和循环引用
//只能应对初级要求的技术一面

/**
 * 深拷贝 - 只考虑了简单的数组、对象
 * @param obj obj
 */
function cloneDeep(obj: any) {
    if (typeof obj !== 'object' || obj == null ) return obj

    let result: any
    if (obj instanceof Array) {
        result = []
    } else {
        result = {}
    }

    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {            
            result[key] = cloneDeep(obj[key]) // 递归调用
        }
    }

    return result
}
```

> 资深、专业的 深拷贝

```javascript
/**
 * 深拷贝
 * @param obj obj
 * @param map weakmap 为了避免循环引用
 */
export function cloneDeep(obj: any, map = new WeakMap()): any {
    if (typeof obj !== 'object' || obj == null ) return obj

    // 避免循环引用
    const objFromMap = map.get(obj)
    if (objFromMap) return objFromMap

    let target: any = {}
    map.set(obj, target)

    // Map
    if (obj instanceof Map) {
        target = new Map()
        obj.forEach((v, k) => {
            const v1 = cloneDeep(v, map)
            const k1 = cloneDeep(k, map)
            target.set(k1, v1)
        })
    }

    // Set
    if (obj instanceof Set) {
        target = new Set()
        obj.forEach(v => {
            const v1 = cloneDeep(v, map)
            target.add(v1)
        })
    }

    // Array
    if (obj instanceof Array) {
        target = obj.map(item => cloneDeep(item, map))
    }

    // Object
    for (const key in obj) {
        const val = obj[key]
        const val1 = cloneDeep(val, map)
        target[key] = val1
    }

    return target
}
```

> 功能测试

```javascript
// 功能测试
const a: any = {
    set: new Set([10, 20, 30]),
    map: new Map([['x', 10], ['y', 20]]),
    info: {
        city: '北京'
    },
    fn: () => { console.info(100) }
}
a.self = a
console.log( cloneDeep(a) )
```

> 单元测试

```javascript
import { cloneDeep } from './clone-deep'

describe('深拷贝', () => {
    it('值类型', () => {
        expect(cloneDeep(100)).toBe(100)
        expect(cloneDeep('abc')).toBe('abc')
        expect(cloneDeep(null)).toBe(null)
    })
    it('普通对象和数组', () => {
        const obj = {
            name: '双越',
            info: {
                city: '北京'
            },
            arr: [10, 20, 30]
        }
        const obj1 = cloneDeep(obj)
        obj.info.city = '上海'

        expect(obj1.info.city).toBe('北京')
        expect(obj1.arr).toEqual([10, 20, 30])
    })
    it('Map', () => {
        const m1 = new Map([['x', 10], ['y', 20]])
        const m2 = cloneDeep(m1)
        expect(m2.size).toBe(2)

        const obj = {
            map: new Map([['x', 10], ['y', 20]])
        }
        const obj1 = cloneDeep(obj)
        expect(obj1.map.size).toBe(2)
    })
    it('Set', () => {
        const s1 = new Set([10, 20, 30])
        const s2 = cloneDeep(s1)
        expect(s2.size).toBe(3)

        const obj = {
            s: new Set([10, 20, 30])
        }
        const obj1 = cloneDeep(obj)
        expect(obj1.s.size).toBe(3)
    })
    it('循环引用', () => {
        const a: any = {}
        a.self = a

        const b = cloneDeep(a)
        expect(b.self).toBe(b)
    })
})

```