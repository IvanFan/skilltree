# Multiplexing

If the kernel find that the condition read operation for a particular process is matched, then the kernel will notify that process.

It's used for the following situations:

1. if the client need to handle multiple input interfaces \(e.g. screen input and network input\)
2. If the tcp server need to listen to ports and need to handle connecting ports
3. if the server need to handle tcp and udp 
4. if the server need to provide different services and different protocols

使用阻塞模式的套接字编写网络程序比较简单，容易实现。但是在服务器端，通常要处理大量的套接字通信请求，如果线程阻塞于上述的某一个输入或输出调用时，将无法处理其他任何运算或响应其他网络请求，这么做无疑是十分低效的，当然可以采用多线程，但大量的线程占用很大的内存空间，并且线程切换会带来很大的开销。而I/O多路复用模型能处理多个connection的优点就使其

**能支持更多的并发连接请求**

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





事件驱动不是无敌的，在事件驱动模型中，处理事件的进程一定是单线程的。

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

## 

## select,poll & epoll

![](/assets/osiomultiplexing1.png)

They are all sync I/O \(the R/W is blocked\)

select has 3 issues:

1. limitation of connection number
2. slow to find comparsion
3. 


