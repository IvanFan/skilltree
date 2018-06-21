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

**Why use multi processing –**

* The main advantage of multiprocessor system is to get more work done in a shorter period of time. These types of systems are used when very high speed is required to process a large volume of data. Multi processing systems can save money in comparison to single processor systems because the processors can share peripherals and power supplies.
* It also provides increased reliability in the sense that if one processor fails, the work does not halt, it only slows down. e.g. if we have 10 processors and 1 fails, then the work does not halt, rather the remaining 9 processors can share the work of the 10th processor. Thus the whole system runs only 10 percent slower, rather than failing altogether.

Multiprocessing refers to the hardware \(i.e., the CPU units\) rather than the software \(i.e., running processes\). If the underlying hardware provides more than one processor then that is multiprocessing.

**Multi tasking system’s working –**

* In a time sharing system, each process is assigned some specific quantum of time for which a process is meant to execute. Say there are 4 processes P1, P2, P3, P4 ready to execute. So each of them are assigned some time quantum for which they will execute e.g time quantum of 5 nanoseconds \(5 ns\). As one process begins execution \(say P2\), it executes for that quantum of time \(5 ns\). After 5 ns the CPU starts the execution of the other process \(say P3\) for the specified quantum of time.
* Thus the CPU makes the processes to share time slices between them and execute accordingly. As soon as time quantum of one process expires, another process begins its execution.
* Here also basically a context switch is occurring but it is occurring so fast that the user is able to interact with each program separately while it is running. This way, the user is given the illusion that multiple processes/ tasks are executing simultaneously. But actually only one process/ task is executing at a particular instant of time. In multitasking, time sharing is best manifested because each running process takes only a fair quantum of the CPU time.

**Multi threading system’s working –**

**Example 1 –**

* Say there is a web server which processes client requests. Now if it executes as a single threaded process, then it will not be able to process multiple requests at a time. Firstly one client will make its request and finish its execution and only then, the server will be able to process another client request. This is really costly, time consuming and tiring task. To avoid this, multi threading can be made use of.
* Now, whenever a new client request comes in, the web server simply creates a new thread for processing this request and resumes its execution to hear more client requests. So the web server has the task of listening to new client requests and creating threads for each individual request. Each newly created thread processes one client request, thus reducing the burden on web server.

**Example 2 –**

* We can think of threads as child processes that share the parent process resources but execute independently. Now take the case of a GUI. Say we are performing a calculation on the GUI \(which is taking very long time to finish\). Now we can not interact with the rest of the GUI until this command finishes its execution. To be able to interact with the rest of the GUI, this command of calculation should be assigned to a separate thread. So at this point of time, 2 threads will be executing i.e. one for calculation, and one for the rest of the GUI. Hence here in a single process, we used multiple threads for multiple functionality.

**Advantages of Multi threading –**

* Benefits of Multi threading include increased responsiveness. Since there are multiple threads in a program, so if one thread is taking too long to execute or if it gets blocked, the rest of the threads keep executing without any problem. Thus the whole program remains responsive to the user by means of remaining threads.
* Another advantage of multi threading is that it is less costly. Creating brand new processes and allocating resources is a time consuming task, but since threads share resources of the parent process, creating threads and switching between them is comparatively easy. Hence multi threading is the need of modern Operating Systems.

# What happens when we turn on computer?

A computer without a program running is just an inert hunk of electronics. The first thing a computer has to do when it is turned on is start up a special program called an operating system. The operating system’s job is to help other computer programs to work by handling the messy details of controlling the computer’s hardware.

**An overview of the boot process**

![](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/boot_process.png "sequence")

The boot process is something that happens every time you turn your computer on. You don’t really see it, because it happens so fast. You press the power button come back a few minutes later and Windows XP, or Windows Vista, or whatever Operating System you use is all loaded.

The BIOS chip tells it to look in a fixed place, usually on the lowest-numbered hard disk \(the boot disk\) for a special program called a boot loader \(under Linux the boot loader is called Grub or LILO\). The boot loader is pulled into memory and started. The boot loader’s job is to start the real operating system.

**Functions of BIOS**

  


  


**POST**

\(Power On Self Test\) The Power On Self Test happens each time you turn your computer on. It sounds complicated and thats because it kind of is. Your computer does so much when its turned on and this is just part of that.

  


  


It initializes the various hardware devices. It is an important process so as to ensure that all the devices operate smoothly without any conflicts. BIOSes following ACPI create tables describing the devices in the computer.

  


  


The POST first checks the bios and then tests the CMOS RAM. If there is no problems with this then POST continues to check the CPU, hardware devices such as the Video Card, the secondary storage devices such as the Hard Drive, Floppy Drives, Zip Drive or CD/DVD Drives.If some errors found then an error message is displayed on screen or a number of beeps are heard. These beeps are known as POST beep codes.

