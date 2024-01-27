### Threads
* `Threads:` segments or parts of the instruction set of a process that can run concurrently:
	- A thread is the most basic unit of CPU utilization/the smallest unit of execution within a process.
	- Implement different tasks within the same process:
		- `Word processor`: Different threads for response to keystrokes, display updates, data fetching, spell checking
		- `Web browser`: Different threads for displaying received text/images and receiving other text/images from the network card
	- Threads belonging to the same process share its code section, data section, and OS resources
- `Benefits`
	- Better responsiveness
	- Improved utilization and economy through resource sharing
	- Readiness and scalability in handling concurrent processes
### Identifiers and Resources
- Threads created using functions, objects, or system calls `CreateThread()` or `new_Thread()`
- Identifiers
	- Thread ID (tid)
	- Separate program counter (PC) - keep track of the next instruction to execute
	- A thread control block (TCB) including
		- tid
		- similar information as in PCB
		- a pointer to the PCB of the original process
- Resources:
	- Threads share code, data, and file space with the main process
	- Each thread has its own stack, and a set of registers
### Process vs Thread Creation
![[Pasted image 20230914084753.png]]
Thread creation is **light weight** compared to process creation. (new thread vs child process)
Note that the child process takes the same amount of memory space as the parent process when `fork()` is called.
### Programming Challenges
- `Identifying tasks`: Examining applications to find areas that can be divided into separate, concurrent sub-tasks
- `Balance`: Ensuring sub-tasks perform equal work of equal value
- `Data splitting`: Dividing data used in separate threads
- `Data dependency`: Examining data for dependencies between two or more sub-tasks
- `Testing and debugging`: Testing and debugging concurrent programs that will run on multiple cores
### Implicit Threading:
Implicit threading, also known as automatic threading or implicit parallelism, refers to a programming model or approach in which **the creation and management of threads are handled by the system or runtime environment**, rather than being explicitly programmed by the developer.
- Transfer thread creation to compilers and run-time libraries
- Developers identify tasks only and write them as functions
- Run-time libraries maps sub-tasks into separate thread