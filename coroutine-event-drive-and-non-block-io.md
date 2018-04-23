# Coroutine

Pros:

* 无需线程上下文切换的开销

* 无需原子操作锁即同步锁的开销  
  所谓原子操作是指不会被线程调度机制打打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何切换到另一个线程的动作。原子操作可以是一个步骤，也可以是多个操作步骤，但是其顺序是不可以被打乱的，或者割掉只执行的部分。视作整体是原子性的核心。

* 方便切换控制流，简化编程模型。

* 高并发+高扩展性+低成本：一个CPU可以支持上万个CPU，适合高并发处理

Cons:

* 无法利用多核资源：协程的本质是单个线程，他不能同时将单个CPU的多个和用上，协程需要和进程配合才能运行在多CPU上，当然我们日常所编写的绝大部分应用都没有这个必要，除非是CPU密集型应用。
* 进行阻塞操作，如IO操作时会阻塞掉整个程序。

e.g. python yield generator

Definition:

1. single thread
2. modify shared data without lock
3. user control the workflow
4. if I/O has been blocked, switch to other coroutine

# Event Drive & Async I/O

# 

通常，我们写服务器处理模型的时候，有以下几种模型：

1. 每收到一个请求，创建一个新的进程，来处理该请求
2. 每收到一个请求，创建一个新的线程，来处理该请求
3. 每收到一个请求，放入一个时间列表，让主进程通过非阻塞I/O方式来处理请求

以上几种方式各有优缺点：

* 第一种方法中，由于创建新的进程开销比较大，会导致服务器性能比较差，但是实现比较简单
* 第二种方式，由于要设计成线程的同步，有可能会死锁等问题。
* 第三种方式中，在写应用程序代码时，逻辑比前两种都复杂。

Event Drive Model:

1. has a message queue
2. add event to the queue
3. the loop will keep popping event to handle
4. event has its own function to handle the event

![](/assets/oscoroutineenevtdrive1.png)

event drive is a pattern

let's compare single thread, multi thread and event drive

![](/assets/os coroutine event drive2.png)

### 用户空间和内核空间 {#用户空间和内核空间}

现在操作系统都是采用虚拟存储器，那么对32位操作系统而言，它的寻址空间（虚拟存储空间）位4G（2的32次方）。操作系统的核心是内核，独立于普通的应用程序，可以访问受保护的内存空间，也有访问底层硬件设备的所有权限。为了保护用户进程不能直接操作内核（kernel），保证内核的安全，操作系统将虚拟空间划分为两个部分，一部分为内核空间，一部分作为用户空间。针对linux操作系统而言，将最高的1G字节供内核使用，成为内核空间，而将较低的3G字节供个进程使用，称为用户空间。

### 进程切换 {#进程切换}

为了控制进程的执行，内核必须有能力挂起正在CPU上运行的进程，并回复以前挂起的某个进程的执行。这种行为称为进程切换。因此可以说，任何进程都是在操作系统内核的支持下运行的，是与内核紧密相关的。

* 从一个进程的运行到另一个进程的运行，这个过程经过下面这些变化：

  1. 保存处理机上下文，包括程序计数器和其他寄存器。
  2. 更新PCB信息。
  3. 把进程的PCB移入相应的队列，如就绪、在某事件阻塞等队列。
  4. 选择另一个进程执行，并更新其PCB。
  5. 更新内存管理的数据结构。
  6. 回复处理机上下文

总之就是非常消耗资源  
_注：进程控制块，是操作系统核心中的一种数据结构，主要表示进程状态。其作用是使一个在多道程序环境下不能独立运行的程序（含数据），成为一个能独立运行的基本单位或与其他进程并发执行的进程。或者说，操作系统是根据PCB来对并发执行的进程进行控制可管理的。PCB通常是系统内存占用区中的一个连续存区，它存放着操作系统用户描述进程情况及控制进程运行所需的全部信息_

### 进程的阻塞 {#进程的阻塞}

正在执行的进程，由于期待的某些事情未发生，如请求系统资源失败、等待某种操作的完成、新数据尚未到达或无新工作等，则有系统自动执行阻塞原语（Block），是自己有运行状态变为阻塞状态。可见进程的阻塞是进程自身的一种阻塞行为，也因此只有处于运行状态的进程（获得CPU），才能将其转为阻塞状态。当进程进入阻塞状态，是不占CPU资源的。

### 文件描述符fd {#文件描述符fd}

文件描述符（File descriptor）是计算机科学中的一个术语。是一个用于表述指向文件的引用的抽象化概念。  
文件描述符在形式上是一个非负整数。实际上，他是一个索引值，指向内核为每一个进程所维护的该进程打开文件 的记录表。当程序大爱一个现有文件或者穿件一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往之适用于UNIX、Linux这样的操作系统。

### 缓存I/O {#缓存io}

缓存I/O又被称作标准I/O，大多数文件系统的默认I/O操作都是缓存I/O。在Linux的缓存I/O机制中，操作系统会将I/O的数据缓存在文件系统的页缓存中，也就是说，数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区中拷贝到应用程序的地址空间。

缓存I/O的缺点：

* 数据在传输过程中需要在应用程序地址空间和内核进行多次数据拷贝操作，这些数据拷贝操作所带来的CPU以及内存的开销是很大的。

## I/O模式 {#io模式}

对于一次I/O访问（以read举例），数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。所以说，当一个read操作发生时，他会经历两个阶段：  
1. 等待数据准备  
2. 将数据从内核拷贝到进程中

正式因为这两个阶段，linux系统产生了下面五种网路模式的方案：

* 阻塞I/O（blocking IO）  
* 非阻塞I/O（nonblocking IO）  
* I/O多路复用（IO multiplexing）  
* 信号驱动I/O（signal driven IO）  
* 异步I/O（asynchronous IO）

注：信号驱动IO在实际中并不常用，所以下面只提及剩下的四中IO模型

### 阻塞I/O（blocking IO） {#阻塞ioblocking-io}

在linux中，没默认情况下所有的socket都是blocking，一个典型的读操作流程是这样的：  
![](https://img-blog.csdn.net/20170722133718812?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2lsbDAzODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

当用户进程调用了recvfrom这个系统调用，kernel就开始了IO的第一个阶段：准备数据（对于网络IO来说，很多时候数据在一开始还没有到达。比如，还没有收到一个完整的UDP包。这个时候kernel就要等待足够的数据到来）。这个过程需要等待。也就是说数据被拷贝到操作系统内核的缓冲区是需要一个过程的。而用户在这边，整个进程会被阻塞（当然，是进程自己选择的阻塞）。当kernel一直等到数据准备好了，它就会将数据从kernel中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。  
_所以，blocking IO的特点就是在IO执行的量的阶段都被block了_

### 非阻塞IO（nonblocking IO） {#非阻塞iononblocking-io}

linux下，可以通过设置socket使其变为non-blocking。当对一个non-blocking socket执行度操作是，流程是这个样子的：  
![](https://img-blog.csdn.net/20170722135300381?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2lsbDAzODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么他并不会block用户进程，而是立刻返回一个error。从用户的角度讲，它发起一个read操作后，并不需要等待，而是马上得到一个结果。用户进程判断结果是个error时，他就知道数据还么有准备好，于是他可以再次发送read操作。一旦kernel中的数据转备好了，并且有再次收到了用户进程的system call，那么他马上就将数据拷贝到了用户内存，然后返回。  
_所以， nonblocking IO的特点是用户进程需要不断的主动询问kernel数据好了没有。_

### I/O多路复用（IO multiplexing） {#io多路复用io-multiplexing}

I/O 多路复用就是本文讲到的select、pool、epool，有些地方也称这种I/O方式为event driven IO（事件驱动IO）。select、epool的好吃就在于单个进程就可以同时处理多个网络连接的IO。他的基本原理就是select、pool、epool这个function会不断地轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。  
![](https://img-blog.csdn.net/20170722140524234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2lsbDAzODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

当用户进程调用了select，那么整个进程都会被block，而同时，kernel会“监听”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。  
_所以，I/O多路复用的特点是通过一种机制，一个进程能同时等待多个文件描述符，而这些文件描述符（套接字描述符）其中的任意一个进入读就绪状态，select\(\)函数就可以返回。_

这个图跟阻塞IO的图其实没有太大的不同，事实上，还等差一些。因为这里需要使用两个system call（select和recvfrom），而阻塞IO（blocking IO）只调用了一个system call（recvfrom）。但是，用select的有事在于他可以同时处理多个connection。

所以，如果处理连接数不是很高 的话，使用select、epool的web server不一定比使用multithreading + nlocking IO的web server性能更好，可能延迟还更大。select、epool的优势并不是对于单个链接能处理的更快，而是在于能处理更多的链接。

在 IO multiplexing Model中，对于每一个socket，一般都设置成为non-blocking，但是，如上图所示，整个用户的process其实是一直被block的，只不过process是被select这个函数block，而不是被socket IO给block。



### 异步I/O（asynchronous IO） {#异步ioasynchronous-io}

Linux下的asynchronous IO其实用的很少，流程如下:  
![](https://img-blog.csdn.net/20170722142116879?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2lsbDAzODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

用户进程发起read操作之后，立刻就可以做其他的事情。而另一方面，从kernel的角度，当他收到一个asynchronous read之后，首先他会立刻返回。所以不会对用户进程产生任何的block。然后，kernel会等待数据准备完成，然后将数据拷贝到用户内存，当这一切完成之后，kernel会给用户进程发送一个signal，告诉它read操作完成了



## 总结 {#总结}

**blocking和non-blocking的区别**  
调用blocking IO会一直block住对应的进程直到操作完成，而non-blockig IO在kernel还准备数据的情况下立刻返回

**同步IO（synchronous IO）和异步IO（asynchronous）的区别**  
在说明同步IO和异步IO的区别之前，需要先给出两个定义。POSIX的定义是这个样子的：

* 同步的输入/输出操作导致请求进程被阻塞，直到该输入/输出操作完成;
* 异步的输入/输出操作不会导致请求进程被阻塞;

二者的区别就在于同步IO做IO操作的时候会将process阻塞，安装这个定义，之前所述的阻塞IO（blocking IO），非阻塞IO（non-blocking IO），IO多路复用（IO multiiplexing）都属于同步IO。  
_注：虽然非阻塞IO没有被阻塞，但是也是同步IO，因为定义中提高的IO操作是指真实的IO操作，就是例子中的recvfrom这个system call。非阻塞IO在执行recvform这个system call的时候，如果kernel的数据没有准备好，这时候不会block进程。但是当kernel准备好数据的时候，recvform会将数据从kernel拷贝到 用户内存中，这时候进程是被block了，在这段时间内进程是被block的。_

而异步IO不一样，当进程发起IO操作后，就直接返回，再也不理睬了，直到kernel发送一个信号，告诉进程说IO完成。在这整体过程中，进程完全没有被block。

各个IO模型的比较示意图：  
![](https://img-blog.csdn.net/20170722155532073?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2lsbDAzODM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

通过上面的图片，可以发现非阻塞IO和异步IO的区别还是很明显的。在非阻塞IO中，虽然劲曾大部分时间不会被block，但是他仍要求进程去主动的check，并且当数据准备完成后，越要进程主动的再次调用recvfrom来将数据拷贝到用户内存。而异步IO则完全同。他就像是用户进程将整个IO操作交给了其他人（kernel）完成，然后别人做完之后发信号通知。在此期间，用户进程不需要检查IO的操作状态，也不需要主动的拷贝数据。

