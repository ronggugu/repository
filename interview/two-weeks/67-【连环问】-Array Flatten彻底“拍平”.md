```
写一个JS函数，实现数组扁平化，减少所有嵌套的层级
如输入[1,[2,[3]],4],输出[1,2,3,4]
```

> 思路

```
先实现一级扁平化，然后递归调用，直到全部扁平
```

<font color=red>`方法一：push`</font>

```javascript
/**
 * 数组深度扁平化，使用 push
 * @param arr arr
 */
export function flattenDeep1(arr: any[]): any[] {
    const res: any[] = []

    arr.forEach(item => {
        if (Array.isArray(item)) {
            const flatItem = flattenDeep1(item) // 递归
            flatItem.forEach(n => res.push(n))
        } else {
            res.push(item)
        }
    })

    return res
}
```

<font color=red>`方法二：concat`</font>

```javascript
/**
 * 数组深度扁平化，使用 concat
 * @param arr arr
 */
export function flattenDeep2(arr: any[]): any[] {
    let res: any[] = []

    arr.forEach(item => {
        if (Array.isArray(item)) {
            const flatItem = flattenDeep2(item) // 递归
            res = res.concat(flatItem)
        } else {
            res = res.concat(item)
        }
    })

    return res
}
```

> 功能测试

```javascript
const arr = [1, [2, [3, ['a', [true], 'b'], 4], 5], 6]
console.info( flattenDeep2(arr) )
```

> 单元测试

```javascript
import { flattenDeep1, flattenDeep2 } from './array-flatten-deep'

describe('数组深度扁平化', () => {
    it('空数组', () => {
        const res = flattenDeep2([])
        expect(res).toEqual([])
    })

    it('非嵌套数组', () => {
        const arr = [1, 2, 3]
        const res = flattenDeep2(arr)
        expect(res).toEqual([1, 2, 3])
    })

    it('一级嵌套', () => {
        const arr = [1, 2, [10, 20], 3]
        const res = flattenDeep2(arr)
        expect(res).toEqual([1, 2, 10, 20, 3])
    })

    it('二级嵌套', () => {
        const arr = [1, 2, [10, [100, 200], 20], 3]
        const res = flattenDeep2(arr)
        expect(res).toEqual([1, 2, 10, 100, 200, 20, 3])
    })

    it('三级嵌套', () => {
        const arr = [1, 2, [10, [100, ['a', [true],'b'], 200], 20], 3]
        const res = flattenDeep2(arr)
        expect(res).toEqual([1, 2, 10, 100, 'a', true, 'b', 200, 20, 3])
    })
})
```
