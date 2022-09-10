> 什么时LRU缓存

```
LRU   -    Least Recently Used最近使用
如果内存优先，只缓存最近使用的，删除“沉水”数据
核心API两个：get set
```

> 分析

```
用哈希表存储数据，这样get set才够快O(1)
必须是有序的，常用数据放在前面，“沉水”数据放在后面
哈希表+有序，就是Map——其他都不行
```

```javascript
export default class LRUCache {
    private length: number
    private data: Map<any, any> = new Map()

    constructor(length: number) {
        if (length < 1) throw new Error('invalid length')
        this.length = length
    }

    set(key: any, value: any) {
        const data = this.data

        if (data.has(key)) {
            data.delete(key)
        }
        data.set(key, value)

        if (data.size > this.length) {
            // 如果超出了容量，则删除 Map 最老的元素
            const delKey = data.keys().next().value
            data.delete(delKey)
        }
    }

    get(key: any): any {
        const data = this.data

        if (!data.has(key)) return null

        const value = data.get(key)

        data.delete(key)
        data.set(key, value)

        return value
    }
}
```

> 功能测试

```javascript
const lruCache = new LRUCache(2)
lruCache.set(1, 1) // {1=1}
lruCache.set(2, 2) // {1=1, 2=2}
console.info(lruCache.get(1)) // 1 {2=2, 1=1}
lruCache.set(3, 3) // {1=1, 3=3}
console.info(lruCache.get(2)) // null
lruCache.set(4, 4) // {3=3, 4=4}
console.info(lruCache.get(1)) // null
console.info(lruCache.get(3)) // 3 {4=4, 3=3}
console.info(lruCache.get(4)) // 4 {3=3, 4=4}
```

> 单元测试

```javascript
import LRUCache from './LRU'

describe('LRU cache', () => {
    it('set get', () => {
        const lru = new LRUCache(2)
        lru.set('1', 100)
        lru.set('2', 200)
        expect(lru.get('1')).toBe(100)
        expect(lru.get('2')).toBe(200)
    })

    it('set 超出容量', () => {
        const lru = new LRUCache(2)
        lru.set('1', 100)
        lru.set('2', 200)
        lru.set('1', 101) // 重新 set
        lru.set('3', 300)
        expect(lru.get('1')).toBe(101)
        expect(lru.get('2')).toBeNull()
        expect(lru.get('3')).toBe(300)
    })

    it('get 超出容量', () => {
        const lru = new LRUCache(2)
        lru.set('1', 100)
        lru.set('2', 200)
        lru.get('1')
        lru.set('3', 300)
        expect(lru.get('1')).toBe(100)
        expect(lru.get('2')).toBeNull()
        expect(lru.get('3')).toBe(300)
    })
})
```
