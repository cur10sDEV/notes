# Chapter 1 - The Big Picture

- ## 1.1 Abstraction

  A fancy of saying that you can ignore most of the details that make up a piece that you're trying to understand.

- ## 1.2 Subdivision

  There are many terms for an abstracted subdivision in computer software—including subsystem, module, and package — but we’ll use the term component in this chapter because it’s simple. When building a software component, developers typically don’t think much about the internal structure of other components, but they do consider other components they can use (so that they don’t have to write any additional unnecessary software) and how to use them.

- ## 1.3 Levels and Layers of Abstraction in a Linux System
  A linux system has three main levels or layers, classifications(or groupings):
  - The **_hardware_** at the base
    - Memory, CPU - to perform read and write to memory, devices - disks and network interfaces(cards, ports)
  - The next level up is the **_kernel_** - the core of the operating system. the kernel is software residing in the memory that tells the cpu where to look for its next task.
    - Acting as a mediator, the kernel manages the hardware(especially the main memory) and is the primary interface between the hardware and the any program running.
  - **_Processes_** — the running programs that the kernel manages—collectively make up the system’s upper level, called user space. (A more specific term for process is user process, regardless of whether a user directly interacts with the process. For example, all web servers run as user processes.)
  - There is a critical difference between how the kernel and the user processes run: the kernel runs in **_kernel mode_** and the user processes run in **_user mode_**.
    - Code running in kernel mode has unrestricted access to the processor and main memory. This is a powerful but dangerous privilege that allows the kernel to easily corrupt and crash the entire system. The memory area that only the kernel can access is called kernel space.
    - User mode, in comparison, restricts access to a (usually quite small) sub-set of memory and safe CPU operations. User space refers to the parts of main memory that the user processes can access. If a process makes a mistake and crashes, the consequences are limited and can be cleaned up by the kernel.
    - Can a user process completely wreck the system? With correct permissions - yes.

> The Linux kernel can run kernel threads, which look much like processes but have access to kernel space. Some examples are kthreadd and kblockd.

- ## 1.4 Hardware - Understanding Main Memory
  Main memory is just a big storage area for a bunch of 1s and 0s. Each slot for a 0 or a 1 is called a **_bit_**.
  - This is where the running kernel and processes reside.
  - All input and output from peripheral devices flows through main memory as a bunch of bits.
  - A CPU is just an operator on memory, it reads it's instructions and data from the memory and write back data to memory.
  - Strictly speaking, a state is a particular arrangement of bits.
  - But Abstract definition of state is **"what something has done or doing at the moment"**.

> Because it’s common to refer to the state in abstract terms rather than to the actual bits, the term image refers to a particular physical arrangement of bits.

- ## 1.5 The Kernel

  Nearly everything that the kernel does revolves around main memory. One of the kernel’s tasks is to split memory into many subdivisions, and it must maintain certain state information about those subdivisions at all times. Each process gets its own share of memory, and the kernel must ensure that each process keeps to its share.

  The kernel is in charge of managing tasks in four general system areas:

  - **Processes** - The kernel is responsible for determining which processes are allowed to use the CPU.
  - **Memory** - The kernel needs to keep track of all memory - currently allocated, shared between processes, and free.
  - **Device Drivers** - The kernel acts as an interface between hardware and processes. It's actually the kernel's job to operate the hardware.
  - **Syscalls and Support** - Processes normally use system calls to communicate with the kernel.

- ### 1.5.1 Process Management

  Process management describes the starting, pausing, resuming, scheduling, and terminating of processes.

  - You might see multiple applications running on a machine "simultaneously", however processes behind these applications typically do not run exactly at the same time

  - Consider a system with a one-core CPU. Many processes may be able to use the CPU, but only one process can actually use the CPU at any given time. In practice, each process uses the CPU for a small fraction of a second, then pauses; then another process uses the CPU for another small fraction of a second; then another process takes a turn, and so on. The act of one process giving up control of the CPU to another process is called a **_context switch_**.

  - Each piece of time—called a **_time slice_**—gives a process enough time for significant computation (and indeed, a process often finishes its current task during a single slice). However, because the slices are so small, humans can’t perceive them, and the system appears to be running multiple processes at the same time (a capability known as multitasking).

> The Kernel is responsible for context switching.

- To understand how this works, let’s think about a situation in which a process is running in user mode but its time slice is up. Here’s what happens:

  > 1. The CPU (the actual hardware) interrupts the current process based on an internal timer, switches into kernel mode, and hands control back to the kernel.
  > 2. The kernel records the current state of the CPU and memory, which will be essential to resuming the process that was just interrupted.
  > 3. The kernel performs any tasks that might have come up during the preceding time slice (such as collecting data from input and output, or I/O, operations).
  > 4. The kernel is now ready to let another process run. The kernel analyzes the list of processes that are ready to run and chooses one.
  > 5. The kernel prepares the memory for this new process and then prepares the CPU.
  > 6. The kernel tells the CPU how long the time slice for the new process will last.
  > 7. The kernel switches the CPU into user mode and hands control of the CPU to the process.

- The context switch answers the important question of when the kernel runs. The answer is that it runs between process time slices during a context switch.

- In case of a multi-CPU system, the kernel dosen't need to relinquish control of it's current CPU in order to allow a process to run on a different CPU, and more than one processes may run at a time.

- ### 1.5.2 Memory Management

  The kernel must manage memory during a context switch, which can be a complex job. The following conditions must hold:

  - The kernel must have its own private area in memory that user processes can’t access.
  - Each user process needs its own section of memory.
  - One user process may not access the private memory of another process.
  - User processes can share memory.
  - Some memory in user processes can be read-only.
  - The system can use more memory than is physically present by using disk space as auxiliary.

  Modern CPUs include a **memory management unit (MMU)** that enables a memory access scheme called **virtual memory**. When using virtual memory, a process does not directly access the
  memory by its physical location in the hardware. Instead, the kernel sets up each process to act as if it had an entire machine to itself. When the process accesses some of its memory, the MMU intercepts the access and uses a memory address map to translate the memory location from the process point of view into an actual physical memory location in the machine. The kernel must still initialize and continuously maintain and alter this memory address map. For example, during a context switch, the kernel has to change the map from the outgoing process to the incoming process.

> The implementation of a memory address map is called a page table.

- ### 1.5.3 Device Drivers and management

  A device is typically accessible only in kernel mode because improper access (such as a user process asking to turn off the power) could crash the machine. A notable difficulty is that different devices rarely have the same programming interface, even if the devices perform the same task (for example, two different network cards). Therefore, device drivers have traditionally been part of the kernel, and they strive to present a uniform interface to user processes in order to simplify the software developer’s job.

- ### 1.5.4 System Calls and Support

  **System calls (or syscalls)** perform specific tasks that a user process alone cannot do well or at all.

  Two system calls, fork() and exec(), are important to understanding how processes start:

  - **fork()** - When a process calls fork(), the kernel creates a nearly identical copy of the process.
  - **exec()** - When a process calls exec(program), the kernel loads and starts program, replacing the current process.

  - Other than init (see Chapter 6), all new user processes on a Linux system start as a result of fork(), and most of the time, you also run exec() to start a new program instead of running a copy of an existing process.
    For example: When you enter **ls** into a terminal window, the shell that's running inside the terminal window calls **fork()** to create a copy of the shell, and this new copy will call **exec(ls)** to run ls.<img here>

  The kernel also supports user processes with features other than
  traditional system calls, the most common of which are pseudodevices. Pseudodevices look like devices to user processes, but they’re implemented purely in software. This means they don’t technically need to be in the kernel, but they are usually there for practical reasons.

> Technically, a user process that accesses a pseudodevice must use a system call to open the device, so processes can’t entirely avoid system calls.

- ## 1.6 User Space
  The main memory that the kernel allocates for user processes is called **user space**.
  Because a process is simply a state (or image) in memory, user space also refers to the memory for the entire collection of runnig processes.
  > userland - an informal term used for user space, sometimes this also means the programs running in user space.

> All processes are essentially equal from kernel's point of view,they perform different tasks for users.

- Fig: Basic services are at the bottom level (closest to the kernel), utility services are at the middle, and applications that user interact with (touch) are at the top.<img here>
- Components can use other components, but they should be at the same service level or below.
- In reality, there are no rules in user space, most apps and services write diagnostic messages known as **logs**. Most pro-
  grams use the standard **syslog service** to write log messages, but some prefer to do all of the logging themselves.

- ## 1.7 Users

  The Linux kernel supports the traditional concept of a Unix user. A user is an entity that can run processes and own files. A user is most often associated with a username; for example, a system could have a user named billyjoe. However, the kernel does not manage the usernames; instead, it identifies users by simple numeric identifiers called user IDs.

  Users exist primarily to support permissions and boundaries. Every user-space process has a user owner, and processes are said to run as the owner. A user may terminate or modify the behavior of its own processes (within certain limits), but it cannot interfere with other users’ processes. In addition, users may own files and choose whether to share them with other users.

  A Linux system normally has a number of users in addition to the ones that correspond to the real human beings who use the system, but the most important user to know about is **_root_**.

  The root user is an exception to the preceding rules because root may terminate and alter another user’s processes and access any file on the local system. For this reason, root is known as the **superuser**.

  Groups are sets of users. The primary purpose of groups is to allow a user to share file access to other members of a group.

> As powerful as the root user is, it still runs in the operating system’s user mode, not kernel mode.
