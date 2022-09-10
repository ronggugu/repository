> 建立TCP连接

```
先建立连接(确保双方都有收发消息的能力)
再传输内容(如发送给一个get请求)
网络连接是Tcp协议，传输内容是HTTP协议
```

> 三次握手，建立连接

```
Client发包，Server接收。Server:有Client要找我
Server发包，Client接收。Client:Server已经收到消息
Client发包，Server接收。Server:Client要准备发送了
```

> 四次挥手，关闭连接

```
Client发包，Server接收。Server:Client已请求结束
Server发包，Client接收。Client:Server已收到，我等待它关闭
Server发包，Client接收。Client:Server此时可以关闭连接了
Client发包，Server接收。Server:可以关闭了（然后关闭连接）
```