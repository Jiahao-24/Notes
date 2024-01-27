##### Recall: Concurrency - Concurrency in OS settings refers to the execution of multiple processes or threads on one or multiple CPU cores within a common time frame in a partial or full fashion.
- Partial Concurrency: Interleaving
	- Switching on the same CPU
	- Opportunistic
	- Time-sharing
- Full Concurrency: Parallelism:
	- Parallel over multiple CPUs or cores
### Race Condition
- **Problem Source:** Process concurrency while having access to shared data, which can be read from or written to.
	- Partial Concurrency: Interleaved instruction execution resulting in an **unordered** and/or **alternating** manipulation of shared data
	- Full Concurrency: Simultaneous access and manipulation to shared data
- #### Race Condition - the concurrent access and manipulation of shared data by multiple processes or threads may result scenarios in which this shared data as well as the inputs, variables, and outputs of each of the processes are susceptible to corruption
	- Defined by two conditions:
		- **Multiple** processes executing **concurrently** with/on **shared data**
		- Outcome depends on the **order** of execution and/or speed or alternating access of data
	- Can result in corrupting the data and/or the processes
	##### Example 1 - two parallel process:
	- Two processes creating children processes using the fork() system call  in parallel
		- Race condition on kernel variable **`next_available_pid`**
		![[Pasted image 20231001204507.png]]
		- Result depends on:
			- Which call is started first and how late the second is
			- How fast the **`next_available_pid`** updates its value
	##### Example 2 - partial concurrency and context switching
	- Threads T1 and T2 with a shared variable i
	- T1:` ... ; i = i + 1 ; ...`
	- T2:` ... ; i = i - 1 ; ...`
	- Result for one run per thread- context switching
		![[Pasted image 20231001204914.png]]
	- Value of `i` after threads terminate depends on
		- Which thread is started first
		- Number of times `i` changes between two switches of either threads
		- Relative sizes of each thread
### Synchronization ?
- Observed problems very frequent
	- Especially in multi-processing and multi-threading
- Solution: **Process Synchronization**
	- Tolls and mechanisms to manage/control the execution and access to shared resources and data
- Importance:
	- Provide control when order or speed of execution matters for the concurrently running process and threads
	- Allows means to transparent process communication
	- Ensure date integrity and outcome correctness
### Critical Sections
- Section(s) of process code where it changes common variables, tables, files, etc.
- #### Example
	![[Pasted image 20231003203841.png]]
- When one process in critical section, no other may be in critical section
	-> Permission: Entry section
	-> Release: Exit section
	-> Enable other process to enter their critical section
	![[Pasted image 20231003204131.png]]
### Critical Section Problem
- Designing protocols to synchronize process access to their critical sections - to execute instructions on shared data in a cooperative, orderly, and safe manner
- Solution Requirements:
	- `Mutual exclusion:` **At most, one process** executing its critical section at a time
	- `Progress:`
		- Process must leave critical section once done
		- Process must notify other processes once done with critical section
		- Process outside critical section should not block others to get in
	- `Bounded waiting:` A process must not wait indefinitely to access its critical section (**wait time to enter critical section is bounded, not indefinite**)
### Process Precedence Graphs
- Orderly execution of bursts and critical sections (fundamental problem of process sync.)
	- Defined by **precedence** relationships
- PPG: A directed graph used to graphically express the execution precedence relationship of multiple processes
	![[Pasted image 20231003204852.png]]
	