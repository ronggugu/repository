>

```javascript
/**
 * 访问节点
 * @param n node
 */
function visitNode(n: Node) {
    if (n instanceof Comment) {
        // 注释
        console.info('Comment node ---', n.textContent)
    }
    if (n instanceof Text) {
        // 文本
        const t = n.textContent?.trim()
        if (t) {
            console.info('Text node ---', t)
        }
    }
    if (n instanceof HTMLElement) {
        // element
        console.info('Element node ---', `<${n.tagName.toLowerCase()}>`)
    }
}

/**
 * 深度优先遍历
 * @param root dom node
 */
function depthFirstTraverse1(root: Node) {
    visitNode(root)

    const childNodes = root.childNodes // .childNodes 和 .children 不一样
    if (childNodes.length) {
        childNodes.forEach(child => {
            depthFirstTraverse1(child) // 递归
        })
    }
}

/**
 * 深度优先遍历
 * @param root dom node
 */
 function depthFirstTraverse2(root: Node) {
     const stack: Node[] = []

     // 根节点压栈
     stack.push(root)

     while (stack.length > 0) {
         const curNode = stack.pop() // 出栈
         if (curNode == null) break

         visitNode(curNode)

         // 子节点压栈
         const childNodes = curNode.childNodes
         if (childNodes.length > 0) {
             // reverse 反顺序压栈
             Array.from(childNodes).reverse().forEach(child => stack.push(child))
         }
     }
 }

/**
 * 广度优先遍历
 * @param root dom node
 */
function breadthFirstTraverse(root: Node) {
    const queue: Node[] = [] // 数组 vs 链表

    // 根节点入队列
    queue.unshift(root)

    while (queue.length > 0) {
        const curNode = queue.pop()
        if (curNode == null) break

        visitNode(curNode)

        // 子节点入队
        const childNodes = curNode.childNodes
        if (childNodes.length) {
            childNodes.forEach(child => queue.unshift(child))
        }
    }
}
```

> 【连环问】深度优先遍历可以不用遍历吗？

```
可以使用栈实现
因为递归的本质就是栈
```

> 递归 VS 非递归

```
递归逻辑更加清晰，但更容易发生stack overflow错误
非递归效率更高，但逻辑比较复杂
```