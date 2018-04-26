TCP stick package refers to the sender of packet data to the receiver when the stick into a package, from the receive buffer, a packet data header followed by a packet data before the tail.

Causes of sticking phenomenon are in many aspects, it can be caused by the sender, the receiver may also cause. The sender by sticking is caused by the TCP protocol itself, TCP to improve the efficiency of data transmission, the sender is often to collect enough data before sending a packet of data. If the successive transmitted data are rarely, usually TCP based optimization algorithm to these data into a packet once sent out, so that the recipient received stick package data. The receiver caused by stick pack is due to the user process does not receive data, can lead to sticking phenomenon. This is because the receiver first received data in the receiving buffer, User process data from the buffer zone, If a packet arrives before a packet of data has not yet been user process removed, The next packet data into the system when the receiver buffer received a packet data, User process according to the predetermined buffer size buffer to receive data from the system, This is a time to multiple packet data \(Figure 1\).

![](http://www.programering.com/images/remote/ZnJvbT1jc2RuJnVybD1ZV2FuNWlONUFqTTVVRE16TVRNdmdUT3ZNWFpuRldicDl5Y2tGMmJzQlhWdk1XYXNKV2RROVNidk5tTGxOWFlpdDJZMjV5ZDNkM0x2b0RjMFJIYQ.jpg "To solve the TCP network transmission with a light rain ridicule Mio Shi ")  
Figure 1

![](http://www.programering.com/images/remote/ZnJvbT1jc2RuJnVybD1ZV2FuNWlONUFqTTVVRE16TVRNdlVUTnZNWFpuRldicDl5Y2tGMmJzQlhWdk1XYXNKV2RROVNidk5tTGxOWFlpdDJZMjV5ZDNkM0x2b0RjMFJIYQ.jpg "To solve the TCP network transmission with a light rain ridicule Mio Shi ")  
Figure 2  
  
![](http://www.programering.com/images/remote/ZnJvbT1jc2RuJnVybD1ZV2FuNWlONUFqTTVVRE16TVRNdmdUTXZNWFpuRldicDl5Y2tGMmJzQlhWdk1XYXNKV2RROVNidk5tTGxOWFlpdDJZMjV5ZDNkM0x2b0RjMFJIYQ.jpg "To solve the TCP network transmission with a light rain ridicule Mio Shi ")  
Figure 3

Stick the package has two kinds, one kind is glued together the package is complete packet \(Figure 1, figure 2\), the other one is stuck with incomplete packets with packet \(Figure 3\), it is assumed that users receive buffer length is m bytes.

Not all stick package phenomena need treatment, if transmission data for continuous data without structure \(such as file transfer\), you don't have to separate the adhesion package \(referred to as sub\). But in practical engineering applications, the transmission of data is generally with the structure of the data, then the need for packet processing.

In the processing of fixed length data structure stick package problems, sub algorithm is relatively simple; in the treatment of variable length data structure stick package problems, sub algorithm is more complex. Especially as shown in Figure 3 stick pack, because a packet of data is divided in two consecutive packets received, processing more difficult. Try to avoid sticking phenomenon should be of practical engineering application.

In order to avoid sticking phenomenon, can take the following measures. One is for the sender by sticking phenomenon, Users can program settings to avoid, TCP provides mandatory data immediately transfer instructions push, TCP software receives the operating instructions, Immediately sends out the data, Without waiting for the send buffer full; two is for the receiver caused by sticking bag, You can optimize the program design, streamline the receiving process, improve work process of receiving priority measures, To receive data, In order to avoid sticking phenomenon; three is composed of the control, A packet of data according to the structure field, Human control of multiple receiving, Then merge, To avoid sticking packets by this means.

The three measures mentioned above, has its shortcomings. The first programmable methods can avoid the sender by stick package, but it closed optimization algorithm, reduces the network transmission efficiency, impact on the performance of the application, not recommended for general use. Only second ways to reduce the possibility of stick package, but can not be completely avoided when sending packets, high frequency, or because the network burst may make a time of data packets at the receiver is faster, the receiver may or may not receive, causing sticking bag. Third kinds of methods while avoiding the stick package, but the application efficiency is low, not suitable for real-time application.

A more comprehensive strategy is: the recipient creates a pre processing thread, the received data packets are pretreatment, separating adhesion. In this method we carried out experiments, proved to be efficient and feasible.





默认情况下, TCP 连接会启用延迟传送算法 \(Nagle 算法\), 在数据发送之前缓存他们. 如果短时间有多个数据发送, 会缓冲到一起作一次发送 \(缓冲大小见`socket.bufferSize`\), 这样可以减少 IO 消耗提高性能.

如果是传输文件的话, 那么根本不用处理粘包的问题, 来一个包拼一个包就好了. 但是如果是多条消息, 或者是别的用途的数据那么久需要处理粘包.

可以参见网上流传比较广的一个例子, 连续调用两次 send 分别发送两段数据 data1 和 data2, 在接收端有以下几种常见的情况:

* A. 先接收到 data1, 然后接收到 data2 .
* B. 先接收到 data1 的部分数据, 然后接收到 data1 余下的部分以及 data2 的全部.
* C. 先接收到了 data1 的全部数据和 data2 的部分数据, 然后接收到了 data2 的余下的数据.
* D. 一次性接收到了 data1 和 data2 的全部数据.

其中的 BCD 就是我们常见的粘包的情况. 而对于处理粘包的问题, 常见的解决方案有:

* 1. 多次发送之前间隔一个等待时间
* 1. 关闭 Nagle 算法
* 1. 进行封包/拆包

_**方案1**_

只需要等上一段时间再进行下一次 send 就好, 适用于交互频率特别低的场景. 缺点也很明显, 对于比较频繁的场景而言传输效率实在太低. 不过几乎不用做什么处理.

_**方案2**_

关闭 Nagle 算法, 在 Node.js 中你可以通过[`socket.setNoDelay()`](https://nodejs.org/dist/latest-v6.x/docs/api/net.html#net_socket_setnodelay_nodelay)方法来关闭 Nagle 算法, 让每一次 send 都不缓冲直接发送.

该方法比较适用于每次发送的数据都比较大 \(但不是文件那么大\), 并且频率不是特别高的场景. 如果是每次发送的数据量比较小, 并且频率特别高的, 关闭 Nagle 纯属自废武功.

另外, 该方法不适用于网络较差的情况, 因为 Nagle 算法是在服务端进行的包合并情况, 但是如果短时间内客户端的网络情况不好, 或者应用层由于某些原因不能及时将 TCP 的数据 recv, 就会造成多个包在客户端缓冲从而粘包的情况. \(如果是在稳定的机房内部通信那么这个概率是比较小可以选择忽略的\)

_**方案3**_

封包/拆包是目前业内常见的解决方案了. 即给每个数据包在发送之前, 于其前/后放一些有特征的数据, 然后收到数据的时候根据特征数据分割出来各个数据包.

