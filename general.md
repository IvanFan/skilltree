# Multiplexing

If the kernel find that the condition read operation for a particular process is matched, then the kernel will notify that process.

It's used for the following situations:

1. if the client need to handle multiple input interfaces \(e.g. screen input and network input\)
2. If the tcp server need to listen to ports and need to handle connecting ports
3. if the server need to handle tcp and udp 
4. if the server need to provide different services and different protocols

使用阻塞模式的套接字编写网络程序比较简单，容易实现。但是在服务器端，通常要处理大量的套接字通信请求，如果线程阻塞于上述的某一个输入或输出调用时，将无法处理其他任何运算或响应其他网络请求，这么做无疑是十分低效的，当然可以采用多线程，但大量的线程占用很大的内存空间，并且线程切换会带来很大的开销。而I/O多路复用模型能处理多个connection的优点就使其

**能支持更多的并发连接请求**



关于I/O多路复用\(又被称为“事件驱动”\)，首先要理解的是，操作系统为你提供了一个功能，当你的某个socket可读或者可写的时候，它可以给你一个通知。这样当配合非阻塞的socket使用时，只有当系统通知我哪个描述符可读了，我才去执行read操作，可以保证每次read都能读到有效数据而不做纯返回-1和EAGAIN的无用功。写操作类似。操作系统的这个功能通过select/poll/epoll/kqueue之类的系统调用函数来使用，这些函数都可以同时监视多个描述符的读写就绪状况，这样，多个描述符的I/O操作都能在一个线程内并发交替地顺序完成，这就叫I/O多路复用，这里的“复用”指的是复用同一个线程。

## Compared with multi-process and multi-thread

Pros:

* less system resources
* don't need to build and maintain new process/thread

Cons:

* single thread
* blocking
* cannot fully use multi-core CPU

## Select

该函数准许进程指示内核等待多个事件中的任何一个发送，并只在有一个或多个事件发生或经历一段指定的时间后才唤醒。函数原型如下：

```
lientSocket s = accept(ServerSocket);  // 阻塞
  waitting_set.add(s); 
  while(1) {
     set_have_data  ds = select(waitting_set); // 多路复用器，
                                               // 多路复用是说多个socket数据通路
                                               // 一个select就可以侦听
                                               // 就好像他们是一个数据通路一样。
     foreach(ds as item) {
          item.read();  // reactor这里不会阻塞，采用异步io，详情查linux aio
          item.send('xxx');
     }
      item.close();
  }
```

![](/assets/osiomultiplexing2.png)

```
#include <sys/select.h>
#include <sys/time.h>

int select(int max_fd, fd_set *readset, fd_set *writeset, fd_set *exceptset, struct timeval *timeout)
FD_ZERO(int fd, fd_set* fds)   //清空集合
FD_SET(int fd, fd_set* fds)    //将给定的描述符加入集合
FD_ISSET(int fd, fd_set* fds)  //判断指定描述符是否在集合中
FD_CLR(int fd, fd_set* fds)    //将给定的描述符从文件中删除
```

![](/assets/osiomultiplexing3.png)

基于select的I/O复用模型的是单进程执行，占用资源少，可以为多个客户端服务。但是select需要轮询每一个描述符，在高并发时仍然会存在效率问题，同时select能支持的最大连接数通常受限。

## Poll

poll的机制与select类似，与select在本质上没有多大差别，管理多个描述符也是进行轮询，根据描述符的状态进行处理，但是poll没有最大文件描述符数量的限制。

Linux提供的poll函数接口如下：

```
#include <poll.h>
int poll(struct pollfd fds[], nfds_t nfds, int timeout);

typedef struct pollfd {
        int fd;                         // 需要被检测或选择的文件描述符
        short events;                   // 对文件描述符fd上感兴趣的事件
        short revents;                  // 文件描述符fd上当前实际发生的事件*/
} pollfd_t;
```

poll\(\)函数返回fds集合中就绪的读、写，或出错的描述符数量，返回0表示超时，返回-1表示出错；

1. fds是一个struct pollfd类型的数组，用于存放需要检测其状态的socket描述符，并且调用poll函数之后fds数组不会被清空；
2. nfds记录数组fds中描述符的总数量；
3. timeout是调用poll函数阻塞的超时时间，单位毫秒；
4. 一个pollfd结构体表示一个被监视的文件描述符，通过传递fds\[\]指示 poll\(\) 监视多个文件描述符。其中，结构体的events域是监视该文件描述符的事件掩码，由用户来设置这个域，结构体的revents域是文件描述符的操作结果事件掩码，内核在调用返回时设置这个域。events域中请求的任何事件都可能在revents域中返回。

## Epoll

epoll在Linux2.6内核正式提出，是基于事件驱动的I/O方式，相对于select和poll来说，epoll没有描述符个数限制，使用一个文件描述符管理多个描述符，将用户关心的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。优点如下：

1. 没有最大并发连接的限制，能打开的fd上限远大于1024（1G的内存能监听约10万个端口）

2. 采用回调的方式，效率提升。只有  
   **活跃可用**  
   的fd才会调用callback函数，也就是说 epoll 只管你“活跃”的连接，而跟连接总数无关，因此在实际的网络环境中，epoll的效率就会远远高于select和poll。

3. 内存拷贝。使用mmap\(\)文件映射内存来加速与内核空间的消息传递，减少复制开销。

epoll对文件描述符的操作有两种模式：LT\(level trigger，水平触发\)和ET\(edge trigger\)。

**水平触发：**默认工作模式，即当epoll\_wait检测到某描述符事件就绪并通知应用程序时，应用程序可以不立即处理该事件；下次调用epoll\_wait时，会再次通知此事件。

**边缘触发：**当epoll\_wait检测到某描述符事件就绪并通知应用程序时，应用程序必须立即处理该事件。如果不处理，下次调用epoll\_wait时，不会再次通知此事件。（直到你做了某些操作导致该描述符变成未就绪状态了，也就是说边缘触发只在状态由未就绪变为就绪时通知一次）。

ET模式很大程度上减少了epoll事件的触发次数，因此效率比LT模式下高。

## select,poll & epoll

![](/assets/osiomultiplexing1.png)

They are all sync I/O \(the R/W is blocked\)

select has 3 issues:

1. limitation of connection number
2. slow to find comparsion
3. 事件驱动不是无敌的，在事件驱动模型中，处理事件的进程一定是单线程的。

在现代工业中我们会面临两个问题：

1. 单线程模型不能有阻塞，一旦发生任何阻塞（包括计算机计算延迟）都会使得这个模型不如多线程。
   **另外，单线程模型不能很好的利用多核cpu**
   。
2. 既然不能有阻塞，那我们只有用多线程去做异步io，那马上就会面临回掉地狱。

为了解决我上述说的两个问题人们做出了一些改进：

1. 利用多进程，每个进程单条线程去利用多核CPU
   **。但是这又引入了新的问题：进程间状态共享和通信。**
   但是对于提升的性能来说，可以忽略不计。
2. **发明了协程。**



