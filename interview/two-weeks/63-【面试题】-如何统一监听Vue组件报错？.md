> window.onerror

```
全局监听所有JS错误
但它是JS级别的，识别不了Vue组件信息
捕捉一些Vue监听不到的错误
但是try...catch捕获的无法监听到
```

> errorCaptured生命周期app.vue

```
监听所有下级组件的错误
返回false会阻止向上传播
```

> errorHandler配置main.js

```
Vue全局错误监听，所有组件错误都会汇总到这里
但errorCaptured反水false，不会传播到这里
```

> 异步错误

```
异步回调里的错误，errorHandler监听不到
需要使用window.onerror
```

> 扩展

```
Promise为处理的catch则需要onunhandledregection
```

> 答案

```
errorCaptured监听下级组件错误，返回false阻止向上传播
errorHandler监听全局Vue组件的错误
window.onerror监听其他JS错误，如异步
```

