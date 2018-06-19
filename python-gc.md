## GC

As I known, JVM has a complex GC process.

When it comes to Python, we will use similar concepts of GC

### Reference counting

PyObject is a common content for each object. ob\_refcnt is the reference count. If the object has a new reference, the ob\_refcnt will increase. vice versa. If ob\_refcnt is equal to 0, the life of object should end

* Pros:  simple real-time

* Cons: counting references costs resources duplicated reference

### Mark-swap

![](/assets/import3.png)

solve the duplicated reference

『标记清除（Mark—Sweep）』算法是一种基于追踪回收（tracing GC）技术实现的垃圾回收算法。它分为两个阶段：第一阶段是标记阶段，GC会把所有的『活动对象』打上标记，第二阶段是把那些没有标记的对象『非活动对象』进行回收。那么GC又是如何判断哪些是活动对象哪些是非活动对象的呢？

对象之间通过引用（指针）连在一起，构成一个有向图，对象构成这个有向图的节点，而引用关系构成这个有向图的边。从根对象（root object）出发，沿着有向边遍历对象，可达的（reachable）对象标记为活动对象，不可达的对象就是要被清除的非活动对象。根对象就是全局变量、调用栈、寄存器。

在上图中，我们把小黑圈视为全局变量，也就是把它作为root object，从小黑圈出发，对象1可直达，那么它将被标记，对象2、3可间接到达也会被标记，而4和5不可达，那么1、2、3就是活动对象，4和5是非活动对象会被GC回收。

标记清除算法作为Python的辅助垃圾收集技术主要处理的是一些容器对象，比如list、dict、tuple，instance等，因为对于字符串、数值对象是不可能造成循环引用问题。Python使用一个双向链表将这些容器对象组织起来。不过，这种简单粗暴的标记清除算法也有明显的缺点：清除非活动的对象前它必须顺序扫描整个堆内存，哪怕只剩下小部分活动对象也要扫描所有对象。

### Generational GC

分代回收是一种以空间换时间的操作方式，Python将内存根据对象的存活时间划分为不同的集合，每个集合称为一个代，Python将内存分为了3“代”，分别为年轻代（第0代）、中年代（第1代）、老年代（第2代），他们对应的是3个链表，它们的垃圾收集频率与对象的存活时间的增大而减小。新创建的对象都会分配在年轻代，年轻代链表的总数达到上限时，Python垃圾收集机制就会被触发，把那些可以被回收的对象回收掉，而那些不会回收的对象就会被移到中年代去，依此类推，老年代中的对象是存活时间最久的对象，甚至是存活于整个系统的生命周期内。同时，分代回收是建立在标记清除技术基础之上。分代回收同样作为Python的辅助垃圾收集技术处理那些容器对象

## 什么情况存在内存泄露 {#什么情况存在内存泄露}

* python引用计数 + 分代收集和标记清除（处理循环引用），进行垃圾回收，但如下两种情况依旧存在内存泄露：
* 第一是对象被另一个生命周期特别长（如全局变量）的对象所引用
* 第二是循环引用中的对象定义了
  `__del__`
  函数，简而言之，循环引用中Python无法判断析构对象的顺序，无法释放

## 相关术语 {#相关术语}

* reachable/collectable（unreachable/uncollectable）
* reachable是针对python对象而言，如果从根集（root）能到找到对象，那么这个对象就是reachable，与之相反就是unreachable，unreachable只存在于循环引用中的对象，Python的gc模块就是针对unreachable对象
* collectable是针对unreachable对象而言，如果这种对象能被回收，是collectable，如果不能被回收，即循环引用中的对象定义了
  `__del__`
  ， 那么就是uncollectable。 即unreachable \(循环引用\)分成 collectable和ubcollectable（
  `__del__`
  ）

## 特殊说明\(PEP442\) {#特殊说明pep442}

python3.4开始已经可以自动处理带有`__del__`方法的循环引用，也不会发生内存泄露了

