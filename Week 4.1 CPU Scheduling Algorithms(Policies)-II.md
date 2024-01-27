### Priority-Based Scheduling (PS)
- Processes scheduled according to associated priorities
	- The higher the process priority, the sooner it executes
		- FCFS: Equal priorities
		- SJF: Follows priority defined by the inverse of burst time
	- Priorities can be defined either inside or outside OS
		- **Inside**: Using measurable OS quantities (e.g., time/memory/file limits)
		- **Outside**: Using process importance, prices, etc.
	- Can be preemptive or non-preemptive:
		- **Preemptive**: preempt the CPU if the priority of the newly arrived process is higher than the priority of the currently running process.
		- **Nonpreemptive**: simply put the newly arrived process at the head of the ready queue if the priority is higher.
	- Time sharing versions (e.g., Priority Round Robin(PRR))
	- **Problem**: Starvation of low-priority processes.
		- *Solutions:*
			- Aging-increase priority with age in ready queue
			- More elaborate time-sharing options
	#### Example
	![[Pasted image 20231001191901.png]]
#### Rate Monotonic Scheduling(RMS) (subclass of Realtime priority scheduling)
- Sub-class of real-time priority-based schedulers handling periodic processes
	- Generates bursts of duration t at constant intervals (or period p)
	- Needs CPU time t within this period (t <= p)
	![[Pasted image 20231001192345.png]]
- ##### **Periodic Processes**: RMS is designed for processes that have to perform their tasks periodically. For example, these processes might need to run every 10 milliseconds, 50 milliseconds, or some other fixed time interval.
- ##### **Static Priorities**: In RMS, when processes are admitted into the system, they are assigned static priorities that are inversely proportional to their periods. This means that processes with shorter periods get higher priorities, and processes with longer periods get lower priorities.
	- **Why shorter periods get higher priorities**: The reasoning behind this is that processes with shorter periods need to run more frequently. If they don't get CPU time when they need it, they might miss their deadlines and disrupt the real-time system's behaviour.
- ##### **Preemptive**
- ##### processes are checked for feasibility before they are admitted into the system

### Multi-level Queue Scheduling
- With both priority and round-robin scheduling, all processes may be placed in a single queue.
	- Mixed processes of different types and priorities
	- Require O(N) search at each context switch
- Solution: **Multi-level Queue Scheduling**
	- One separated queue per priority
		- Processes are placed permanently to one queue
		- Easy priority scheduling
		- Easy FCFS or RR scheduling in same priority
	- Natural separations of different process types:
		- Have different response-time requirements
		- Use different scheduling policies per queue (i.e., type)
	![[Pasted image 20231001201052.png]]
	![[Pasted image 20231001201105.png]]
### Multilevel Feedback Queue Scheduling
- A process can move between the various queues as opposed to multilevel queue; **aging** can be implemented this way
- This idea is to separate processes according to the characteristics of their **CPU** bursts -> processes requiring high CPU burst move to low-priority queue
- Multilevel-feedback-queue scheduler defined by the following parameters:
	- number of queues
	- scheduling algorithms for each queue
	- method used to determine when to upgrade a process priority
	- method used to determine when to demote a process priority
	- method used to determine which queue a process will enter when that process needs service
	#### Example
	![[Pasted image 20231001201912.png]]
	- Three queues:
		- Q0 - RR with time quantum 8ms
		- Q1 - RR with time quantum 16ms
		- Q2 - FCFS
	- Scheduling
		- The scheduler first executes all processes in queue Q0
		- A new job enters queue Q0
			- When it gains CPU, it receives 8ms
			- If it does not finish within this time, job is moved to the tail of queue Q1
		- At Q1 it receives 16 additional ms
			- If it still does not complete, it is preempted and moved to queue Q2
