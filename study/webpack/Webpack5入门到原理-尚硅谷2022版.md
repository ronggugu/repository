[TOC]

## 一、基础
[Webpack5入门到原理-尚硅谷2022版](https://dwmorning.github.io/webpack5-docs/)
### 1. 基本使用

`Webpack` 是一个静态资源打包工具。

它会以一个或多个文件作为打包的入口，将我们整个项目所有文件编译组合成一个或多个文件输出出去。

输出的文件就是编译好的文件，就可以在浏览器段运行了。

我们将 `Webpack` 输出的文件叫做 `bundle`。

#### 1.1 功能介绍

Webpack 本身功能是有限的:

*   开发模式：仅能编译 JS 中的 `ES Module` 语法
*   生产模式：能编译 JS 中的 `ES Module` 语法，还能压缩 JS 代码

#### 1.2 开始使用

1.  资源目录

<!---->

    webpack_code # 项目根目录（所有指令必须在这个目录运行）
        └── src # 项目源码目录
            ├── js # js文件目录
            │   ├── count.js
            │   └── sum.js
            └── main.js # 项目主文件

1.  创建文件

```javascript
//count.js
export default function count(x, y) {
  return x - y;
}

//sum.js
export default function sum(...args) {
  return args.reduce((p, c) => p + c, 0);
}

//main.js
import count from "./js/count";
import sum from "./js/sum";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));

```

1.  下依依赖

<!---->

    npm init -y
    //需要注意的是 package.json 中 name 字段不能叫做 webpack, 否则下一步会报错
    npm i webpack webpack-cli -D

1.  启用 Webpack

```
//开发者模式
npx webpack ./src/main.js --mode=development
//生产者模式
npx webpack ./src/main.js --mode=development

```

### 2. 基本配置

5个核心概念

*   entry（入口） 指示 Webpack 从哪个文件开始打包
*   output（输出） 指示 Webpack 打包完的文件输出到哪里去，如何命名等
*   loader（加载器） webpack 本身只能处理 js、json 等资源，其他资源需要借助 loader，Webpack 才能解析
*   plugins（插件） 扩展 Webpack 的功能
*   mode（模式） 主要由两种模式： 开发模式：development 生产模式：production

#### 2.1 准备Webpack 配置文件

```javascript
// Node.js的核心模块，专门用来处理文件路径
const path = require("path");

module.exports = {
  // 入口
  // 相对路径和绝对路径都行
  entry: "./src/main.js",
  // 输出
  output: {
    // path: 文件输出目录，必须是绝对路径
    // path.resolve()方法返回一个绝对路径
    // __dirname 当前文件的文件夹绝对路径
    path: path.resolve(__dirname, "dist"),
    // filename: 输出文件名
    filename: "main.js",
  },
  // 加载器
  module: {
    rules: [],
  },
  // 插件
  plugins: [],
  // 模式
  mode: "development", // 开发模式
};
```

运行命令`npx webpack`

### 3. 开发模式介绍

这个模式下我们主要做两件事：

*   编译代码，使浏览器能识别运行
    开发时我们有样式资源、字体图标、图片资源、html 资源等，webpack 默认都不能处理这些资源，所以我们要加载配置来编译这些资源

*   代码质量检查，树立代码规范
    提前检查代码的一些隐患，让代码运行时能更加健壮。
    提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。

### 4. 处理样式资源

配置,包含css\less\sass\styl

```javascript
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};

```

注意需要下载的包

    css-loader style-loader
    less less-loader
    sass sass-loader
    stylus-loader

### 5. 处理图片资源

配置

```javascript
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024 // 小于10kb的图片会被base64处理
          }
        }
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

### 6. 修改输出资源的名称和路径

配置

```diff
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
+    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
  },
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
+        generator: {
+          // 将图片文件输出到 static/imgs 目录中
+         // 将图片文件命名 [hash:8][ext][query]
+         // [hash:8]: hash值取8位
+         // [ext]: 使用之前的文件扩展名
+         // [query]: 添加之前的query参数
+         filename: "static/imgs/[hash:8][ext][query]",
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};

```

### 7. 自动清空上次打包资源

```diff
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js",
+    clean: true, // 自动将上次打包目录资源清空
  },
  module: {
    rules: [
    ],
  },
  plugins: [],
  mode: "development",
};

```

### 8.处理字体图标资源

```diff
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
  },
  module: {
    rules: [
+    {
+       test: /\.(ttf|woff2?)$/,
+       type: "asset/resource",
+       generator: {
+         filename: "static/media/[hash:8][ext][query]",
+       },
+     },
    ],
  },
  plugins: [],
  mode: "development",
};
```

### 9. 处理js资源

#### 9.1 Eslint

配置文件由很多种写法：

*   .eslintrc.\*：新建文件，位于项目根目录
*   .eslintrc
*   .eslintrc.js
*   .eslintrc.json

区别在于配置格式不一样
package.json 中 eslintConfig：不需要创建文件，在原有文件基础上写
ESLint 会查找和自动读取它们，所以以上配置文件只需要存在一个即可

`.eslintrc.js配置`

```javascript
module.exports = {
  // 解析选项
  parserOptions: {},
  // 具体检查规则
  rules: {},
  // 继承其他规则
  extends: [],
  // ...
  // 其他规则详见：https://eslint.bootcss.com/docs/user-guide/configuring
};


//parserOptions 解析选项
parserOptions: {
  ecmaVersion: 6, // ES 语法版本
  sourceType: "module", // ES 模块化
  ecmaFeatures: { // ES 其他特性
    jsx: true // 如果是 React 项目，就需要开启 jsx 语法
  }
}


//rules 具体规则
//"off" 或 0 - 关闭规则
//"warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
//"error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)
rules: {
  semi: "error", // 禁止使用分号
  'array-callback-return': 'warn', // 强制数组方法的回调函数中有 return 语句，否则警告
  
  'default-case': [
    'warn', // 要求 switch 语句中有 default 分支，否则警告
    { commentPattern: '^no default$' } // 允许在最后注释 no default, 就不会有警告了
  ],
  eqeqeq: [
    'warn', // 强制使用 === 和 !==，否则警告
    'smart' // https://eslint.bootcss.com/docs/rules/eqeqeq#smart 除了少数情况下不会有警告
  ],
}
```

`在webpack中使用`

*   下载包

<!---->

    npm i eslint-webpack-plugin eslint -D

*   配置webpack.config.js

```diff
+ const ESLintWebpackPlugin = require("eslint-webpack-plugin");
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [],
  },
  plugins: [
+    new ESLintWebpackPlugin({
+     // 指定检查文件的根目录
+      context: path.resolve(__dirname, "src"),
+    }),
  ],
  mode: "development",
 }
```

#### 9.2 Babel

配置文件由很多种写法：

*   babel.config.\*：新建文件，位于项目根目录
    *   babel.config.js
    *   babel.config.json
*   .babelrc.\*：新建文件，位于项目根目录
    *   .babelrc
    *   .babelrc.js
    *   .babelrc.json
*   package.json 中 babel：不需要创建文件，在原有文件基础上写

Babel 会查找和自动读取它们，所以以上配置文件只需要存在一个即可

##### 9.2.1 配置

`babel.config.js`

```javascript
module.exports = {
    presets: ["@babel/preset-env"],
};
```

简单理解：就是一组 Babel 插件, 扩展 Babel 功能

*   @babel/preset-env: 一个智能预设，允许您使用最新的 JavaScript。
*   @babel/preset-react：一个用来编译 React jsx 语法的预设
*   @babel/preset-typescript：一个用来编译 TypeScript 语法的预设

`下载包`

    npm i babel-loader @babel/core @babel/preset-env -D

`配置webpack.config.js`

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
+     {
+       test: /\.js$/,
+       exclude: /node_modules/, // 排除node_modules代码不编译
+       loader: "babel-loader",
+     },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "src"),
    }),
  ],
  mode: "development",
};
```

### 10. 处理 Html 资源

`下载包`

    npm i html-webpack-plugin -D

`配置webpack.config.js`

```diff
+ const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [],
  },
  plugins: [
+    new HtmlWebpackPlugin({
+     // 以 public/index.html 为模板创建文件
+     // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
+     template: path.resolve(__dirname, "public/index.html"),
+   }),
  ],
  mode: "development",
};
```

*   注意点：index.html中不再引用打包文件

### 11. 开发服务器&自动化

下载包 `npm i webpack-dev-server -D`
`配置webpack.config.js`

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [],
  },
  plugins: [],
+  // 开发服务器
+ devServer: {
+   host: "localhost", // 启动服务器域名
+   port: "3000", // 启动服务器端口号
+   open: true, // 是否自动打开浏览器
+ },
  mode: "development",
};
```

运行命令`npx webpack serve`

### 12.生产模式介绍

`文件目录`

    ├── webpack-test (项目根目录)
        ├── config (Webpack配置文件目录)
        │    ├── webpack.dev.js(开发模式配置文件)
        │    └── webpack.prod.js(生产模式配置文件)
        ├── node_modules (下载包存放目录)
        ├── src (项目源码目录，除了html其他都在src里面)
        │    └── 略
        ├── public (项目html文件)
        │    └── index.html
        ├── .eslintrc.js(Eslint配置文件)
        ├── babel.config.js(Babel配置文件)
        └── package.json (包的依赖管理配置文件)

*   修改文件修改 webpack.dev.js中所有路径
*   执行

<!---->

    npx webpack serve --config ./config/webpack.dev.js

### 13. CSS处理

#### 13.1 提取 Css 成单独文件

下载包`npm i mini-css-extract-plugin -D`
`配置webpack.prod.js`

```diff
+ const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../dist"), // 生产模式需要输出
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
    clean: true,
  },
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
-       use: ["style-loader", "css-loader", "sass-loader"],
+       use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
+       //所有style-loader都要修改，省略
    ],
  },
  plugins: [
+   // 提取css成单独文件
+   new MiniCssExtractPlugin({
+     // 定义输出文件名和目录
+     filename: "static/css/main.css",
+   }),
  ],
  mode: "production",
};
```

#### 13.2 CSS兼容性处理

*   下载包

<!---->

    npm i postcss-loader postcss postcss-preset-env -D

*   配置webpack.prod.js

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
+         {
+           loader: "postcss-loader",
+           options: {
+             postcssOptions: {
+               plugins: [
+                 "postcss-preset-env", // 能解决大多数样式兼容性问题
+               ],
+             },
+           },
+        },
+        //在所有css-loader之后，在xx-loader之前添加
        ],
      },
    ],
  },
  plugins: [],
  mode: "production",
};
```

*   控制兼容性
    `package.json`

```javascript
{
  // 其他省略
  "browserslist": ["ie >= 8"]
}

//或者

{
  // 其他省略
  "browserslist": ["last 2 version", "> 1%", "not dead"]
}
```

*   合并处理

```diff
+ // 获取处理样式的Loaders
+ const getStyleLoaders = (preProcessor) =>  {
+   return [
+     MiniCssExtractPlugin.loader,
+     "css-loader",
+     {
+       loader: "postcss-loader",
+       options: {
+         postcssOptions: {
+           plugins: [
+             "postcss-preset-env", // 能解决大多数样式兼容性问题
+           ],
+         },
+       },
+     },
+     preProcessor,
+   ].filter(Boolean);
+ };

module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: getStyleLoaders(),
      },
      {
        test: /\.less$/,
+       use: getStyleLoaders("less-loader"),
      },
      {
        test: /\.s[ac]ss$/,
+       use: getStyleLoaders("sass-loader"),
      },
      {
        test: /\.styl$/,
+       use: getStyleLoaders("stylus-loader"),
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          // 将图片文件输出到 static/imgs 目录中
          // 将图片文件命名 [hash:8][ext][query]
          // [hash:8]: hash值取8位
          // [ext]: 使用之前的文件扩展名
          // [query]: 添加之前的query参数
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
      {
        test: /\.(ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "static/media/[hash:8][ext][query]",
        },
      },
      {
        test: /\.js$/,
        exclude: /node_modules/, // 排除node_modules代码不编译
        loader: "babel-loader",
      },
    ],
  },
  plugins: [],
  mode: "production",
};
```

#### 13.3 CSS压缩

*   下载包
    `npm i css-minimizer-webpack-plugin -D`
*   配置webpack.prod.js

```diff
+ const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [],
  },
  plugins: [
    // css压缩
+     new CssMinimizerPlugin(),
  ],
  mode: "production",
};
```

## 二、高级

### 1. 提升开发体验

#### 1.1 SourceMap

*   为什么

所有 css 和 js 合并成了一个文件，并且多了其他代码。此时如果代码运行出错那么提示代码错误位置我们是看不懂的。一旦将来开发代码文件很多，那么很难去发现错误出现在哪里。

所以我们需要更加准确的错误提示，来帮助我们更好的开发代码

*   是什么

SourceMap（源代码映射）是一个用来生成源代码与构建后代码一一映射的文件的方案。

它会生成一个 xxx.map 文件，里面包含源代码和构建后代码每一行、每一列的映射关系。当构建后代码出错了，会通过 xxx.map 文件，从构建后代码出错位置找到映射后源代码出错位置，从而让浏览器提示源代码文件出错位置，帮助我们更快的找到错误根源

*   怎么用

开发模式：cheap-module-source-map

优点：打包编译速度快，只包含行映射
缺点：没有列映射

```js
module.exports = {
  // 其他省略
  mode: "development",
  devtool: "cheap-module-source-map",
};
```

生产模式：source-map
优点：包含行/列映射
缺点：打包编译速度更慢

```js
module.exports = {
  // 其他省略
  mode: "production",
  devtool: "source-map",
};
```

### 2. 提升打包构建速度

#### 2.1 HotModuleReplacement

*   为什么

开发时我们修改了其中一个模块代码，Webpack 默认会将所有模块全部重新打包编译，速度很慢。

所以我们需要做到修改某个模块代码，就只有这个模块代码需要重新打包编译，其他模块不变，这样打包速度就能很快。

*   是什么

HotModuleReplacement（HMR/热模块替换）：在程序运行中，替换、添加或删除模块，而无需重新加载整个页面。

*   怎么用

基本配置

```diff
module.exports = {
  // 其他省略
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
+   hot: true, // 开启HMR功能（只能用于开发环境，生产环境不需要了）
  },
};
```

此时 css 样式经过 style-loader 处理，已经具备 HMR 功能了。 但是 js 还不行。

JS 配置

```js
// main.js
import count from "./js/count";
import sum from "./js/sum";
// 引入资源，Webpack才会对其打包
import "./css/iconfont.css";
import "./css/index.css";
import "./less/index.less";
import "./sass/index.sass";
import "./sass/index.scss";
import "./styl/index.styl";

const result1 = count(2, 1);
console.log(result1);
const result2 = sum(1, 2, 3, 4);
console.log(result2);

// 判断是否支持HMR功能
if (module.hot) {
  module.hot.accept("./js/count.js", function (count) {
    const result1 = count(2, 1);
    console.log(result1);
  });

  module.hot.accept("./js/sum.js", function (sum) {
    const result2 = sum(1, 2, 3, 4);
    console.log(result2);
  });
}
```

上面这样写会很麻烦，所以实际开发我们会使用其他 loader 来解决。

比如：vue-loader, react-hot-loader

#### 2.2 OneOf

*   为什么

打包时每个文件都会经过所有 loader 处理，虽然因为 test 正则原因实际没有处理上，但是都要过一遍。比较慢。

*   是什么

顾名思义就是只能匹配上一个 loader, 剩下的就不匹配了。

*   怎么用

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
+     {
+       oneOf: [
          {
            // 用来匹配 .css 结尾的文件
            test: /\.css$/,
            // use 数组里面 Loader 执行顺序是从右到左
            use: ["style-loader", "css-loader"],
          },
          {
            test: /\.less$/,
            use: ["style-loader", "css-loader", "less-loader"],
          },
          {
            test: /\.s[ac]ss$/,
            use: ["style-loader", "css-loader", "sass-loader"],
          },
          {
            test: /\.styl$/,
            use: ["style-loader", "css-loader", "stylus-loader"],
          },
          {
            test: /\.(png|jpe?g|gif|webp)$/,
            type: "asset",
            parser: {
              dataUrlCondition: {
                maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
              },
            },
            generator: {
              // 将图片文件输出到 static/imgs 目录中
              // 将图片文件命名 [hash:8][ext][query]
              // [hash:8]: hash值取8位
              // [ext]: 使用之前的文件扩展名
              // [query]: 添加之前的query参数
              filename: "static/imgs/[hash:8][ext][query]",
            },
          },
          {
            test: /\.(ttf|woff2?)$/,
            type: "asset/resource",
            generator: {
              filename: "static/media/[hash:8][ext][query]",
            },
          },
          {
            test: /\.js$/,
            exclude: /node_modules/, // 排除node_modules代码不编译
            loader: "babel-loader",
          },
+       ],
+     },
    ],
  },
  plugins: [],
  mode: "development",
};
```

生产模式也是如此配置

#### 2.3 Include/Exclude

*   为什么

开发时我们需要使用第三方的库或插件，所有文件都下载到 node\_modules 中了。而这些文件是不需要编译可以直接使用的。

所以我们在对 js 文件处理时，要排除 node\_modules 下面的文件。

*   是什么

    *   include
        包含，只处理 xxx 文件

    *   exclude
        排除，除了 xxx 文件以外其他文件都处理

*   怎么用

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
-           // exclude: /node_modules/, // 排除node_modules代码不编译
+           include: path.resolve(__dirname, "../src"), // 也可以用包含
            loader: "babel-loader",
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
+     exclude: "node_modules", // 默认值
    }),
  ],
  mode: "development",
};
```

生产模式也是如此配置。

#### 2.4 Cache

*   为什么

每次打包时 js 文件都要经过 Eslint 检查 和 Babel 编译，速度比较慢。

我们可以缓存之前的 Eslint 检查 和 Babel 编译结果，这样第二次打包时速度就会更快了。

*   是什么

对 Eslint 检查 和 Babel 编译结果进行缓存。

*   怎么用

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            loader: "babel-loader",
+           options: {
+             cacheDirectory: true, // 开启babel编译缓存
+             cacheCompression: false, // 缓存文件不要压缩
+           },
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
+     cache: true, // 开启缓存
+     // 缓存目录
+     cacheLocation: path.resolve(__dirname,"../node_modules/.cache/.eslintcache"),
    }),
  ],
  // 开发服务器
  mode: "development",
};
```

#### 2.5 Thead

*   为什么

当项目越来越庞大时，打包速度越来越慢，甚至于需要一个下午才能打包出来代码。这个速度是比较慢的。

我们想要继续提升打包速度，其实就是要提升 js 的打包速度，因为其他文件都比较少。

而对 js 文件处理主要就是 eslint 、babel、Terser 三个工具，所以我们要提升它们的运行速度。

我们可以开启多进程同时处理 js 文件，这样速度就比之前的单进程打包更快了。

*   是什么

多进程打包：开启电脑的多个进程同时干一件事，速度更快。

需要注意：请仅在特别耗时的操作中使用，因为每个进程启动就有大约为 600ms 左右开销。

*   怎么用
    我们启动进程的数量就是我们 CPU 的核数。

如何获取 CPU 的核数，因为每个电脑都不一样。

```js
// nodejs核心模块，直接使用
const os = require("os");
// cpu核数
const threads = os.cpus().length;
```

`下载包`

    npm i thread-loader -D

`使用`

```diff
+ const os = require("os");
+ const TerserPlugin = require("terser-webpack-plugin");

+ // cpu核数
+ const threads = os.cpus().length;
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
+           use: [
+             {
+               loader: "thread-loader", // 开启多进程
+               options: {
+                 workers: threads, // 数量
+               },
+             },
+             {
+               loader: "babel-loader",
+               options: {
+                 cacheDirectory: true, // 开启babel编译缓存
+               },
+             },
+           ],
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
      cache: true, // 开启缓存
      // 缓存目录
      cacheLocation: path.resolve(
        __dirname,
        "../node_modules/.cache/.eslintcache"
      ),
+     threads, // 开启多进程
    }),
  ],
+ optimization: {
+   minimize: true,
+   minimizer: [
+     // css压缩也可以写到optimization.minimizer里面，效果一样的
+     new CssMinimizerPlugin(),
+     // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
+     new TerserPlugin({
+       parallel: threads // 开启多进程
+     })
+   ],
+ },
  mode: "production",
  devtool: "source-map",
};
```

### 3. 减少代码体积

#### 3.1 Tree Shaking

*   为什么

开发时我们定义了一些工具函数库，或者引用第三方工具函数库或组件库。

如果没有特殊处理的话我们打包时会引入整个库，但是实际上可能我们可能只用上极小部分的功能。

这样将整个库都打包进来，体积就太大了。

*   是什么

Tree Shaking 是一个术语，通常用于描述移除 JavaScript 中的没有使用上的代码。

注意：它依赖 ES Module。

*   怎么用

Webpack 已经默认开启了这个功能，无需其他配置。

#### 3.2 Babel

*   为什么

Babel 为编译的每个文件都插入了辅助代码，使代码体积过大！

Babel 对一些公共方法使用了非常小的辅助代码，比如 \_extend。默认情况下会被添加到每一个需要它的文件中。

你可以将这些辅助代码作为一个独立模块，来避免重复引入。

*   是什么

@babel/plugin-transform-runtime: 禁用了 Babel 自动对每个文件的 runtime 注入，而是引入 @babel/plugin-transform-runtime 并且使所有辅助代码从这里引用。

*   怎么用

下载包

`npm i @babel/plugin-transform-runtime -D`
配置

```diff
module.exports = {
  entry: "./src/main.js",
  output: {},
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            use: [
              {
                loader: "thread-loader", // 开启多进程
                options: {
                  workers: threads, // 数量
                },
              },
              {
                loader: "babel-loader",
                options: {
                  cacheDirectory: true, // 开启babel编译缓存
                  cacheCompression: false, // 缓存文件不要压缩
+                 plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积
                },
              },
            ],
          },
        ],
      },
    ],
  },
  plugins: [],
  optimization: {},
  mode: "production",
  devtool: "source-map",
};
```

#### 3.3 Image Minimizer

*   为什么

开发如果项目中引用了较多图片，那么图片体积会比较大，将来请求速度比较慢。

我们可以对图片进行压缩，减少图片体积。

注意：如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。

*   是什么

image-minimizer-webpack-plugin: 用来压缩图片的插件

*   怎么用

下载包

    npm i image-minimizer-webpack-plugin imagemin -D

还有剩下包需要下载，有两种模式：

无损压缩

    npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D

有损压缩

    npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D

[有损/无损压缩的区别](https://baike.baidu.com/item/%E6%97%A0%E6%8D%9F%E3%80%81%E6%9C%89%E6%8D%9F%E5%8E%8B%E7%BC%A9)

配置
我们以无损压缩配置为例：

```diff
+ const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");
module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../dist"), // 生产模式需要输出
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
    clean: true,
  },
  module: {
    rules: [],
  },
  plugins: [],
  optimization: {
    minimizer: [
      // css压缩也可以写到optimization.minimizer里面，效果一样的
      new CssMinimizerPlugin(),
      // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
      new TerserPlugin({
        parallel: threads, // 开启多进程
      }),
+     // 压缩图片
+     new ImageMinimizerPlugin({
+       minimizer: {
+         implementation: ImageMinimizerPlugin.imageminGenerate,
+         options: {
+           plugins: [
+             ["gifsicle", { interlaced: true }],
+             ["jpegtran", { progressive: true }],
+             ["optipng", { optimizationLevel: 5 }],
+             [
+               "svgo",
+               {
+                 plugins: [
+                   "preset-default",
+                   "prefixIds",
+                   {
+                     name: "sortAttrs",
+                     params: {
+                       xmlnsOrder: "alphabetical",
+                     },
+                   },
+                 ],
+               },
+             ],
+           ],
+         },
+       },
+     }),
    ],
  },
  mode: "production",
  devtool: "source-map",
};
```

### 4. 优化代码运行性能

#### 4.1 Code Split

*   打包代码时会将所有 js

文件打包到一个文件中，体积太大了。我们如果只要渲染首页，就应该只加载首页的 js 文件，其他文件不应该加载。

所以我们需要将打包生成的文件进行代码分割，生成多个 js 文件，渲染哪个页面就只加载某个 js 文件，这样加载的资源就少，速度就更快。

*   是什么

代码分割（Code Split）主要做了两件事：

&#x20;==1. 分割文件==：将打包生成的文件进行分割，生成多个 js 文件。&#x20;

==2.按需加载==：需要哪个文件就加载哪个文件。

*   怎么用
    *   1. 多入口
 下载包
```
npm i webpack webpack-cli html-webpack-plugin -D
```
    *   2. 提取重复代码
    *   3. 按需加载，动态导入
    *   4. 单入口
    *   5. 更新配置
    *   6. 给动态导入文件取名称

    

