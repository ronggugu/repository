> node 和 element

```
DOM是一棵树，所有节点都是Node
Node是Element的基类
Element是其他HTML元素的基类，如HTMLDivElement
```

> HTMLCollection 和 NodeList

```
HTMLCollection 是Element的集合
NodeList是Node集合
```

> 扩展：类数组->数组

```
//HTMLCollection和NodeList都不是数组，而是类数组
//将类数组 变成 数组
const arr1 = Array.from(list)
const arr2 = Array.protptype.slice.call(list)
const arr3 = [...list]
```