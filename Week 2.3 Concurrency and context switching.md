### What is Concurrency?
Concurrency(并发) is the capability of a system to execute multiple tasks or processes at the same time, giving the illusion of simultaneous execution, and thereby improving system efficiency and responsiveness.
- `Partial Concurrency: Interleaving`:
	- Interleaved execution of tasks (processes or threads) on one CPU
	- Results in tasks partially overlap in time but may not run simultaneously
	- All the tasks make interleaved progress
	![[Pasted image 20230914204318.png]]
- `Full Concurrency: Parallelism`:
	- Parallel over multiple CPUS or cores
	- Results in tasks fully overlap in time and can execute simultaneously
	- more than one task at a time
	![[Pasted image 20230914211924.png]]
- `Hybird Concurrency`:
	- Parallel execution of interleaving processes on multiple cores
	![[Pasted image 20230914204539.png]]
### Types of Partial Concurrency
- `Opportunistic Interleaving`:
	- natural consequence of state transitions of processes
	- executing process voluntarily transitions to a wait state
	- another process in its ready state exploits the opportunity to execute on CPU
	- eventually, first process gets its opportunity to resume
- `Time Sharing`:
	- CPU time is divided among different processes in ready states
	- each process gets a time share(called slice) to execute on CPU
	- **forcefully** interrupted for next process to take its turn
	- eventually, first processes gets another turn
### Context Switching
The core of partial concurrency where OS procedure for CPU to switch between processes
- When CPU switches, the system must:
	- save the state and information (a.k.a. **context**) of the old process
	- load the saved context for the new process
- `Context`:
	- state of the process (ready, waiting, or terminated)
	- content of CPU registers including the program counter(PC) and the flags register
	- process address space
	- other fields as per the OS
### Illustrative Example
	![[Pasted image 20230915094651.png]]
### Pros & Cons
- `Pros`:
	- increased CPU throughput(吞吐量)
	- increased CPU response time
	- no one process from hogging on the CPU infinitely
	- fast high-priority process execution (along with scheduling)
- `Cons`:
	- switching duration is a wasted overhead
		- depends on the size of the context
		- context switching for threads in very fast compared to processes
	- processes must not be interrupted frequently
	- need for synchronization
### Types of Parallelism
- `Data Parallelism`:
	- distributes subsets of the same data across multiple cores
	- same operation on each
	![[Pasted image 20230914215115.png]]
- `Task Parallelism`:
	- distributing processes or threads across cores
	- unique operation for each of them
	![[Pasted image 20230914215252.png]]
	