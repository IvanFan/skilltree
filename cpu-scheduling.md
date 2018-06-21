A process has following Attributes.

```
1. Process Id:    A unique identifier assigned by operating system
2. Process State: Can be ready, running, .. etc
3. CPU registers: Like Program Counter (CPU registers must be saved and 
                  restored when a process is swapped out and in of CPU)
5. Accounts information:
6. I/O status information: For example devices allocated to process, 
                           open files, etc
8. CPU scheduling information: For example Priority (Different processes 
                               may have different priorities, for example
                               a short process may be assigned a low priority
                               in the shortest job first scheduling)
```

All the above attributes of a process are also known as_**Context of the process**_.  
Every Process has its known[Program control Block](http://en.wikipedia.org/wiki/Process_control_block)\(PCB\) i.e each process will have a unique PCB. All the Above Attributes are the part of the PCB.

**Context Switching**  
The process of saving the context of one process and loading the context of other process is known as Context Switching. In simple term, it is like loading and unloading of the process from running state to ready state.

**When does Context switching happen?**  
1. When a high priority process comes to ready state, compared to priority of running process  
2. Interrupt Occurs  
3. User and Kernel mode switch: \(It is not necessary though\)  
4. Preemptive CPU scheduling used.

**Context Switch vs Mode Switch**  
A mode switch occurs when CPU privilege level is changed, for example when a system call is made or a fault occurs. The kernel works in more privileged mode than a standard user task. If a user process wants to access things which are only accessible to the kernel, a mode switch must occur. The currently executing process need not be changed during a mode switch.  
A mode switch typically occurs for a process context switch to occur. Only the Kernel can cause a context switch.

**CPU Bound vs I/O Bound Processes:**  
A CPU Bound Process requires more amount of CPU time or spends more time in the running state.  
I/O Bound Process requires more amount of I/O time and less CPU time. I/O Bound process more time in the waiting state.

**Exercise:**  
**1.**Which of the following need not necessarily be saved on a context switch between processes? \(GATE-CS-2000\)  
\(A\) General purpose registers  
\(B\) Translation lookaside buffer  
\(C\) Program counter  
\(D\) All of the above

**Answer \(B\)**

**Explanation:**  
In a process context switch, the state of the first process must be saved somehow, so that, when the scheduler gets back to the execution of the first process, it can restore this state and continue.The state of the process includes all the registers that the process may be using, especially the program counter, plus any other operating system specific data that may be necessary.A Translation look-aside buffer \(TLB\) is a CPU cache that memory management hardware uses to improve virtual address translation speed. A TLB has a fixed number of slots that contain page table entries, which map virtual addresses to physical addresses. On a context switch, some TLB entries can become invalid, since the virtual-to-physical mapping is different. The simplest strategy to deal with this is to completely flush the TLB.

**2.**The time taken to switch between user and kernel modes of execution be t1 while the time taken to switch between two processes be t2. Which of the following is TRUE? \(GATE-CS-2011\)  
\(A\) t1 &gt; t2  
\(B\) t1 = t2  
\(C\) t1 &lt; t2  
\(D\) nothing can be said about the relation between t1 and t2.

**Answer: \(C\)**  
**Explanation:**Process switching involves mode switch. Context switching can occur only in kernel mode.





# States of a process

States of a process are as following:

![](https://www.geeksforgeeks.org/wp-content/uploads/gq/2015/06/process-states1.png)

* **New \(Create\) –**
  In this step process is about to create but not yet created, it is the program which is present in secondary memory that will be picked up by OS to create the process.
* **Ready –**
  New -
  &gt;
   Ready to run. After creation of process, the process enters the ready state i.e. into the main memory. The process here is ready to run and is waiting to get the CPU time for its execution.
* **Run –**
  The process is running in the main memory.
* **Blocked or wait –**
  Whenever the process requests the I/O it enters the blocked or wait state. It is executed in the main memory and it doesn’t require CPU. Once the I/O operation is completed the process goes to ready state.
* **Terminated or completed –**
  Process is killed as well as PCB is deleted.
* **Suspend ready –**
  Set of process that were initially in ready state but lack of main memory caused them to go to suspend ready, it is a secondary memory.
* **Suspend wait or suspend blocked –**
  Similar to suspend ready but uses the process which was performing I/O operation and lack of main memory caused them to move to secondary memory.
 
  When work is finished it may go to suspend ready.

**CPU and IO Bound Processes:**  
If process is having lots of CPU operation then it is called CPU bound process. Similarly, If process is having lots of IO operation then it is called IO bound process.

**Types of schedulers:**

1. **Long term – performance –**
   Makes decision about how many processes should be made to stay in the ready state this decides the degree of multiprogramming. Once decision is taken it lasts for long time hence called long term scheduler.
2. **Short term – Context switching time –**
   Short term scheduler will decide which process to be executed next and then it will call dispatcher. Dispatcher is a software that moves process from ready to run and vice versa. In other words, it is context switching.
3. **Medium term – Swapping time –**
   Suspension decision is taken by medium term scheduler. Medium term scheduler is used for swapping that is moving the process from main memory to secondary and vice versa.

**Multiprogramming –**We have many processes ready to run. There are two types of multiprogramming:

1. **Pre-emption –**
   Process is forcefully removed from CPU. Pre-emption is also called as time sharing or multitasking.
2. **Non pre-emption –**
   Processes are not removed until they complete the execution.



