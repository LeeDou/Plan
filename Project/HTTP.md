# HTTP

## HTTP的基本优化
影响一个HTTP网络请求的主要因素有两个： 带宽和延迟
- 带宽：与网络设施有关
- 延迟：
  - 浏览器阻塞（HOL blocking）：浏览器会因为一些原因阻塞请求。浏览器对于同一个域名，同时只能有4个连接（根据浏览器内核不同可能会有所差异），超过浏览器最大连接数限制，后续请求就会被阻塞。
  - DNS 查询（DNS Lookup）： 需要通过DNS解析域名为IP才能建立连接。通过DNS缓存结果减少时间。
  - 建立连接（initial connection）： HTTP是基于TCP协议的，浏览器最快也要在第三次握手时才能捎带HTTP请求报文，达到真正的建立连接，但这些连接无法复用会导致妹子请求都经历三次握手和慢启动。

## HTTP1.0 和HTTP1.1 的一些区别
1. 缓存处理，HTTP1.0 使用header里 If-modified-since, expires 来作为缓存判断的标准，HTTP1.1 则引入可更多的缓存控制策略如 Entity tag， If-unmodified-since，If-match， If-none-match 等更多的缓存策略。
2. 带宽优化及网络连接的使用，HTTP1.0中客户端值需要某对象的一部分，服务器会将整个对象送过来，HTTP1.1中请求头引入了range，他允许只请求资源的某个部分，返回码是206.
3. 错误通知的管理，HTTP1.1中新增了24个错误状态响应码，如409（conflict）表示请求的资源与资源的当前状态发生冲突；410（gone）表示服务器上的某个资源被永久性删除。
4. host头处理，在HTTP1.0中认为每台服务器都绑定一个唯一的IP地址，因此，请求消息中的URL并没有传递主机名（hostname）。HTTP1.1的请求消息和响应消息都支持host头域，且请求消息中如果没有host头域，且请求消息中如果没有host头域会报告一个错误（400 Bad Request）
5. 长连接，HTTP1.1 支持场链接(PersistentConnection)和请求的流水（Pipelining）处理，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立连接和关闭连接的消耗和延迟，在HTTP1.1 中默认开启connection： keep-alive。

### SPDY
与HTTP1.x 相比，SPDY做的优化
- 降低延迟，采取多路复用
- 请求优先级，给每个request设置优先级，重要请求可以优先得到响应
- header压缩
- 基于HTTPS的加密传输协议，提高数据的可靠性
- 服务端推送

### HTTP2.0
HTTP2.0 是SPDY的升级版
- HTTP2.0 支持明文HTTP传输
- 采用HPACK对header进行压缩
- HTTP2.0 采用二进制帧进行传输，HTTP1.x基于文本
- 服务端推送
- 多路复用
