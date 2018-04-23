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



