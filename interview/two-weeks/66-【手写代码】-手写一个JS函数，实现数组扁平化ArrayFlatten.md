> Array Flatten

```
写一个JS函数，实现数组扁平化，之减少一级
如输入[1,[2,[3]],4],输出[1,2,[3],4]
```

> 思路

```
定义空数组arr = [],遍历当前数组
如果item非数组，则累加到arr
如果item是数组，则遍历之后累加到arr
```

<font color=red>`方法一：push`</font>

```javascript
/**
 * 数组扁平化，使用 push
 * @param arr arr
 */
export function flatten1(arr: any[]): any[] {
    const res: any[] = []

    arr.forEach(item => {
        if (Array.isArray(item)) {
            item.forEach(n => res.push(n))
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
 * 数组扁平化，使用 concat
 * @param arr arr
 */
export function flatten2(arr: any[]): any[] {
    let res: any[] = []

    arr.forEach(item => {
        res = res.concat(item)
    })

    return res
}
```

> 功能测试

```javascript
const arr = [1, [2, [3], 4], 5]
console.info( flatten2(arr) )
```

> 单元测试

```javascript
import { flatten1, flatten2 } from './array-flatten'

describe('数组扁平化', () => {
    it('空数组', () => {
        const res = flatten2([])
        expect(res).toEqual([])
    })

    it('非嵌套数组', () => {
        const arr = [1, 2, 3]
        const res = flatten2(arr)
        expect(res).toEqual([1, 2, 3])
    })

    it('一级嵌套', () => {
        const arr = [1, 2, [10, 20], 3]
        const res = flatten2(arr)
        expect(res).toEqual([1, 2, 10, 20, 3])
    })

    it('二级嵌套', () => {
        const arr = [1, 2, [10, [100, 200], 20], 3]
        const res = flatten2(arr)
        expect(res).toEqual([1, 2, 10, [100, 200], 20, 3])
    })
})
```
