> key 和 value

```
for...in 遍历到key
for...of 遍历到value
```

> 试用不同的数据类型

```
遍历对象：for...of不可以，for...in可以
遍历Map,Set：for..of可以，for...in不可以
遍历generator：for...of可以，for...in不可以
//Generator 函数是 ES6 提供的一种异步编程解决方案(不常用)
```

> 可枚举 vs 可迭代

```
for...in用于可枚举数据，如对象、数组、字符串,得到key
for...of用于可迭代数据，如数组、字符串、Map、Set,得到value
```