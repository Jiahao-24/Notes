### Overview
- Responsibilities categorized into **user-oriented** and **system-oriented**
![[Pasted image 20230913180009.png]]
### Process Management
- `Process`: A program or part-of-program in execution
	- Can be single- or multi-threaded:
		- Single-threaded: Executed sequentially using one program counter.
		- Multi-threaded: **Threads(?)** executed in parallel using one counter each
	- Highly interacts with the OS:
		- To access resources: CPU time, memory, I/O, files
		- To releases these resources when completed or terminated
- `OS Responsiblity`
	- Creating, suspending, resuming, and deleting user/system processes
	- Scheduling processes and threads on the CPU(s)
	- Providing mechanisms for process synchronization/communication
	- Providing mechanisms for handling **deadlock(?)**
### Memory Management
- For a program to be executed, instructions and data must be loaded into the system's memory (main, cache, virtual)
- Modern systems run more than one program at a time:
	- Must all be kept in the memory
	- Organization and access memory locations of different programs needs strict management mechanisms
- `OS Responsiblity`
	- Deciding processes and data to move into and out of memory
	- Allocating and deallocating memory space as needed
	- Keeping track of memory parts being used by each program
### Storage & I/O Management
- `Storage`:
	- Managing programs and/or data not fitting in memory or kept for long time in mass-storage
	- Moving programs and data between memory and storage
- `I/O`: Managing data transfer to and from I/O
	- Buffering: Storing data temporarily while it is being transferred
	- Caching: Storing parts of data in faster storage for performance
	- Spooling: Overlapping output of one job with input of other jobs
- `OS Responsiblity`
	- Storage partitioning and allocation
	- Disk scheduling
	- Interaction with I/O hardware, interfaces, and drivers
### File-System Management
- `File`: An abstraction of storage physical properties into a uniform logical unit of information storage
	- Consists of a collection of related information defined by its creator
	- Stored in media with different organization
	- Accessed by different types of user applications
	- Organized into directories for easy access and use
- `OS Responsibility`
	- Creating, manipulating and deleting files and directories
	- Mapping files onto secondary storage
	- Backing-up files onto stable storage
	- Enforcing different access privileges of files by different users
![[Pasted image 20230913185347.png]]
