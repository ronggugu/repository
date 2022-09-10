```
JS上线时要压缩、混淆
线上的JS报错信息，将无法识别行、列
sourcemap就可以解决这个问题
```

> webpack通过devtool配置sourcemap

```
eval - JS在eval(...)中，不生成sourcemap
source-map - 生成单独的map文件，并在JS最后指定
eval-source-map ,JS在eval(...)中，sourcemap内嵌
inline-source-map - sourcemap内嵌到JS中
cheap-source-map - sourcemap中只有行信息，没有列
eval-cheap-source-map - 同上，没有独立的map文件
```

> 推荐

```
开发环境 - eval, eval-source-map, eval-cheap-source-map
线上环境 - source-map, 无
```