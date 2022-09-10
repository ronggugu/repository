> 三者都用于网络请求，但是不同维度
>  - Ajax(Asynchronous Javascript and XML)，一种技术统称
> - Fetch，一个具体的API
> - Axios，第三方库[https://axios-http.com/](https://axios-http.com/)

> 面试题：用XMLHttpRequest实现Ajax

```javascript
function ajax1(url, successFn) {
    const xhr = new XMLHttpRequest()
    xhr.open("GET", url, false)
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            if (xhr.status == 200) {
                successFn(xhr.responseText)
            }
        }
    }
    xhr.send(null)
}
```

> Fetch
> - 浏览器原生API，用于网络请求
> - 和XMLHttpRequest一个级别
> - Fetch语法更加简洁、易用，支持Promise

```javascript
function ajax2(url) {
    return fetch(url).then(res => res.json())
}
```

> Axios
> - 最常用的网络请求lib(库)(随着Vue火爆起来)
> - 内部可用XMLHttpRequest和Fetch来实现
> - Axios，第三方库

> 划重点

```
API和lib的区别
实际项目中，使用现成的lib，尽量不要自己造轮子
但读源码、造轮子是很好的个人学习方式
```