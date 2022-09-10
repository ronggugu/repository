> cookie

```
HTTP无状态，每次请求都要带cookie,以帮助识别身份
服务端也可以向客户端set-cookie,cookie大小限制4kb
默认有跨域限制：不可跨域共享、传递cookie(如果需要则在客户端与服务端都设置withCredentials)
```

> 现代浏览器开始禁止第三方cookie

```
和跨域限制不同，这里是：禁止网页引入的第三方JS设置cookie
打击第三方广告，保护用户隐私
新增属性SameSite:Strict/Lax/None;值可自己选择
```

> cookie和session

```
cookie用于登录验证，存储用户标识（如userId）
session在服务端，存储用户详细信息，和cookie信息一一对应
cookie+session是常见登录验证解决方案
```

> cookie vs token

```
cookie是HTTP规范，而token是自定义传递
cookie会默认被浏览器存储，而token需自己存储
token默认没有跨域限制
```

> JWT(JSON Web Token) 可替代cookie与session

```
前端发起登录，后端验证成功之后，返回一个加密的token
前端自行存储这个token（其中包含了用户信息，加密）
以后访问服务端接口，都带着这个token，作为用户信息
```

> 答案

```
cookie: HTTP标准；跨域限制；配合session使用
token: 无标准；无跨域限制；用于JWT
```

> 【连环问】session和JWT哪个更好用？

```
session优点：
原理简单，易于学习
用户信息存储在服务端，可快速封禁某个用户

session缺点：
占用服务端内存，硬件成本高
多进程，多服务器时，不好同步——需要使用第三方缓存，若redis
默认有跨域限制

JWT优点：
不占用服务端内存
多进程、多服务器 不受影响
没有跨域限制

JWT缺点：
用户信息存储在客户端，无法快速封禁某用户
玩意服务端密钥被泄露，则用户信息全部丢失
token体积一般大于cookie，会增加请求的数据量
```

> 答案

```
如有严格管理用户信息的需求（保密、快速封禁）推荐Session
如没有特殊要求，则使用JWT（如创业初期的网站）
```

> 【连环问】如何实现SSO单点登录

```
基于cookie
cookie默认不可跨域共享，但有些情况可设置为共享
主域名相同，如www.baidu.com  image.baidu.com
设置cookie domain为主域名，即可共享cookie

主域名完全不同，则cookie无法共享，则需要使用SSO(第三方)
```