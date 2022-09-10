> 名词解释

```
SPA - Sing-page Application 单页面应用
MPA - Muti-page Application 多页面应用
默认情况下，Vue，React都是SPA
```

> MPA案例

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'production',
  // 多入口
  entry: {
    home: './src/home/index.js',
    product: './src/product/index.js',
    about: './src/about/index.js'
  },
  output: {
    filename: 'js/[name].[contentHash].js', // name 即 entry 的 key
    path: path.resolve(__dirname, './dist')
  },
  plugins: [
    // 三个页面
    new HtmlWebpackPlugin({
      title: '首页',
      template: './template/index.html',
      filename: 'home.html',
      chunks: ['home']
    }),
    new HtmlWebpackPlugin({
      title: '产品',
      template: './template/product.html',
      filename: 'product.html',
      chunks: ['product']
    }),
    new HtmlWebpackPlugin({
      title: '关于',
      template: './template/about.html',
      filename: 'about.html',
      chunks: ['about']
    })
  ]
}
```

> SPA特点

```
功能较多，一个页面展示不完
以操作为主，非展示为主
适合一个综合Web应用
```

> SPA场景

```
大型后台管理系统，如阿里云console
知识库，如语雀，石墨文档
比较复杂的WebApp 如外卖H5
```

> MPA特点

```
功能较少，一个页面展示的完
以展示为主，操作较少
```

> MPA场景

```
分享页，如腾讯文档分享出去
新闻详情页，如新闻App的详情页，微信公众号发布的页面
```