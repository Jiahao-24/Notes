Road Map
![[Pasted image 20230913185442.png]]
### Process - Definition
- An instance of a program or part-of-program in execution
	- Program becomes process when loaded into memory
	- Execution of user program started via OS GUI
	- One program can run as several separate processes
	- A process can be an execution environment for other code, e.g. Java Virtual Machine (JVM)
- Process execution progresses in sequential fashion
	- Status defined by:
		- Program counter
		- Content of CPU registers
	- Multiple processes of same program treated as separate execution sequences with different counters
### Two Types of Processes
* `System processes` that execute system codes, and system processes are executed in *kernel mode(master mode)*
* `User processes` that execute user codes (but treated in the same way), whereas user processes are executed in *user mode(slave mode)*.
### System Calls
* `System calls` constitutes an interface between the *user(programs)* and the *services* that are made available by an `operating system`.
	- A systems call is a programmatic way in which a computer program requests a service from the operating system.
	- Each operating system has its own system calls (e.g., Linux has 300, FreeBSD has 500, and Windows 7 has 700).
	- System calls point to specific programs called routines that are written in C, C++, or assembly language.
	- Operating systems provides an API (Application Programming Interface) as an interface between user programs and system calls.
	- Different categories of system calls
	![[Pasted image 20230913190519.png]]
	![[Pasted image 20230913190544.png]]
### Memory Layout of a Process
- Fixed Size Sections:
	- **Text Section:** Executable code
	- **Data Section:** Program's global variables
- Variable Size Sections:
	-  **Heap Section:** Dynamically allocated memory during run time
	- **Stack Section:** Temporary storage when OS is invoking functions for the process
	- Both sections shrink/expand dynamically during execution:
		- `Heap`: As per the dynamically allocated size
		- `Stack`: As per the size of the stored activation record of the different called functions.
![[Pasted image 20230913191127.png]]
![[Pasted image 20230913191302.png]]
### Process State
![[Pasted image 20230913191354.png]]
As a process executes, it changes **state** defining its current activity
- `New`: When being created
- `Running`: Instructions are being executed
- `Waiting (a.k.a. blocked)`: The process is waiting for some event to occur
- `Ready`: The process is waiting to be scheduled
- `Terminated`: The process has finished execution
### Process Creation
- A system program is created as the first process at boot time (e.g., **`systemd`** in Linux)
- Parent processes create children processes
	- Using **`fork()`** in UNIX or **`CreateProcess()`** in Windows
	- Children processes can act as parents by creating other processes
	- Each created process is assigned a process identifier (pid)
	> `pstree` command in linux
	![[Pasted image 20230913191932.png]]
```C
int main()
{
	/* fork a child process */
	fork();
	
	/* fork another child process */
	fork();
	
	/* and fork another */
	fork();

	return 0;
}
```
`How many process to be created through this code?`
- Fork #1 creates an additional processes. You now have two processes.  
- Fork #2 is executed by two processes, creating two processes, for a total of four.
- Fork #3 is executed by four processes, creating four processes, for **a total of eight**. Half of those have pid == 0 and half have pid != 0
### Process Space and Execution
- Once a process in created, it is allocated an address space in the memory for execution
- Content of address-space
	- Child duplicate of parent (same data and code) (part of **`fork()`**)
	- Child has a program loaded into it (e.g. using **`exec()`** in UNIX)
- Execution options
	- Parent and children execute concurrently(同时)
	- Parent waits until children terminate (e.g. Using **`wait()`** in UNIX)
![[Pasted image 20230913192813.png]]
### Process Control Block (PCB)
- `PCB`: Representation of each process in the OS
	> serves as the repository for all the data needed by the OS to handle to (start, restart, schedule, monitor resources, etc.)
- `PCB` contains many pieces of information associated with a specific process, including these:
	- Process state
	- Program counter
	- CPU registers
	- CPU scheduling info
	- Memory management info (i.e., a address space)
	- I/O status info
	- Accounting info
![[Pasted image 20230913193433.png]]
![[Pasted image 20230913193606.png]]
### Process Termination
- Processes terminates itself when execution is completed.
	- Asks OS to delete it (e.g., using **`exit()`** in UNIX)
	- May return a status value to parent process (e.g., using **`wait()`**)
	- Resources deallocated by OS (such as its address-space)
	- Entry in process table must remain until parent calls
	**`wait() - to get the exit status of child`**
- Parent process terminates child process
	- Using **`abort()`** in UNIX or **`TerminateProcess()`** in Windows
	- Occurs when:
		- Child exceeded allocated resources
		- Task assigned to child no longer required
		- Parent exiting, and OS not allowing child to continue
### Cooperation and Communication
- Cooperating processes:
	- Affecting, affected, or sharing information with other executing processes
	- Reasons: 
		- Information sharing
		- Computation speedup
		- Modularity
- Need inter-process communication
	- Two models of IPC
		- Shared memory
		- Message passing: `send()` and `receive()`
			- Synchronous or blocking
			- Asynchronous or non-blocking
	![[Pasted image 20230918223137.png]]