![[Pasted image 20230923184916.png]]
##### Definition:
- **Waiting time:** total time a process spends waiting in the ready queue before it gets CPU time for execution
- **Turnaround time:** the total time taken for a process to complete its execution, including both the time it spends waiting in the ready queue and the time it spends executing on the CPU
- **Response time:** the total time it takes for a process to receive the first CPU burst after it has been submitted to the system.
- `Waiting Time = Turnaround Time - Burst Time`
- `Turnaround Time = Completion Time - Arrival Time`
- `Response Time = Time of First CPU Burst - Arrival Time`
### First come First Serve (FCFS)
- Schedule process that requested execution first
	- oldest process in the ready queue is allocated the CPU first
	- manage ready queue using a first-in, first-out(FIFO) policy.
		- when CPU idle, scheduler selects the head of the queue
		- once scheduled, process is removed from the queue , next one become the head of the queue
		- new process added to queue tail
	
#### FCFS Example
![[Pasted image 20231001174342.png]]
> There is a **Convoy effect** as all the other processes wait for the one big process to get off the CPU, which results in lower CPU and device utilization than might be possible if the shorter processes were allowed to go first.
	
Suppose that the processes arrive in the order ***P2, P3, P1***:
The Gantt chart for the schedule is:
	![[Pasted image 20231001174658.png]]
- Waiting time for ***P1 = 6ms; P2 = 0; P3 = 3***
- Average = ***(6+0+3)/3 = 3ms***
- much better than the previous case
- no convoy effect
##### CASE: one CPU-bound and many I/O-bound processes
The **CPU**-bound process will get and hold the **CPU**. During this time, all the other processes will finish their **I/O** and will move into the ready queue, waiting for the **CPU**. While the processes wait in the ready queue, the **I/O** devices are idle. Eventually, the **CPU**-bound process finishes its **CPU** burst and moves to an **I/O** device. All the **I/O**-bound processes, which have short **CPU** bursts, execute quickly and move back to the **I/O** queues. At this point, the **CPU** sits idle. The **CPU**-bound process will then move back to the ready queue and be allocated the **CPU**. Again, all the **I/O** processes end up waiting in the ready queue until the **CPU**-bound process is done.

#### FCFS Pros and Cons
- Pros:
	- Simple and easy to implement (with a FIFO queue)
	- Fair - every process will eventually run as long as CPU is not blocked by a process
- Cons:
	- Waiting time depends on arrival order
	- Short process stuck waiting for longer process - convoy effect
	- No differentiation between processes
	- Nonpreemptive - Once the CPU has been allocated to a process, that process keeps the CPU until it releases the CPU, either by terminating or by requesting I/O.

### Shortest Job First (SJF)
- Schedule process with shortest burst time first
	- associate with each process the length of its next CPU burst (How?)
	- use these lengths to schedule the process with the shortest time
	- waiting-time optimal, but poor on turnaround and response times
#### Nonpreemptive SJF Example
![[Pasted image 20231001181214.png]]
#### Predicting Burst Times
- **Predicting ?**
	- Knowing exact burst times is impossible, must be predicted
	- Typically, should have some correlation with previous bursts.
- Exponential average based prediction:
	![[Pasted image 20231001181708.png]]
	- ***r_n:*** Predicted value for n-th burst
	- ***t_n:*** Actual time of n-th burst
	![[Pasted image 20231001182018.png]]
	- ***a*** must be in [0, 1], typically = 1/2
#### Preemptive SJF
- Shortest Remaining Time First (SRTF)
	- Stop current process and schedule new one if the burst time of latter is shorter than the remaining time of former.
	- Now we add the concepts of varying arrival times and preemption to the analysis considering four processes.
	![[Pasted image 20231001182506.png]]
	Process ***P1*** is started at time 0, since it is the only process in the queue. Process ***P2*** arrives at time 1. The remaining time for process ***P1*** (7 milliseconds) is larger than the time required by process ***P2*** (4 milliseconds), so process ***P1*** is preempted, and process ***P2*** is scheduled.
	- **`Waiting time`** `= Start time of last executed segment - Arrival time - Scheduled time of all prior segments`
		- ***P1*** = 10 - 0 - 1 = 9ms
		- ***P2*** = 1 - 1 - 0 = 0ms
		- ***P3*** = 17 - 2 - 0 = 15ms
		- ***P4*** = 5 - 3 - 0 = 2ms
		- Avg = (9 + 0 + 15 +2)/4 = 26/4 = 6.5ms
### Round Robin (RR)
- FCFS with preemption to enable time sharing
	- Defines a time quantum/slice (10 - 100 ms)
	- **Circular operation**
		- Process bursts sequentially, each process get up to 1 slice
		- The ready queue is treated as a circular queue. The CPU scheduler goes around the ready queue.
- Implementable using a circular FIFO queue
	- Timer interrupts every quantum to schedule next process
	- Interrupt occurs = context switching, current process sent to tail
	- A terminating process releases CPU for next process

>The CPU scheduler picks the first process from the ready queue, sets a timer to interrupt after 1 time quantum, and dispatches the process.
  One of two things will then happen. The process may have a CPU burst of less than 1 time quantum. In this case, the process itself will release the CPU voluntarily. The scheduler will then proceed to the next process in the ready queue. If the CPU burst of the currently running process is longer than 1 time quantum, the timer will go off and will cause an interrupt to the operating system. A context switch will be executed, and the process will be put at the tail of the ready queue. The CPU scheduler will then select the next process in the ready queue. 

Pros - Fast response time
Cons - Long waiting and turnaround times
![[Pasted image 20231001184313.png]]
Textbook pg.210 for details
#### RR Facts and Considerations
![[Pasted image 20231001184549.png]]
#### Time Quantum and Context Switch Time
![[Pasted image 20231001184706.png]]
Assume, a process is of 10 time units. 
If q = 12 time units, the process finishes in less than 1 time q, with 0 context switch. 
If q = 6 time units, the process finishes in 2 time q, with 1 context switch. 
If q = 1 time unit, process finishes in 10 time q, with 9 context switches. 