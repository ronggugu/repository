> beforeCreate

```
创建一个空白的Vue实例
data、method尚未被初始化，不可使用
```

> created

```
Vue实例初始化完成，完成响应式绑定
data、method被初始化，可使用
尚未开始渲染模板
```

> beforeMount

```
编译模板，调用render生成vdom
还没有开始渲染模板
```


> mounted

```
完成DOM渲染
组件创建完成
开始由“创建阶段”进入“运行阶段”
```

> beforeUpdate

```
data发生变化之后
准备更新DOM(尚未更新DOM)
```

> updataed

```
data发生变化，且DOM更新完成
不要在updataed中修改data,可能会导致死循环
```

> beforeUnmount

```
组件进入销毁阶段（尚未销毁，可正常使用）
可移除、解绑一些全局事件、自定义事件
```

> unmounted

```
组件被销毁了
所有子组件也被销毁了
```

> keep-alive 组件

```
onActivated缓存组件被激活
onDeactivated缓存组件被隐藏
```

> [连环问]Vue什么时候操作DOM比较合适

```
mounted和updated都不能保证子组件全部挂载
使用$nextTick渲染DOM
mounted(()=>{
    this.$nextTick(function(){
        
    })
})
```

> [连环问]Ajax应该在那个生命周期

```
有两个选择，created和mounted
推荐mounted
```

> [连环问]Vue3 Composition API 生命周期有何区别？

```
用setup代替了beforeCeate和created
使用Hooks函数的形式，如mouned改成onMounted()
```
