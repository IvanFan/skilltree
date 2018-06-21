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

