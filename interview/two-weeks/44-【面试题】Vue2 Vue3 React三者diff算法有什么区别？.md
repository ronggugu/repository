> 介绍Diff算法

```
diff算法很早就有了
diff算法应用广泛，例如github的pull request中的代码diff
如果严格diff两棵树，时间复杂度O(n^3)，不可用
```

> Tree diff的优化

```
只比较同一层级，不跨级比较
tag不同则删掉重建（不再去比较内部的细节）
子节点通过key区分（key的重要性）
优化后时间复杂度O(n)
```

> React diff 仅右移
![image](https://note.youdao.com/yws/public/resource/488f3bf54a29a2eee5561c5e8bba83dc/xmlnote/WEBRESOURCEb3f88bf323472561e00b48a66345ac82/1470)

> vue2 双端比较
![image](https://note.youdao.com/yws/public/resource/488f3bf54a29a2eee5561c5e8bba83dc/xmlnote/WEBRESOURCEb49fe950974febd3de1ab130651f9889/1475)

> vue3 最长递增子序列
![image](https://note.youdao.com/yws/public/resource/488f3bf54a29a2eee5561c5e8bba83dc/xmlnote/WEBRESOURCE34d5cacb48cc786995573144ceedbe64/1482)
![image](https://note.youdao.com/yws/public/resource/488f3bf54a29a2eee5561c5e8bba83dc/xmlnote/WEBRESOURCE542be6a2d206c6f9174e64fd82e1be62/1486)

> [连环问]vue react 为何循环时必须使用key?

```
vdom diff算法会根据key判断元素是否要删除
匹配了key，则只移动元素——性能良好
未匹配key，则删除重建——性能较差
```