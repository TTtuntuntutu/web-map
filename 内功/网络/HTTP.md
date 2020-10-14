HTTP

- 基于`TCP/IP`的应用层通信协议，定义了客户端(client)和服务端(server)如何通讯
- 默认，HTTP端口是`80`，HTTPS端口是`443`



## 时间线

### 1.x

HTTP/0.9：1991

HTTP/1.0：1996

- 缺点：
  - connectionless：一次连接(connection)只能有一次请求(request) => 意味着所有请求都会有延迟损失，严重的性能问题(因为三次握手等)
  - slow start：没有在建立连接后尽快发送所有数据，这个时间让TCP congestion control algorithm去确定可以传输的数据量，避免将无法处理的数据包泛洪道网络中 => 这也说明了带宽（bandwidth）的作用是受限制的
  - stateless：服务端不会记录客户端的信息，所以每次客户端的请求都必须包含完整的信息，这其实是有冗余的

![三次握手](/Immagine/三次握手.png)

HTTP/1.1：1999

- 新引入的特性
  - Persistent Connections
  - Pipelining
  - Chunked Transfers
- 缺点
  - **虽然**HTTP/1.1引入了Persistent Connections(`keep-alive`)和Pipelining，多个请求可以共享同一连接，这样去摊分初始连接建立、slow start的成本。**但是**多个请求仍然必须一个接一个地序列化，因此客户端和服务器相当于只能在任何给定时间还是为每个连接执行一次请求/响应交换。这个时候如果其中某个缓慢或繁重的请求阻塞了，后面的请求就会被阻塞 => 所以这个时候的开发者开始变通方法，css精灵图、单一的css/js文件等，但是在当下一个网页都是如此复杂的情况下，HTTP/1.1开始显得力不从心
  - 简单来说，HTTP/1.1做不到 一个连接并发请求



### 2.x

SPDY：2009

- 说明
  - 这不是一项标准，Google内研的SPDY，孕育了HTTP/2.0的诞生。它通过降低延迟(latency)来提高性能

> For those who don’t know the difference, **latency** is the delay i.e. how  long it takes for data to travel between the source and destination  (measured in milliseconds) and **bandwidth** is the amount of data  transfered per second (bits per second).



HTTP/2：2015

- 新引入特性

  - Binary Protocol：Frames & Streams
  - Multiplexing

    > it uses frames and streams for requests and responses, once a TCP  connection is opened, all the streams are sent asynchronously through  the same connection without opening any additional connections. And in  turn, the server responds in the same asynchronous way i.e. the response has no order and the client uses the assigned stream id to identify the stream to which a specific packet belongs. 

  - HPACK header compression
  - Server Push：服务端可以主动去推客户端需要的资源，通过`PUSH_PROMISE` frame
  - Request Prioritization：客户端可以给请求加优先级，服务端在接收后，根据优先级信息决定提供多少资源处理
  - Security：TLS

- 缺点：虽然HTTP/2解决了一个连接可以发送多个请求问题，但也引入了新的问题

  

  首先要知道HTTP/2是一个二进制协议（Binary Protocol），**一次连接的结构**是这样的：

  - A Connection：
    - A Stream(Frame+Frame+...)
    - A Stream(Frames)
    - ...

  简单来说：即使丢失的数据仅涉及单个请求，所有请求和响应也同样受到数据包丢失（例如由于网络拥塞）的影响

  

  原因是，TCP并不理解HTTP/2一次连接结构的这种抽象，它看到的只是字节流（a stream of bytes ）。它的职责是以正确的顺序从一个端点向另一端点传递整个字节流。当一个TCP数据包丢失时，TCP会在这个流中留一个间隙，重新发送丢失数据包的请求，直到拿到数据包来填充这段间隙。在这个阶段内，即使成功交付的字节属于完全独立的HTTP请求，也不能传递给应用程序。这就带来了不必要的延迟。这就是臭名昭著的 **TCP-based head of line block** 。



### 3.x

我们逐渐来到了2.x的瓶颈。



架构：

- QUIC ≈ TLS(Security Layer) + 部分TCP(Transprot)
- 基于UDP

![TCP_UDP](/Immagine/TCP_UDP.png)



和TCP一样，QUIC streams共享一个QUIC连接，没有额外的握手和slow start。但是区别在于当一个数据包丢失的时候，它仅会影响这个流。

我们熟知的UDP相对于TCP，是不保证可靠交付。而 QUIC (Quick UDP Internet Connections)，就是来解决这个可靠性的问题。此外：

- QUIC实现在用户侧，协议的更新不依赖于操作系统更新，而TCP依赖操作系统更新
- QUIC stream可以简单搬运HTTP-level stream特性，从而获取 部分HTTP/2的优点，而不会有the head-of-line blocking => 所以HTTP/3不会包含这些



至于为什么选择UDP?

相比于TCP，UDP更加灵活；而且UDP和TCP一样被广泛地使用，基于UDP去构建HTTP/3，而不是考虑一个新的传输层协议，是因为这样无需对对接到网络的设备固件、操作系统进行大量改进。





## 补充

### HTTP协议是由谁实现的

答：应用

- [stackoverflow-Where is http protocol implementation in Linux](https://stackoverflow.com/questions/40428474/where-is-http-protocol-implementation-in-linux) 解释了HTTP是基于TCP的应用层协议，TCP由OS实现，HTTP由具体的应用实现，比如浏览器

- [How to implement HTTP/2 stream connection in browser?](https://stackoverflow.com/questions/52273174/how-to-implement-http-2-stream-connection-in-browser) 这里也提到浏览器做了这样的事情，用户对于是否使用HTTP/1.1还是HTTP/2是无感知的：

  >  That’s part of the beauty of the way it was implemented - the browser  takes care of all this and the web page and web application has no need  to know whether HTTP/1.1 or HTTP/2 was used.



### **RESTful programming** 

**Re**presentational **S**tate **T**ransfer



简单来说，RESTful编程是，提倡web应用程序应该像最初设想的使用HTTP 。

也就是：

-  `GET` ：查询

- `PUT` ：修改，Record完整替换

- `PACTH`：修改，Record部分字段更新

- `POST` ：创建

- `DELETE` ：删除



以`GET`请求举例，`GET`应该是无副作用的，因此两次`GET`请求得到的结果应该是一样的：

```
http://myserver.com/catalog?item=1729
```



[stackoverflow-这个回答](https://stackoverflow.com/questions/671118/what-exactly-is-restful-programming/1081601#1081601) 解释了`PUT`和`PATCH`的区别：

> To modify the record (`lname` and `age` will remain unchanged):
>
> ```
> PATCH /user/123
> fname=Johnny
> ```
>
> To update the record (and consequently `lname` and `age` will be DEFAULT, maybe NULL):
>
> ```
> PUT /user/123
> fname=Johnny
> ```





REST is *an architecture style* for designing networked applications.

REST is not a "standard". There will never be a W3C recommendataion for REST, for example.



## 链接

- [Journey to HTTP/2 Sat, August 13,2016](https://kamranahmed.info/blog/2016/08/13/http-in-depth/)：这篇文章的时间线很清楚
- [What Is HTTP/3 – Lowdown on the Fast New UDP-Based Protocol](https://kinsta.com/blog/http3/)
- [HTTP/3: the past, the present, and the future](https://blog.cloudflare.com/http3-the-past-present-and-future/)：这篇文章是个商业推广，但是对于why http/1.1，why http/2，why http/3，即技术的出现是解决了什么问题有很清晰的说明
- [What exactly is RESTful programming?](https://stackoverflow.com/questions/671118/what-exactly-is-restful-programming)




