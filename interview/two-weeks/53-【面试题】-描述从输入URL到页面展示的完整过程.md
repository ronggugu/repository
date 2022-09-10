> 步骤

```
网络请求
    DNS查询（得到IP），建立TCP连接（三次握手）
    浏览器发起HTTP请求
    收到请求响应，得到HTML源代码
    在解析HTML过程中，遇到静态资源还会继续发起网络请求，比如JS，CSS,图片，视频等（注：静态资源可能有强缓存，则不必请求）
    
解析：字符串->结构化数据
    HTML->DOM树
    CSS->CSSOM树
    两者结合形成render树
    
渲染：render Tree 绘制到页面
    计算各个DOM的尺寸，定位，最后绘制到页面
    遇到JS可能会执行（参考defer async）
    异步CSS、图片加载，可能会触重新渲染
```

```
优化解析：
css放在<head>中，不要异步加载css
JS放在<body>最下面（或合理使用defer async）
<img>提前定义width height
```