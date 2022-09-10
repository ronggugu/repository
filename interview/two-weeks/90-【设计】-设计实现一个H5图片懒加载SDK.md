> 分析

```
定义<img src=loading.png data-src=xx.png>
页面滚动，图片露出是，将data-src赋值给src
滚动要节流
```

> 获取图片定位

```
元素的位置elem.getBoundingClientRect()
图片top和window.innerHeight
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>img lazy load</title>
    <style>
        .item-container {
            border-top: 1px solid #ccc;
            margin-bottom: 30px;
        }
        .item-container img {
            width: 100%;
            border: 1px solid #eee;
            border-radius: 10px;
            overflow: hidden;
        }
    </style>
</head>
<body>
    <h1>img lazy load</h1>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal1.jpeg"/>
    </div>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal2.webp"/>
    </div>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal3.jpeg"/>
    </div>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal4.webp"/>
    </div>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal5.webp"/>
    </div>

    <div class="item-container">
        <p>新闻标题</p>
        <img src="./img/loading.gif" data-src="./img/animal6.webp"/>
    </div>

    <script src="https://cdn.bootcdn.net/ajax/libs/lodash.js/4.17.21/lodash.min.js"></script>
    <script>
        function mapImagesAndTryLoad() {
            const images = document.querySelectorAll('img[data-src]')
            if (images.length === 0) return

            images.forEach(img => {
                const rect = img.getBoundingClientRect()
                if (rect.top < window.innerHeight) {
                    // 漏出来
                    // console.info('loading img', img.dataset.src)
                    img.src = img.dataset.src
                    img.removeAttribute('data-src') // 移除 data-src 属性，为了下次执行时减少计算成本
                }
            })
        }

        window.addEventListener('scroll', _.throttle(() => {
            mapImagesAndTryLoad()
        }, 100))

        mapImagesAndTryLoad()
    </script>
</body>
</html>
```