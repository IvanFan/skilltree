**What is a process and process table? What are different states of process**  
A_process\_is an instance of program in execution.For example a Web Browser is a process, a shell \(or command prompt\) is a process.  
The operating system is responsible for managing all the processes that are running on a computer and allocated each process a certain amount of time to use the processor. In addition, the operating system also allocates various other resources that processes will need such as computer memory or disks. To keep track of the state of all the processes, the operating system maintains a table known as the\_process table_. Inside this table, every process is listed along with the resources the processes is using and the current state of the process.  
_Processes can be in one of three states: running, ready, or waiting_. The running state means that the process has all the resources it need for execution and it has been given permission by the operating system to use the processor. Only one process can be in the running state at any given time. The remaining processes are either in a waiting state \(i.e., waiting for some external event to occur such as user input or a disk access\) or a ready state \(i.e., waiting for permission to use the processor\). In a real operating system, the waiting and ready states are implemented as queues which hold the processes in these states. The animation below shows a simple representation of the life cycle of a process \(Source:[http://courses.cs.vt.edu/csonline/OS/Lessons/Processes/index.html](http://courses.cs.vt.edu/csonline/OS/Lessons/Processes/index.html)\)

**What is a Thread? What are the differences between process and thread?**  
A thread is a single sequence stream within in a process. Because threads have some of the properties of processes, they are sometimes calledlightweight processes. Threads are popular way to improve application through parallelism. For example, in a browser, multiple tabs can be different threads. MS word uses multiple threads, one thread to format the text, other thread to process inputs, etc.  
A thread has its own program counter \(PC\), a register set, and a stack space. Threads are not independent of one other like processes as a result threads shares with other threads their code section, data section and OS resources like open files and signals. See[http://www.personal.kent.edu/~rmuhamma/OpSystems/Myos/threads.htm](http://www.personal.kent.edu/~rmuhamma/OpSystems/Myos/threads.htm)for more details.

**Process is the minimum unit for system resource management**

**Thread is the minimum unit for CPU resource management**

**What is deadlock? **  
Deadlock is a situation when two or more processes wait for each other to finish and none of them ever finish.  Consider an example when two trains are coming toward each other on same track and there is only one track, none of the trains can move once they are in front of each other.  Similar situation occurs in operating systems when there are two or more processes hold some resources and wait for resources held by other\(s\).

**What are the necessary conditions for deadlock?**  
\_Mutual Exclusion:\_There is s resource that cannot be shared.  
\_Hold and Wait: \_A process is holding at least one resource and waiting for another resource which is with some other process.  
\_No Preemption:\_The operating system is not allowed to take a resource back from a process until process gives it back.  
\_Circular Wait:  \_A set of processes are waiting for each other in circular form.

**What is Virtual Memory? How is it implemented?**

Virtual memory creates an illusion that each user has one or more contiguous address spaces, each beginning at address zero. The sizes of such virtual address spaces is generally very high.

The idea of virtual memory is to use disk space to extend the RAM. Running processes don’t need to care whether the memory is from RAM or disk. The illusion of such a large amount of memory is created by subdividing the virtual memory into smaller pieces, which can be loaded into physical memory whenever they are needed by a process.

**What is Thrashing?**

Thrashing is a situation when the performance of a computer degrades or collapses. Thrashing occurs when a system spends more time processing page faults than executing transactions. While processing page faults is necessary to in order to appreciate the benefits of virtual memory, thrashing has a negative affect on the system. As the page fault rate increases, more transactions need processing from the paging device. The queue at the paging device increases, resulting in increased service time for a page fault \(Source: h[ttp://cs.gmu.edu/cne/modules/vm/blue/thrash.html](http://cs.gmu.edu/cne/modules/vm/blue/thrash.html)\)

**Multiprogramming – **A computer running more than one program at a time \(like running Excel and Firefox simultaneously\).

**Multiprocessing – **A computer using more than one CPU at a time.

**Multitasking –**

Tasks sharing a common resource \(like 1 CPU\).

**Multithreading**

is an extension of multitasking.

### **Multi programming –**

In a modern computing system, there are usually several concurrent application processes which want to execute. Now it is the responsibility of the Operating System to manage all the processes effectively and efficiently.  
One of the most important aspects of an Operating System is to multi program.  
In a computer system, there are multiple processes waiting to be executed, i.e. they are waiting when the CPU will be allocated to them and they begin their execution. These processes are also known as jobs. Now the main memory is too small to accommodate all of these processes or jobs into it. Thus, these processes are initially kept in an area called job pool. This job pool consists of all those processes awaiting allocation of main memory and CPU.  
CPU selects one job out of all these waiting jobs, brings it from the job pool to main memory and starts executing it. The processor executes one job until it is interrupted by some external factor or it goes for an I/O task.

The main idea of multi programming is to maximize the CPU time.  
**Multi programmed system’s working –**

* In a multi-programmed system, as soon as one job goes for an I/O task, the Operating System interrupts that job, chooses another job from the job pool \(waiting queue\), gives CPU to this new job and starts its execution. The previous job keeps doing its I/O operation while this new job does CPU bound tasks. Now say the second job also goes for an I/O task, the CPU chooses a third job and starts executing it. As soon as a job completes its I/O operation and comes back for CPU tasks, the CPU is allocated to it.
* In this way, no CPU time is wasted by the system waiting for the I/O task to be completed.

  Therefore, the ultimate goal of multi programming is to keep the CPU busy as long as there are processes ready to execute. This way, multiple programs can be executed on a single processor by executing a part of a program at one time, a part of another program after this, then a part of another program and so on, hence executing multiple programs. Hence, the CPU never remains idle.

### **Multiprocessing –**

In a uni-processor system, only one process executes at a time.  
Multiprocessing is the use of two or more CPUs \(processors\) within a single Computer system. The term also refers to the ability of a system to support more than one processor within a single computer system. Now since there are multiple processors available, multiple processes can be executed at a time. These multi processors share the computer bus, sometimes the clock, memory and peripheral devices also.

**Multi processing system’s working –**

* With the help of multiprocessing, many processes can be executed simultaneously. Say processes P1, P2, P3 and P4 are waiting for execution. Now in a single processor system, firstly one process will execute, then the other, then the other and so on.
* But with multiprocessing, each process can be assigned to a different processor for its execution. If its a dual-core processor \(2 processors\), two processes can be executed simultaneously and thus will be two times faster, similarly a quad core processor will be four times as fast as a single processor.



