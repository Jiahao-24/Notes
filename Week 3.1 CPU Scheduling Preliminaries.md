### Recall: Road Map
![[Pasted image 20230923165125.png]]
### Queue Diagram
![[Pasted image 20230923170820.png]]
### What is CPU scheduling?
- Selection of one of ready processes from ready queue for execution on CPU
	![[Pasted image 20230923171144.png]]
- `Scheduling Policy:`
	- Criterion/criteria based on which selection is made
	- Defined based on clear measures (e.g., queuing time, priority, urgency, etc.)
### CPU–I/O Burst Cycle
Burst: segment of a process executing between two specific events or conditions. In the context of CPU bursts, it refers to the time a process spends actively using the CPU.
- I/O events
- Creation and termination of children processes
- Interrupt or system call
- Process termination

> The success of CPU scheduling depends on an observed property of processes: process execution consists of a cycle of CPU execution and I/O wait (alternate between these two states). **Processes execute in cycles of CPU bursts and I/O bursts.**
> 
> Process execution begins with a CPU burst. That is followed by an I/O burst, which is followed by another CPU burst, then another I/O burst, and so on. Eventually, the final CPU burst ends with a system request to terminate execution.
![[Pasted image 20230923182341.png]]
- **I/O-Bound Process**: has many short CPU bursts
	> An I/O-bound process is one that spends a substantial amount of its time waiting for I/O operations to complete, rather than actively using the CPU.
- **CPU-Bound Process**: has few long CPU bursts
	> A CPU-bound process is one that primarily requires the CPU's computational power to perform its tasks, it tend to use little I/O and spend most of their time executing instructions on the CPU.
### Preemptive() vs Non-preemptive Scheduling
"Preemptive" refers to a mode of operation in which a higher-priority task or process can interrupt or preempt a lower-priority task that is currently running.
#### Non-preemptive Scheduling:
- Process burst keeps executing on CPU until it releases it upon its completion(termination) irrespective of other processes or events. (See 5.1.3 for details)
#### Preemptive Scheduling:
- Processes can be forcefully stopped:
	- To schedule other processes
	- To execute an interrupt handler
- Used in most modern OSs
- can result in race conditions when data are shared among several processes.
	> Consider the case of two processes that share data. While one process is updating the data, it is preempted so that the second process can run. The second process then tries to read the data, which are in an inconsistent state.
### Scheduling Quality Metrics(Criteria)
Different CPU-scheduling algorithms have different properties, and the choice of a particular algorithm may favour one class of processes over another.
- **CPU utilization:** Keeping CPU as busy as possible (use `top` in UNIX to obtain)
- **Throughput:** Maximizing # of processes that complete their execution per time unit
- **Turnaround time:** Minimizing amount of time from process creation to termination
- **Waiting time:  Minimizing process queuing time
- **Response time:** Minimizing amount of time between a submitted request and first response