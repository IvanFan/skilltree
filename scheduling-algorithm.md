* 先来先服务\(FCFS, First Come First Serve\)

* 短作业优先\(SJF, Shortest Job First\)

* 最高优先权调度\(Priority Scheduling\)

* 时间片轮转\(RR, Round Robin\)
* 多级反馈队列调度\(multilevel feedback queue scheduling\)

# 

# FCFS

先来先服务（FCFS）调度算法是一种最简单的调度算法，该算法既可用于作业调度， 也可用于进程调度。FCFS算法比较有利于长作业（进程），而不利于短作业（进程）。由此可知，本算法适合于CPU繁忙型作业， 而不利于I/O繁忙型的作业（进程）。

# SJ/PF

短作业（进程）优先调度算法（SJ/PF）是指对短作业或短进程优先调度的算法，该算法既可用于作业调度， 也可用于进程调度。但其对长作业不利；不能保证紧迫性作业（进程）被及时处理；作业的长短只是被估算出来的。

# FPF

1. 优先权调度算法的类型。

为了照顾紧迫性作业，使之进入系统后便获得优先处理，引入了最高优先权优先（FPF）调度算法。 此算法常被用在批处理系统中，作为作业调度算法，也作为多种操作系统中的进程调度，还可以用于实时系统中。当其用于作业调度， 将后备队列中若干个优先权最高的作业装入内存。当其用于进程调度时，把处理机分配给就绪队列中优先权最高的进程，此时， 又可以进一步把该算法分成以下两种：

1\)非抢占式优先权算法

2\)抢占式优先权调度算法（高性能计算机操作系统）

1. 优先权类型 。

对于最高优先权优先调度算法，其核心在于：它是使用静态优先权还是动态优先权， 以及如何确定进程的优先权。

3.动态优先权

高响应比优先调度算法为了弥补短作业优先算法的不足，我们引入动态优先权，使作业的优先等级随着等待时间的增加而以速率a提高。 该优先权变化规律可描述为：优先权=（等待时间+要求服务时间）/要求服务时间；即 =（响应时间）/要求服务时间

# RP

时间片轮转法一般用于进程调度，每次调度，把CPU分配队首进程，并令其执行一个时间片。 当执行的时间片用完时，由一个记时器发出一个时钟中断请求，该进程被停止，并被送往就绪队列末尾；依次循环。

# multilevel feedback queue scheduling

多级反馈队列调度算法多级反馈队列调度算法，不必事先知道各种进程所需要执行的时间，它是目前被公认的一种较好的进程调度算法。 其实施过程如下：

1\) 设置多个就绪队列，并为各个队列赋予不同的优先级。在优先权越高的队列中， 为每个进程所规定的执行时间片就越小。

2\) 当一个新进程进入内存后，首先放入第一队列的末尾，按FCFS原则排队等候调度。 如果他能在一个时间片中完成，便可撤离；如果未完成，就转入第二队列的末尾，在同样等待调度…… 如此下去，当一个长作业（进程）从第一队列依次将到第n队列（最后队列）后，便按第n队列时间片轮转运行。

3\) 仅当第一队列空闲时，调度程序才调度第二队列中的进程运行；

仅当第1到第（ i-1 ）队列空时， 才会调度第i队列中的进程运行，并执行相应的时间片轮转。

4\) 如果处理机正在处理第i队列中某进程，又有新进程进入优先权较高的队列， 则此新队列抢占正在运行的处理机，并把正在运行的进程放在第i队列的队尾。



## Dynamic scheduling {#最早截止时间优先算法}

## 最早截止时间优先算法 \(EDF\) {#最早截止时间优先算法}

算法是根据任务的开始截止时间和完成截止时间来确定任务的优先级。截止时间越早，其优先级越高。就绪队列中任务按其截止时间排列，队首任务先分配处理机。



### LLF

最短空闲时间优先算法（LLF）也是一种动态调度算法。LLF指在调度时刻，任务的优先级根据任务的空闲时间动态分配。空闲时间越短，优先级越高。空闲时间=deadline-任务剩余执行时间。LLF可调度条件和EDF相同。

  



