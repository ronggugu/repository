![image](https://note.youdao.com/yws/public/resource/488f3bf54a29a2eee5561c5e8bba83dc/xmlnote/WEBRESOURCE1d478aacf0a551c974a83465c75a0dfa/1627)

> 答案

```
无：HTML暂停解析，下载JS，执行JS,再继续解析HTML
defer: HTML继续解析，并行下载JS，HTML解析完在执行JS
async:HTML继续解析，并行下载JS，执行JS，再解析HTML
```

> [连环问] prefetch和dns-prefetch有什么区别？

```
preload资源在当前页面使用，会优先加载
prefetch资源再未来页面使用，空闲时加载
dns-prefetch 即DNS预查询
preconnect 即DNS预连接
```

```html
<!--preload-->
<link rel="preload" href="style.css" as ="style">
<link rel="preload" href="main.js" as ="script">

<!--prefetch-->
<link rel="prefetch" href="other.js" as ="script">

<!--dns-prefetch-->
<link rel="dns-prefetch" href="https://fonts.gstatic.com" > 

<!--preconnect-->
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```