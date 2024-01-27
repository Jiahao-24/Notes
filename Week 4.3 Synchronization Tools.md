#### Recall: 
- **Critical Sections:** Parts of code where shared data (variables/tables/files, etc.) are being accessed
- **Critical Section Problem:** Designing protocols to synchronize access to critical sections
	##### Three requirements:
	- Mutual exclusion
	- Progress
	- Bounded waiting
- **Precedence Problem:** Designing tools to impose precedence relations between processes and critical sections

##### Race Condition
A race condition is a scenario occurs due to multiple cooperative process sharing the same resource or executing the same piece of code, where the output state can vary depending one the order of execution of the processes. Here all the processes race to say its output is correct, while it is actually not.
- **`counter++`** could be implemented as
```C
register1 = counter
register1 = register1 + 1
counter = register1
```
- **`counter--`** could be implemented as
```C
register2 = counter
register2 = register2 - 1
counter = register2	
```
- Consider this execution interleaving with "`counter = 5`" initially:
	S0: producer execute **`register1 = counter`**
	`{register1 = 5}`
	S1: producer execute **`register1 = register1 + 1`**
	`{register1 = 6, counter = 5}`
	S2: consumer execute **`register2 = counter`** 
	`{register2 = 5, counter=5}`
	S3: consumer execute **`register2 = register2 – 1`** 
	`{register2 = 4}`
	S4: producer execute **`counter = register1`** 
	`{counter = 6 }`
	S5: consumer execute **`counter = register2`** 
	`{counter = 4}`
	
	The wrong result due to two processes concurrently accessing the same data and it is not synchronized. We need process synchronization to maintain data consistency.
### Mutex Locks
- #### Mutex Locks: Short for mutual exclusion locks
	- Lock: Atomic Boolean variable indicating if lock is available or not
	- Process acquire lock before entering critical section using `acquire()`
		- Succeeds if lock is available
		- Blocked if unavailable => loops continuously  until available: `Spinlock`
	- Process release lock after existing critical section using `release()`
	![[Pasted image 20231003212107.png]]
- #### Pros and Cons
	- ##### **Pros**
		- No context switch required when a process waits on a lock
			- Much preferred in certain circumstances on multicore systems
			- Much preferred if locks will be held for short periods
	- ##### **Cons**
		- Busy waiting: Continuous loops in the call to `acquire()`
			- Delays process
			- Wastes CPU cycles
		- Lock contention: 
			Highly contended locks = Degraded performance
### Semaphore
- #### Synchronization tool that provides more sophisticated ways (than Mutex locks) for process to synchronize their activities.
- Semaphore **S - non-negative integer** variable and shared between threads. 
	- ##### This variable is used to solve the critical section problem and to achieve process synchronization in the multiprocessing environment.
- Can only be accessed via two uninterruptible (atomic) operations, apart from initialization
	- **`wait()`** and **`signal()`**
		- Originally called P() *{from Dutch word Proberen meaning ‘to test’}* and V() *{from Dutch word Verhogen meaning ‘to increment’}*
- #### Definition of the **`wait()`** operation
	```C
	wait(S) {
		while (S <= 0)
			; // busy wait (semicolon after condition means nothing to execute, just wait. Test <=0 means other thread already in critical section.)
		s--; // because it needs to inform other process know
	}
	```
- #### Definition of the `Signal()` operation
	```C
	signal(S) { 
		S++; //means to tell other process that a process completes its operation and other process can use it. }
	```
- Note: the two semaphore operations must be executed individually, i.e. when one operation is executed, all other process can't do any operation on that semaphore(integer variable S)
### Semaphore Usage
- **Binary semaphores:** Behave similarly to mutex locks
	> 0 means some process/thread is already in the critical section and the requesting process must wait; 1 means it is free for the other process to enter in a critical section or to use the shared resource
	- Used to solve various synchronization problems:
	![[Pasted image 20231003214613.png]]
- **Counting Semaphores:**
	- Integer value can range over an unrestricted domain
	- Control access to finite resources
	- Semaphore initialized to number of instances
		```C
		wait(S);
		  print();
		signal(S);
		```
	- Must guarantee that no two processes can execute the **`wait()`** and **`signal()`** on the same semaphore at the same time
	- Thus, the implementation becomes the critical section problem where the **`wait`** and **`signal`** code are placed in the critical section
	- Could now have **busy waiting** in critical section implementation
		- But implementation code is short
		- Little busy waiting if critical section rarely occupied
### Eliminating Busy Wait
- **Waiting queue** for each semaphore S:
	- Process added to queue when semaphore is unavailable
	- Scheduler selects other process(es) to execute
	- Process brought back to ready queue when signal() is executed
- Implementation
	![[Pasted image 20231003225732.png]]
	- **`List structure`**
		- Link field in PCB
	![[Pasted image 20231003225911.png]]
	- **`sleep()`**
		- Suspends process and move to waiting queue
	![[Pasted image 20231003225956.png]]
	- **`wakeup()`**
		- Awakens process from waiting queue and brings it in the ready queue
	
	check pg. 274 for details
### Problem with Semaphores
- #### Incorrect use:
	- Hard-to-detect timing errors
	- Violation of critical section problem criteria
		- Interchange of `signal()` with `wait()` => **Violates mutual exclusion**
		- Replace `signal()` with `wait()` => **Violates progress & bounded waiting**
		- Omit `wait()` or `signal()` => **Violates either of the above**
		![[Pasted image 20231003230422.png]]
- **Deadlocks:** Circular waits on semaphores
### Deadlock and Starvation
- #### Deadlock - two or more processes are waiting indefinitely for an event that can be caused by only one of the waiting processes
- Let ***S*** and ***Q*** be two semaphores initialized to 1
	![[Pasted image 20231003230718.png]]
- **Starvation - indefinite blocking**
	- A process may never be removed from the semaphore queue in which it is suspended
- **Priority Inversion** - scheduling problem when lower-priority process holds a lock needed by higher-priority process
	- Solved via **priority-inheritance protocol**
### Monitors
- Abstract data types, encapsulating functions and data for exclusive use
	- Only one process enters monitor at a time
	- Local functions can access only local variables
	- Local variables can be accessed only by local functions
	![[Pasted image 20231003232045.png]]
	![[Pasted image 20231003232119.png]]
- **Pure Monitors:**
	- Only one process enters monitor at a time
	- Other processes placed at entry queue until current one exits
	- Lack mechanisms for processes to access the monitor while others are waiting
### Monitors with Conditions
- Condition variables: `condition x y ... ;`
	- Represent some conditions that a process can wait on
	- One queue per variable
- Implementation
	- Variables invokable by **`wait()`** and **`signal()`**
	- **`x.wait()`** ⇒ process suspended
		- Queued in an x-variable queue
		- Lock given to entry queue
	- **`x.signal()`** ⇒ One process awakened
		- FCFS
		- Priority: Conditional wait **`x.wait(c)`**
	- **`x.signal()`** has no effect if no process is suspended on variable x
	![[Pasted image 20231003232408.png]]
#### Awakened Process
- Notify and Continue
	- Notifying process keeps monitor lock
		- Continues execution ⇒ **No penalty for notifying**
	- Awakened process wait at the entry queue for monitor lock
		- Awaited condition no longer true ⇒ **Re-suspended**
- Notify and Wait
	- Notifying process preempted
		- Replaced in entry queue ⇒ **Further delays**
	- Notified process gets the lock
		- Executes immediately
		- Awaited condition certainly true ⇒ **No re-suspensions**