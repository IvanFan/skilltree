### HTTP/2与HTTP/1.1比较\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=3)\]

HTTP/2 相比 HTTP/1.1 的修改并不会破坏现有程序的工作，但是新的程序可以借由新特性得到更好的速度。[\[12\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Chapter_12._HTTP_2.0-12)

HTTP/2 保留了 HTTP/1.1 的大部分语义，例如[请求方法](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE#%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95)、[状态码](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE#%E7%8A%B6%E6%80%81%E7%A0%81)乃至[URI](https://zh.wikipedia.org/wiki/URI)和绝大多数[HTTP头部](https://zh.wikipedia.org/w/index.php?title=HTTP%E5%A4%B4%E9%83%A8&action=edit&redlink=1)字段一致。而 HTTP/2 采用了新的方法来编码、传输客户端——服务器间的数据。[\[12\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Chapter_12._HTTP_2.0-12)

### HTTP/1.1与SPDY的区别\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=4)\]

主条目：

[SPDY](https://zh.wikipedia.org/wiki/SPDY)

[SPDY](https://zh.wikipedia.org/wiki/SPDY)\(发音为"speedy"\) 是一个由[Google](https://zh.wikipedia.org/wiki/Google)主导的研究项目发明的HTTP替代协议。[\[13\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-extremetech-13)SPDY一开始主要关注降低延迟，采用了TCP通道，但是使用了不同的协议来达到此目的。其与HTTP/1.1相比，主要的改变有：[\[14\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Grigorik-14)

* 实现无需先入先出的
  [多路复用](https://zh.wikipedia.org/wiki/%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)
* 为简化客户端和服务器开发的消息—帧机制
* 强制性压缩（包括HTTP头部）
* 优先级排序
* 双向通讯

### HTTP/2与SPDY的比较\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=5)\]

HTTP/2的开发基于SPDY进行跃进式改进。在诸多修改中，最显著的改进在于，HTTP/2使用了一份经过定制的压缩算法，基于[霍夫曼编码](https://zh.wikipedia.org/wiki/%E9%9C%8D%E5%A4%AB%E6%9B%BC%E7%BC%96%E7%A0%81)，以此替代了SPDY的动态流压缩算法，以避免对协议的Oracle攻击——这一类攻击以[CRIME](https://zh.wikipedia.org/wiki/CRIME)为代表。此外，HTTP/2禁用了诸多加密包，以保证基于TLS的连接的前向安全。



## 新特性\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=6)\]

在 HTTP/2 的第一版草案（对 SPDY 协议的复刻）中，新增的性能改进不仅包括HTTP/1.1中已有的[多路复用](https://zh.wikipedia.org/wiki/%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)，修复[队头阻塞](https://zh.wikipedia.org/wiki/%E9%98%9F%E5%A4%B4%E9%98%BB%E5%A1%9E)问题，允许设置设定请求优先级，还包含了一个头部压缩算法\(HPACK\)[\[15\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-HPACK-15)[\[16\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-16)。此外， HTTP/2 采用了二进制而非明文来打包、传输客户端—服务器间的数据。[\[12\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Chapter_12._HTTP_2.0-12)



### 帧、消息、流和TCP连接\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=7)\]

有别于HTTP/1.1在连接中的明文请求，HTTP/2与SPDY一样，将一个TCP连接分为若干个流（Stream），每个流中可以传输若干消息（Message），每个消息由若干最小的二进制帧（Frame）组成。[\[12\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Chapter_12._HTTP_2.0-12)这也是HTTP/1.1与HTTP/2最大的区别所在。 HTTP/2中，每个用户的操作行为被分配了一个**流编号**\(stream ID\)，这意味着用户与服务端之间创建了一个TCP通道；协议将每个请求分区为二进制的控制帧与数据帧部分，以便解析。这个举措在SPDY中的实践表明，相比HTTP/1.1，新页面加载可以加快11.81% 到 47.7%[\[17\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-17)

### HPACK 算法\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=8)\]

HPACK算法是新引入HTTP/2的一个算法，用于对HTTP头部做压缩。其原理在于：

* 客户端与服务端根据
  [RFC 7541](https://tools.ietf.org/html/rfc7541)
  的附录A，维护一份共同的静态字典（Static Table），其中包含了常见头部名及常见头部名称与值的组合的代码；
* 客户端和服务端根据先入先出的原则，维护一份可动态添加内容的共同动态字典（Dynamic Table）；
* 客户端和服务端根据
  [RFC 7541](https://tools.ietf.org/html/rfc7541)
  的附录B，支持基于该静态哈夫曼码表的哈夫曼编码（Huffman Coding）。

### 服务器推送\[[编辑](https://zh.wikipedia.org/w/index.php?title=HTTP/2&action=edit&section=9)\]

网站为了使请求数减少，通常采用对页面上的图片、脚本进行[极简化](https://zh.wikipedia.org/wiki/%E6%A5%B5%E7%B0%A1%E5%8C%96)处理。但是，这一举措十分不方便，也不高效，依然需要诸多HTTP链接来加载页面和页面资源。

HTTP/2引入了**服务器推送**，即服务端向客户端发送比客户端请求更多的数据。这允许服务器直接提供浏览器渲染页面所需资源，而无须浏览器在收到、解析页面后再提起一轮请求，节约了加载时间。[\[18\]](https://zh.wikipedia.org/wiki/HTTP/2#cite_note-Pratt-18)

