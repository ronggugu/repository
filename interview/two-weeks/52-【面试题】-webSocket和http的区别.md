> webSocket

```
支持端对端通讯
可以由client发起，也可以由serve发起
用于：消息通知，直播间讨论区，聊天室，协同编辑
```

> webSocket连接过程

```
先发起一个HTTP请求
成功之后再升级到WebSocket协议，再通讯
```

> 区别

```
WebSocket协议名是ws://，可双端发起请求
WebSocket没有跨域限制
通过send和onmessage通讯（HTTP通过req和res）
```

> ws可升级成wss，类似HTTPS，也是加密的功能

> 实际项目推荐使用socket.io，API更简洁

> 【连环问】 WebSocket和HTTP长轮询的区别

```
HTTP长轮询：客户端发起请求，服务端阻塞，不会立即返回
WebSocket：客户端可发起请求，服务端也可发起请求
```