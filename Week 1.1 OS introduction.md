A computer system can be divided roughly into four components: the `hardware`, the `operating system`, the `application programs`, and a `user`.
### Basic elements of computer systems
A modern general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus that provides access between components and shared memory.
![[Pasted image 20230913134638.png]]

* `Hardware Elements` - CPU, memory, input/output (I/O) devices, storage — provides the basic computing resources for the system.
* `Hardware Connectors(Bus)`: Control Bus, Address Bus, Data Bus
* `CPU` composition:
	* Arithmetic Logic Unit (ALU)
	* Registers and cache
	* Control unit
	* Internal bus
* `CPU` architectures:
	![[Pasted image 20230913135804.png]]
	
* `Storage` Hierarchy:
	* Memory & Mass-storage: Hardware storing information
	![[Pasted image 20230913142219.png]]
	
* `I/O` devices: all types of peripherals
	* Monitors, mouse, keyboard, printer, projector, network card
	* Hardware interrupts used to read from and write in I/O
	* Interaction with other devices: 
		* typically through the CPU
		* Direct memory access: direct transfers of date to or from the device and main memory, with no intervention by the CPU
### What is an operating System? What does it do?
An operating system is **software that manages a computer’s hardware**. It also **provides a basis for application programs** and acts as **an intermediary between the computer user and the computer hardware**.
![[Pasted image 20230913140937.png]]
* **A Control Program** - a.k.a. `The Kernel`
	- The only program that runs all the time on an ON machine
	- Most closely interact with hardware
	- Eases and manages execution of diverse user programs
	- Prevents errors and improper use of computer
- **Resource Allocator/Manager**
	- Acts as the manager of hardware resources
	- Grants, shares, and releases access of user programs to these resources 
	> same as governments, which performs no useful function by itself, but provides an environment within which other programs can do useful work
- **Improved Resource Utilization**
	- Single programs cannot keep CPU or I/O devices busy
	- Utilization can be improved through `multi-prcoessing`:
		- `Multiprogramming`: multiple programs lined-up so CPU switch between them when another gets idle or have to wait (e.g, for I/O)
		- `Multitasking`: Time sharing of multiple active programs on CPU(s) • 
		- `Multithreading`: An extension of multitasking to the “threads” of the same active program
* **OS roles as a `control program`/ `resource allocator/manager`**
	- Needed to manage program switching
	- Needed for CPU scheduling
	- Needed for memory swapping or memory virtualization
### Interrupts
- **Definition**
	* Events that alters the sequence of instructions executed by the CPU
	* Initiate interrupts service routine (ISR) or interrupt handlers
		- Program executed to resolve the cause of the interrupt
		- Located by an Interrupt Vector Table (IVT) stored in the memory
- **Types**
	- `Hardware`: Triggered by hardware to get OS attention
		- Devices send signal to interrupt controller
		- CPU preempts(预先制止) running low-level program to handle device request
	- `Software`: Following the execution of a code instruction:
		- **Exceptions**: When instruction performs an illegal action
		- **System calls**: Request from a user program to get a service from the OS
	- `Other`: Inter-process, Spurious(stops an unwanted interrupts), etc.
![[Pasted image 20230913145142.png]]
### Timers
- A tool to prevent infinite loop
	- OS sets a counter for each active program
	- Counter is decremented with every system clock tick
	- Interrupt when counter reaches 0
	- Interrupt transition control to OS
		- Treated like a fatal(致命) error
		- Give more time
- Example from Linux
	- Parameter HZ specifies frequency of timer interrupts.
	- HZ = 250 means 250 interrupts per second (1 interrupt every 4 ms)
### Dual-Mode Operation
![[Pasted image 20230913145549.png]]
- **User Mode (Mode bit = 1)**
	- Where user programs are executed
	- No access to peripherals and other OS-exclusive instructions
- **Kernel Mode (Mode bit = 0)**
	- When a user application requests a service from OS
	- OS executing `privileged` instructions
### Interactions with System Calls
- User interaction with system calls runs through an interface
	- Intercepts function calls in the API and invokes the necessary system calls within the OS
	- Enables portability, makes programmers' life easier
![[Pasted image 20230913145857.png]]