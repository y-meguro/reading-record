# 概要

## 本

- [Operating System Concepts | Abraham Silberschatz, Peter B. Galvin, Greg Gagne](https://amzn.to/2CN0bst)
  - 参考: [Operating System Concepts - 9th edition](https://www.os-book.com/OS9/index.html)

## かかった時間

- x 時間

## 読む前の状態

- [作って理解するOS](create_and_understand_os.md) だけ読んだが、まだ理解の怪しい部分があったので、理解を深めようと読むことにした

## 読む前後の変化

- xxx

# 読書メモ

## Preface

- Level
  - at the junior or senior undergraduate level or at the first-year graduate level
- Content of This Book
  - Overview: Chapter 1 - 2
  - Process management: Chapter 3 - 7
  - Memory management: Chapter 8 - 9
  - Storage management: Chapter 10 - 13
  - Protection and security: Chapter 14 - 15
  - Case studies: Chapter 16 - 18

## 1: Introduction

- An operating is a program that manages a computer's hardware. It also provides a basis for application programs and acts as an intermediary between the computer user and the computer hardware.
- Some operating systems are designed to be convenient, others to be efficient, and others to be some combination of the two.
- Chapter Objectives
  - To describe the basic organization of computer systems.
  - To provide a grand tour of the major components of operating systems.
  - To give an overview of the many types of computing environments.
  - To explore several open-source operating systems.

### 1.1: What Operating Systems Do

- A computer system can be divided roughly into four components:
  - the hardware, the operating system, the application programs, and the users
- User View
  - Most computer users sit in front of a PC.
    - In this case, the operating system is designed mostly for ease of use, with some attention paid to performance and none paid to resource utilization - how various hardware and software resources are shared.
  - In other cases, a user sits at a terminal connected to a mainframe or a minicomputer.
    - The operating system in such cases is designed to maximize resource utilization.
  - In still other cases, users sit at workstations connected to networks of other workstations and servers.
    - Theire operating system is designed to compromise between individual usability and resource utilization.
- System View
  - In this context, we can view an operating system as a resource allocator.
- Defining Operating Systems
  - In general, we have no completely adequate definition of an operating system.
  - Operating systems exist because they offer a reasonable way to solve the problem of creating a usable computing system.
  - In addition, we have no universally accepted definition of what is part of the operating system.
    - A more common definition, and the one that we usually follow, is that the operating system is the one program running at all the times on the computer - usually called the kernel.
      - Along with the kernel, there are two other types of programs: system programs / application programs
  - Mobile operating systems often include not only a core kernel but also middleware.

### 1.2: Computer-System Organization

- Computer-System Operation
  - A modern general-purpose computer system consists of one or more CPUs and a number of device controllers connected through a common bus that provides access to shared memory.
  - For a computer to start runnning it needs to have an initial program to run. This initial program, or bootstrap program, tends to be simple. The bootstrap program must know how to load the operating system and how to start executing that system.
    - To accomplish this goal, the bootstrap program must locate the operating-system kernel and load it into memory.
  - THe occurrence of an event is usually signaled by an interrupt from either the hardware or the software.
- Storage Structure
  - The CPU can load instructions only from memory, so any programs to run must be stored there. General-purpose computers run most of theire programs from rewritable memory, called main memory.
  - A typical instruction - execution cycle, as executed on a system with a von Neumann architecture, first fetches an instruction from memory and stores that instruction in the instruction register. The instruction is then decoded and may cause operands to be fetched from memory and stored in some internal register. After the instruction on the operands has been executed, the result may be stored back in memory.
  - The most common secandary-storage device is a magnetic disk, which provides storage for both programs and data.
  - The main differences among the various storage systems lie in speed, cost, size, and volatility.
  - Solid-state disks は SSD のこと。非揮発性。
- I/O Structure
  - A large portion of operating system code is dedicated to managing I/O, both because of its importance to the reliability and performance of a system and because of the varying nature of the devices.
  - This form of interrupt-driven I/O is fine for moving small amounts of data but can produce high overhead when used for bulk data movement such as disk I/O. To solve this problem, direct memory access (DMA) is used.

### 1.3: Computer-System Architecture

- A computer system can be organaized in a number of different ways, which we can categorize roughly according to the number of general-purpose processors used.
- Single-Processor Systems
  - Until recently, most computer systems used a single processor. On a single processor system, there is one main CPU capable of executing a general-purpose instruction set, including instructions from user processes. Almost all single-processor systems have other special-purpose processors as well.
- Multiprocessor Systems
  - Within the past several years, multiprocessor systems (also known as parallel systems or multicore systems) have begun to dominate the landscape of computing.
  - Multiprocessor systems have three main advantages:
    - Increased throughput
    - Economy of scale
    - Increased reliability
  - A recent trend in CPU design is to include multiple computing cores on a single chip. Such multiprocessor systems are termed multicore.
  - Finally, blade servers are a relatively recent development in which multiple processor boards, I/O boards, and networking boards are placed in the same chassis. The difference between these and traditional multiprocessor systems is that each blade-processor board boots independently and runs its own operating system.
- Clustered Systems
  - Another type of multiprocessor system is a clustered system, which gathers together multiple CPUs.
  - Clustered systems differ from the multiprocessor systems in that they are composed of two or more individual systems - or nodes - joined together. Each node may be a single processor system or a multicore system.
  - the generally accepted definition is that clustered computers share storage and are closely linked via a local-area network LAN or a faster interconnect, such as InfiniBand.
  - Clustering is usually used to provide high-availability service.
  - Clustering can be structured asymmetrically or symmetrically.
    - In asymmetric clustering, one machine is in hot-standby mode while the other is running the applications.
      - The hot-standby host machine does nothing but monitor the active server. If that server fails, the hot-standby host becomes the active server.
    - In symmetric clustering, two or more hosts are running applications and are monitoring each other.
      - This structure is obviously more efficient, as it uses all of the available hardware. However it does require that more than one application be available to run.
  - If the applications and their data are stored on the SAN (Storage-area network), then the cluster software can assign the application to run on any host that is attached to the SAN.

### 1.4: Operating-System Structure

- One of the most important aspects of operating systems is the ability to multiprogram.
  - Multiprogramming increases CPU utilization by organizing jobs so that the CPU always has one to execute.
- Multiprogrammed systems provide an environment in which the various system resources are utilized effectively, but they do not provide for user interaction with the computer system. Time sharing (or multitasking) is a logical extension of multiprogramming.
  - In time-sharing systems, the CPU executes multiple jobs by switching among them, but the switches occur so frequently that the users can interact with each program while it is running.
- A program loaded into memory and executing is called process.
- A more common method for ensuring reasonable response time is virtual memory, a technique that allows the execution of a process that is not completely in memory.

### 1.5: Operating-System Operation

- A properly designed operating system must ensure that an incorrect program cannot cause other programs to execute incorrectly.
- Dual-Mode and Multimode Operation
  - In order to ensure the proper execution of the operating system, we must be able to distinguish between the execution of operating-system code and user-defined code.
  - At the very least, we need two separate modes of operation: user mode and kernel mode (also called supervisor mode, system mode, or privileged mode).
    - mode bit が 0 であれば kernel mode、1 であれば user mode。
  - The dual mode of operation provides us with the means for protecting the operating system from errant users - and errant users from one another.
  - 2 つの mode だけでなく、VMM を入れて 3 つにするものや、4 つの privilege level を持つものもある
  - System calls provide the means for a user program to ask the operating system to perform tasks reserved for the operating system on the user program's behalf.
- Timer
  - A timer can be set to interrupt the computer after a specified period.
  - We can use the timer to prevent a user program from running too long.

### 1.6: Process Management

- A program does nothing unless its instructions are executed by a CPU. A program in execution is a process.
- We emphasize that a program by itself is not a process. A program is a passive entity, like the contents of a file stored on disk, whereas a process is an active entity.
- The operating system is responsible for the following activities in connection with process management:
  - Scheduling processes and threads on the CPUs
  - Creating and deleting both user and system processes
  - Suspending and resuming processes
  - Providing mechanisms for process synchronization
  - Providing mechanisms for process communication

### 1.7: Memory Management

- The main memory is generally the only large storage device that the CPU is able to address and access directly.
- The operating system is responsible for the following activities in connection with memory management:
  - Keeping track of which parts of memory are currently being used and who is using them
  - Deciding which processes (or parts of processes) and data to move into and out of memory
  - Allocating and deallocating memory space as needed

### 1.8: Storage Management

- The operating system abstracts from the physical properties of its storage devices to define a logical storage unit, the file.
- File-System Management
  - The operating system is responsible for the following activities in connection with file management:
    - Creating and deleting files
    - Creating and deleting directories to organaize files
    - Supporting primitives for manipulating files and directories
    - Mapping files onto secondary storage
    - Backing up files on stable (nonvolatile) storage media
- Mass-Storage Management
  - The operating system is responsible for the following activities in connection with disk management:
    - Free-space management
    - Storage allocation
    - Disk scheduling
  - The entire speed of operation of a computer may hinge on the speeds of the disk subsystem and the algorithms that manipulate that subsystem.
- Caching
  - Because caches have limited size, cache management is an important design problem.
  - Data transfer from cache to CPU and registers is usually a hardware function, with no operating-system intervention. In contrast, transfer of data from disk to memory is usually controlled by the operating system.
  - Since the various CPUs can all execute in parallel, we must make sure that an update to the value of A in one cache is immediately reflected in all other caches where A resides. This situation is called cache coherency, and it is usually a hardware issue.
- I/O Systems
  - One of the purposes of an operating system is to hide the peculiarities of specific hardware devices from the user.
  - The I/O subsystem consists of several components:
    - A memory-management component that includes buffering, caching, and spooling
    - A general device-driver interface
    - Drivers for specific hardware devices

### 1.9: Protection and Security

- If a computer system has multiple users and allows the concurrent execution of multiple processes, then access to data must be regulated.
- Protection, then, is any mechanism for controlling the access of processes or users to the resources defined by a computer system.
- Consider a user whose authentication information is stolen. Her data could be copied or deleted, even though file and memory protection are working. It is the job of security to defend a system from external and internal attacks.
- Protection and security require the system to be able to distinguish among all its users. Most operating systems maintain a list of user names and associated user identifiers (user IDs).

### 1.10: Kernel Data Structures

- In this section, we briefly describe several fundamental data structures used extensively in operating systems.
- Lists, Stacks, and Queues
  - An array is a simple data structure in which each element can be accessed directly.
  - Whereas each item in an array can be accessed directly, the items in a list must be accessed in a particular order.
    - The most common method for implementing this structure is a linked list, in which items are linked to one another.
  - A stack is a sequentially ordered data structure that uses the last in, first out (LIFO) principle for adding and removing items, meaning that the last item placed onto a stack is the first item removed.
  - A queue, in contrast, is a sequentially ordered data structure that uses the first in, first out (FIFO) principle.
- Trees
  - A tree is a data structure that can be used to represent data hierarchically. Data values in a tree structure are linked through parent-child relationships.
  - In a general tree, a parent may have an unilimited number of children.
  - In a binary tree, a parent may have at most two children, which we term the left child and the right child.
- Hash Functions and Maps
  - A hash function takes data as its input, performs a numeric operation on this data, and returns a numeric value. This numeric value can then be used as an index into a table (typically an array) to quickly retrieve the data.
  - One use of a hash function is to implement a hash map, which associates (or maps) [key:value] pairs using a hash function.
- Bitmaps
  - A bitmap is a string of n binary digits that can be used to represent the status of n items.
  - Thus, bitmaps are commonly used when there is a need to represent the availability of a large number of resources.

### 1.11: Computing Environments

- Mobile Computing
  - Mobile computing refers to computing on handheld smartphones and tablet computers.
- Distributed Systems
  - A distributed system is a collection of physically separate, possibly heterogeneous, computer systems that are networked to provide users with access to the various resources that the system maintains.
  - The protocols that create a distributed system can greatly affect that sysstem's utility and popularity.
  - A network operating system is an operating system that provides features such as file sharing across the network, along with a communication scheme that allows different processes on different computers to exchange messages.
- Client-Server Computing
  - Many of today's systems act as server systems to satisfy requests generated by client systems.
    - このような特殊な distributed system を client-server system という
  - Server systems can be broadly categorized as compute servers and file servers:
    - The compute-server system provides an interface to which a client can send a request to perform an action. In response, the server executes the action and sends the results to the client.
    - The file-server system provides a file-system interface where clients can create, update, read, and delete files.
      - An example of such a system is a web server that delivers files to clients running web browsers.
- Peer-to-Peer Computing
  - Another structure for a distributed system is the peer-to-peer (P2P) system model.
  - In this model, clients and servers are not distinguished from one another. Instead, all nodes whithin the system are considered peers, and each may act s either a client or a server, depending on whether it is requesting or providing a service.
  - In a client-server system, the server is a bottleneck; but in a peer-to-peer system, services can be provided by several nodes distributed throughout the network.
- Virtualization
  - Virtualization is a technology that allows operating systems to run as applications within other operating systems.
  - Broadly speaking, virtualization is one member of a class of software that also includes emulation.
- Cloud Computing
  - Cloud computing is a type of computing that delivers computing, storage, and even applications as a service across a network.
  - There are actually many types of cloud computing, including the following:
    - Public cloud / Private cloud / SaaS / PaaS / IaaS
- Real-Time Embedded Systems
  - Embeded systems almost always run real-time operating systems.
  - A real-time system has well-defined, fixed time constraints. Processing must be done within the defined constraints, or the system will fail.

### 1.12: Open-Source Operating Systems

- Open-source operating systems are those available in source-code format rather than as compiled binary code.
- To counter the move to limit software use and redistribution, Richard Stallman in 1983 started the GNU project to create a free, open-source, UNIX-compatible operating system.
- Linux
  - The GNU project produced many UNIX-compatible tools, including compilers, editors, and utilities, but never released a kernel.
  - In 1991, a student in Finland, Linus Torvalds, released a rudimentary UNIX-like kernel using the GNU compiiers and tools and invited contributions worldwide.
- BSD
  - It started in 1978 as a derivative of AT&T's UNIX.
  - BSD UNIX's development was slowed by a lawsuit by AT&T', but eventually a fully functional, open-source version, 4.4BSD-lite, was released in 1994.
  - Darwin, the core kernel component of Mac OS X, is based on BSD UNIX and is open-sourced as well.
- Solaris
  - Solaris is the commercial UNIX-based operating system of Sun Microsystems.

### 1.13: Summary

- これまでに書いている内容なので省略

## 2: System Structures

- Chapter Objectives
  - To describe the services an operating system provides to users, processes, and other systems.
  - To discuss the various ways of structuring an operating system.
  - To explain how operating systems are installed and customized and how they boot.

### 2.1: Operating-System Services

- An operating system provides an environment for the execution of programs.
- One set of operating system services provides functions that are helpful to the user.
  - User interface.
    - command-line interface (CLI) / batch interface / graphical user interface (GUI)
  - Program execution.
  - I/O operations.
  - File-system manipulation.
  - Communications.
    - There are many circumstances in which one process needs to exchange information with another process.
    - Communications may be implemented via shared memory, in which two or more processes read and write to a shared section of memory, or message passing, in which packets of information in predefined formats are moved between processes by the operating system.
  - Error detection.
  - Resource allocation.
  - Accounting.
    - We want to keep track of which users use how much and what kinds of computer resources. This record keeping may be used for accounting (so that users can be billed) or simply for accumulating usage satistics.
  - Protection and security.

### 2.2: User and Operating-System Interface

- Command Interpreters
  - On systems with multiple command interpreters to choose from, the interpreters are known as shells. For example, on UNIX and Linux systems, a user may choose among several different shells, including the Bourne shell, C shell, Bourne-Again shell, Korn shell, and others.
  - The main function of the command interpreters is to get and execute the next user-specified command. Many of the commands given at this level manipulate files: create, delete, list, print, copy, execute, and so on.
  - These commands can be implemented in two general ways.
    - In one approach, the command interpreter itself contains the code to execute the command.
    - As alternative approach implements most commands through system programs. In this way, programmers can add new commands to the system easily by creating new files with the proper names.
- Graphical User Interfaces
  - A second strategy for interfacing with the operating system is through a user-friendly graphical interface, or GUI.
- Choice of Interface
  - The choice of whether to use a command-line or GUI interface is mostly one of personal preference. System administrators who manage computers and power users who have deep knowledge of a system frequently use the command-line interface.
  - In contrast, most Windows users are happy to use Windows GUI environment and almost never use the MS-DOS shell interface.

### 2.3: System Calls

- System calls provide an interface to the services made available by an operating system. These calls are generally available as routines written in C and C++, although certain low-level tasks may have to be written using assembly-language instructions.
- Typically, application developers design programs according to an application programming interface (API).
  - Three of the most common APIs available to application programmers are the Windows API for Windows systems, the POSIX API for POSIX-based systems (which include virtually all versions of UNIX, Linux, and Mac OS X), and the Java API for programs that run on the Java virtual machine. A programmer accesses an API via a library of code provided by the operating system.
- Why would an application programmer prefer programming according to an API rather than invoking actual system calls? There are several reasons for doing so.
  - One benefit concerns program portability. An application programmer designing a program using an API can expect her program to compile and run on any system that supports the same API.
  - Furthermore, actual system calls can often be more detailed and difficult to work with than the API available to an application programmer.
- For most programming languages, the run-time support system provides a system-call interface that serves as the link to system calls made available by the operating system.
- Three general methods are used to pass parameters to the operating system.
  - The simplest approach is to pass the parameters in registers. だがレジスタの数よりパラメータの数が多い場合がある
  - The parameters are generally stored in block, or table, in memory, and the address of the block is passed as a parameter in a register. This is the approach taken by Linux and Solaris.
  - Parameters also can be placed, or pushed, onto the stack by the program and popped off the stack by the operating system.

### 2.4: Types of System Calls

- System calls can be grouped roughly into six major categories:
  - process control / file manipulation / device manipulation / information maintenance / communications / protection
- Process Control
  - end, abort
  - load, execute
  - create process, terminate process
  - get process attributes, set process attributes
  - wait for time
  - wait event, signal event
  - allocate and free memory
- File Management
  - create file, delete file
  - open, close
  - read, write, reposition
  - get file attributes, set file attributes
- Device Management
  - request device, release device
  - read, write, reposition
  - get device attributes, set device attributes
  - logically attach or detach devices
- Information Maintenance
  - get time or date, set time or date
  - get system data, set system data
  - get process, file, or device attributes
  - set process, file, or device attributes
- Communication
  - create, delete communication connection
  - send, receive messages
  - transfer status information
  - attach or detach remote devices
- Protection
  - get permission, set permission
  - allow user, deny user

### 2.5: System Programs

- At the lowest level is hardware. Next is the operating system, then the system programs, and finally the application programs.
- System programs, also known as system utilities, provide a convenient environment for program development and execution.
- They can be divided into these categories:
  - File management.
  - Status information.
    - Some programs simply ask the system for the date, time, amount of available memory or disk space, number of users, or similar status information.
  - File modification.
  - Programming-language support.
    - Compilers, assemblers, debuggers, and interpreters for common programming languages are often provided with the operating system or available as a separate download.
  - Program loading and execution
  - Communications.
    - These programs provide the mechanism for creating virtual connections among processes, users, and computer systems.
  - Background services.
    - Constantly running system-program processes are known as services, subsystems, or daemons.
    - Along with system programs, most operating system are supplied with programs that are useful in solving common problems include Web browsers, word processors and text formatters, spreadsheets, database systems, compilers, plotting and statical-analysis packages, and games.

### 2.6: Operating-System Design and Implementation

- Design Goals
  - The first problem in designing a system is to define goals and specifications.
  - The requirements can, however, be divided into two basic groups: user goals and system goals.
    - Users want certain obvious properties in a system. The system should convenient to use, easy to learn and to use, reliable, safe, and fast. There is no general agreement on how to achieve them.
    - A similar set of requirements can be defined by those people who must design, create, maintain, and operate the system. The system should be easy to design, implement, and maintain; and it should be flexible, reliable, error free, and efficient. でこれらもあいまいなもの。
  - Although no text book can tell you how to do it, general principles have been developed in the field of software engineering, and we turn now to a discussion of some of these principles.
- Mechanisms and Policies
  - One important principle is the separation of policy from mechanism. Mechanisms determine how to do something; policies determine what will be done.
  - The separation of policy and mechanism is important for flexibility. Policies are likely to change across places or over time. A general mechanism insensitive to changes in policy would be more desirable.
- Implementation
  - Although some operating systems are still written in assembly language, most are written in a higher-level language such as C or an even higher-level language such as C++. Actually, an operating system can be written in more than one language.
  - The advantages of using a high-level language, or at least a systems-implementation language, for implementing operating systems are the same as those gained when the language is used for application programs:
    - The code can be written faster, is more compact, and is easier to understand and debug.
    - In addition, improvements in compiler technology will improve the generated code for the entire operating system by simple recompilation.
    - Finally, an operating system is far easier to port - to move to some other hardware - if it is written in a higher-level language.

### 2.7: Operating-System Structure

- A system as large and complex as a modern operating system must be engineered carefully if it is to function properly and be modified easily. A common approach is to partition the task into small components, or modules, rather than have one monolithic system.
- In this section, we discuss how these components are interconnected and melded into a kernel.
- Simple Structure
  - Frequently, such systems started as small, simple, and limited systems and then grow beyond theire original scope.
  - This monolithic structure was difficult to implement and maintain. It had a distinct advantage, however; there is very little overhead in the system call interface or in communication within the kernel.
  - We still see evidence of this simple, monolithic structure in the UNIX, Linux, and Windows operating systems.
- Layered Approach
  - With proper hardware support, operating systems can be broken into pieces that are smaller and more appropriate than those allowed by the original MS-DOS and UNIX systems.
  - A system an be made modular in many ways. One method is the layered approach, in which the operating system is broken into a number of layers (levels). The bottom layer (layer 0) is the hardware; the highest (leyer N) is the user interface.
  - A typical operating-system layer - say, layer M - consists of data structures and a set of routines that can be invoked by higher-level layers. Layer M, in turn, can invoke operations on lower-level layers.
  - The main advantage of the layered approach is simplicity of construction and debugging.
  - Each layer is implemented only with operations provided by lower-level layers. A layer does not need to know how to these operations are implemented; it needs to know only what these operations do. Hence, each layer hides the existence of certain data structures, operations, and hardware from higher-level layers.
  - The major difficulty with the layered approach involves appropriately defining the various layers.
  - A final problem with layered implementation is that they tend to be less efficient than other types.
- Microkernels
  - In the mid-1980s, researchers at Carnegie Mellon University developed an operating system called Mach that modularized the kernel using the microkernel approach.
  - This method structures the operating system by removing all nonessencial components from the kernel and implementing them as system and user-level programs.
  - The main function of the microkernel is to provide communication between the client program and the various services that are also running in user space.
  - One benefit of the microkernel approach is that it makes extending the operating system easier. All new services are added to user space and consequently do not require modification of the kernel.
  - The microkernel also provides more security and reliability, since most services are running as user - rather than kernel - processes.
  - Unfortunately, the performance of microkernel can suffer due to increased system-function overhead.
- Modules
  - Perhaps the best current methodology for operating-system design involves using loadable kernel modules. Here, the kernel has a set of core components and links in additional services via modules, either at boot time or during run time. This type of design is common in modern implementation of UNIX, such as Solaris, Linux, and Mac OX X, as well as Windows.
  - The idea of the design is for the kernel to provide core services while other services are implemented dynamically, as the kernel is running. Linking service dynamically is preferable to adding new features directly to the kernel, which would require recompiling the kernel every time a change was made.
  - layered system と似ているが、どのモジュールも他のモジュールを呼べるので、より flexible。microkernel とも似ているが、message passing を使わなくても communicate できるので、より efficient。
- Hybrid Systems
  - In practice, very few operating systems adopt a single, strictly defined structure. Instead, they combine different structures, resulting in hybrid systems that address performance, security, and usability issues.
- Mac OS X
  - It is a layered system.
  - The top layers include the Aqua user interface and a set of application environments and services. Notably, the Cocoa environment specifies an API for the Objective-C programming language, which is used for writing Mac OS X applications.
  - Below these layers is the kernel environment, which consists primarily of the Mach microkernel and the BSD UNIX kernel.
- iOS
  - iOS is a mobile operating system designed by Apple to run its smartphone, the iPhone, as well as its tablet computer, the iPad.
- Android
  - The Android operating system was designed by the Open Handset Alliance(led rapid primarily by Google) and was developed for Android smartphones and tablet computers.
  - Whereas iOS is designed to run on Apple mobile devices and is close-sourced, Android runs on a variety of mobile platforms and is open-sourced, partly explaining its rapid rise in popularity.

### 2.8: Operating-System Debugging

- Broadly, debugging is the activity of finding and fixing errors in a system, both in hardware and in software.
- Failure Analysis
  - If a process fails, most operating systems write the error information to a log file to alert system operators or users that the problem occured.
  - The operating system can also take a core dump - a capture of the memory of the process - and store it in a file for later analysis. (Memory was referred to as the "core" in the early days of computing.)
  - A failure in the kernel is called a crash. When a crash occurs, error information is saved to a log file, and the memory state is saved to a crush dump.
  - Operating-system debugging and process debugging frequently use different tools and techniques due to the very different nature of these two tasks.
- Performance Tuning
  - We mentioned earlier that performance tuning seeks to improve performance by removing processing bottlenecks. To identify bottlenecks, we must be able to monitor system performance.
  - In a number of systems, the operating system does this by producing trace listings of system behavior. All interesting events are logged with their time and important parameters and are written to a file.
- DTrace
  - DTrace is a facility that dynamically adds probes to a running system, both in user processes and in the kernel.
  - DTrace is composed of a compiler, a framework, providers of probes written within that framework, and consumers of those probes.

### 2.9: Operating-System Generation

- More commonly, however, operating systems are designed to run on any of a class of machines at a variety of sites with a variety of peripheral configurations. The system must then be configured or generated for each specific computer site, a process sometimes known as system generation (SYSGEN).
- The following kinds of information must be determined.
  - What CPU is to be used?
  - How will the boot disk be formatted?
  - How much memory is available?
  - What devices are available?
- Once this information is determined, it can be used in several ways.
  - At one extreme, a system administrator can use it to modify a copy of the source code of the operating system.
  - At a slightly less tailored level, the system description can lead to the creation of tables and the selection of modules from a precompiled library.
  - At the other extreme, it is possible to construct a system that is completely table driven.
- The major differences among these approaches are the size and generality of the generated system and the ease of modifying it as the hardware configuration changes.

### 2.10: System Boot

- The procedure of starting a computer by loading the kernel is known as booting the system. On most computer systems, a small piece of code known as the bootstrap program or bootstrap loader locates the kernel, loads it into main memory, and starts its execution.
- The bootstrap program can perform a variety of tasks. Usually, one task is to run diagnostics to determine the state of the machine. If the diagnostics pass, the program can continue with the booting steps. It can also initialize all aspects of the system, from CPU registers to device controllers and the contents of main memory. Sooner or later, it starts the operating system.
- The program stored in the boot block may be sophisticated enough to load the entire operating system into memory and begin its execution.

### 2.11: Summary

- これまでに書いている内容なので省略

## 3: Process Concept

- Chapter Objectives
  - To introduce the notion of a process - a program in execution, which forms the basis of all computation.
  - To describe the various features of processes, including scheduling, creation, and termination.
  - To explore interprocess communication using shared memory and message passing.
  - To describe communication in client-server systems.

### 3.1: Process Concept

- この本では job と process はほぼ同じ意味だが、process という言葉を使う
- The Process
  - a process is a program in execution.
  - A process is more than a program code.
    - text section(program counter やレジスタの中身), data section, heap, and stack も含む
  - a program by itself is not a process.
    - a program is a passive entity.
    - a process is a active entity.
      - 実行状態などを持つので、同じプログラムから生成されるプロセスでも、すべて別々のものとなる
  - a program becomes a process when an executable file is loaded into memory.
- Process State
  - state は以下のどれか
    - New / Running / Waiting / Ready / Terminated
    - state の名前は任意で OS による
  - 各 processor で、Running 状態にある process は、どの瞬間でも 1 つだけ
- Process Control Block (PCB)
  - Each process is represented in the operating systems by a process control block - also called a task control block.
    - PCB は以下の情報を含む
      - Process state / Program counter / CPU registers / CPU-scheduling information / Memory-management information / Accounting information / I/Ostatus information
  - In brief, the PCB simply serves as the repository for any information that may vary from process to process.
- Threads
  - This single thread of control allows the process to perform only one task at a time.
  - Most modern operating systems have extended the process concept to allow a process to have multiple threads of execution and thus to perform more than one task at a time.

### 3.2: Process Scheduling

- The objective of multiprogramming is to have some process running at all times, to maximize CPU utilization.
- THe objective of time sharing is to switch the CPU among processes so frequently that users can interact with each program while it is running.
- To meet these objectives, the process scheduler selects an available process for program execution on the CPU.
- Scheduling Queues
  - As processes enter the system, they are put into a job queue, which consists of all processes in the system.
  - ready queue
    - The processes that are residing in main memory and are ready and waiting to execute are kept on a list called the ready queue.
  - device queue
    - The list of processes waiting for a particular I/O device is called a device queue.
- Schedulers
  - The long-term scheduler, or job scheduler, selects processes from this pool and loads them into memory for execution.
  - THe short-term scheduler, or CPU scheduler, selects from among the processes that are ready to execute and allocates the CPU to one of them.
  - In general, most processes can be described as either I/O bound or CPU bound.
    - I/O-bound process
      - An I/O-bound process is one that spends more of its time doing I/O than it spends doing computation.
    - CPU-bound process
      - A CPU-bound process, in contrast, generates I/O requests infrequently, using more of its time doing computation.
    - I/O bound ばかりだと reqdy queue が空いてしまうし、CPU-bound ばかりだと I/O waiting queue が空いてしまうので、バランス良く選択することが大事
- Context Switch
  - Switching the CPU to another process requires performing a state save of the current process and a state restore of a different process. This task is known as a context switch.

### 3.3: Operations on Processes

- In this section, we explore the mechanisms involved in creating processes and illustrate process creation on UNIX and Windows systems.
- Process Creation
  - Most operating systems identify processes according to a unique process identifier (or pid), which is typically an integer number.
  - UNIX の場合、fork() の system call で新しい process が作成される。fork() が実行された後、親プロセスも子プロセスも実行状態にあり、1 つのコードから 2 つのプロセスが作成される。親プロセスは wait() を実行して、子プロセスの完了を待ち、子プロセスは exec() した後 exit() して親プロセスに処理を戻す
- Process Termination
  - A process terminates when it finishes executing its final statement and asks the operating system to delete it by using the exit() system call.
  - At that point, the process may return a status value to its parent process. All the resources of the process - including physical and virtual memory, open files, and I/O buffers - are deallocated by the operating system.
  - cascading termination
    - Some systems do not allow a child to exist if its parent has terminated. In such systems, if a process terminates, then all its children must also be terminated. This phenomenon, reffered to as cascading termination, is normally initiated by the operating system.
  - zombie process
    - A process that has terminated, but whose parent has not yet called wait(), is known as a zombie process.
    - 一般的には親プロセスが wait() を call したタイミングで zombie process も解放される
    - もしその後も wait() が呼ばれなければ、child processes は orphans となる
      - Linux や UNIX では、orphans processes の新しい親プロセスとして、init process をアサインする。init process は定期的に wait() を実行し、orphaned process を解放する

### 3.4: Interprocess Communication

- Processes executing concurrently in the operating system may be either independent processes or cooperating processes.
- A process is independent if it cannot affect or be affected by the other processes executing in the system. Any process that does not share data with any other process is independent.
- A process is cooperating if it can affect or be affected by the other process executing in the system. Clearly, any process that shares data with other processes is a cooperating process.
- There are several reasons for providing an environment that allows process cooperation:
  - Information sharing / Computations speedup / Modularity / Convenience
- interprocess communication (IPC)
  - Cooperating processes require an interprocess communication mechanism that will allow them to exchange data and information.
- There are two fundamental models of interprocess communication: shared memory and message passing
  - Both of the models just mentioned are common in operating systems, and many systems implement both.
  - Message passing is also easier to implement in a distributed system than shared memory.
  - Shared memory can be faster than message passing, since message-passing systems are typically implemented using system calls and thus require the more time-consuming task of kernel intervention.
- Shared-Memory Systems
  - producer-consumer problem
    - サイズが有限の 1 個のバッファーと 2 種類のスレッドが存在するとする。一方のスレッドを生産者、もう一方のスレッドを消費者と呼ぶ。生産者はバッファーに空きができるまでデータを入れることができない。消費者は生産者がバッファーに何か書き込むまで、このバッファーからデータを取り出すことができない
    - One solution to the producer-consumer problem uses shared memory. To allow producer and consumer processes to run concurrently, we must have available a buffer of items that can be filled by the producer and emptied by the consumer.
    - Two types of buffers can be used.
      - The unbounded buffer places no practical limit on the size of the buffer.
      - The bounded buffer assumes a fixed buffer size. In this case, the consumer must wait if the buffer is empty, and the producer must wait if the buffer is full.
- Message-Passing Systems
  - It is particularly useful in a distributed environment, where the communicating processes may reside on different computers connected by a network.
  - Naming
    - Processes that want to communicate must have a way to refer to each other. They can use either direct or indirect communication.
      - Under direct communication, each process that want to communicate must explicitly name the recipient or sender of the communication.
      - With indirect communication, the messages are sent to and received from mailboxes, or ports.
  - Synchronization
    - Massage passing may be either blocking or nonblocking - also known as synchronous and asynchronous.
    - Blocking send
      - The sending process is blocked until the message is received by the receiving process or by the mailbox.
    - Nonblocking send
      - The sending process sends the message and resumes operation.
    - Blocking receive
      - The receiver blocks until a message is available.
    - Nonblocking receive
      - The receiver retrieves either a valid message or a null.
    - Different combinations of send() and receive() are possible.
  - Buffering
    - Whether communication is direct or indirect, messages exchanged by communicating processes reside in a temporary queue. Basically, such queues can be implemented in three ways:
      - Zero capacity / Bounded capacity / Unbounded capacity

### 3.5: Examples of IPC Systems

- In this section, we explore three different IPC systems.
  - POSIX Shared Memory / Mach / Windows
- An Example: POSIX Shared Memory
  - POSIX shared memory is organized using memory-mapped files, which associate the region of shared memory with a file.
  - A process must first create a shared-memory object using the shm_open() system call.
  - Once the object is established, the ftruncate() function is used to configure the size of the object in bytes.
  - Finally, the mmap() function establishes a memory-mapped file containing the shared-memory object.
- An Example: Mach
  - The Mach kernel supports the creation and destruction of multiple tasks, which are similar to processes but have multiple threads of control and fewer associated resources.
  - Most communication in Mach - including all intertask information - is carried out by messages. Messages are sent to and received from mailboxes, called ports in Mach.
- An Example: Windows
  - The message-passing facility in Windows is called the advanced local procedure call (ALPC) facility. It is used for communication between two processes on the same machine.
  - Windows uses two types of ports:
    - connection ports and communication ports
  - When an ALPC channel is created, one of three message-passing techniques is chosen:
    - For small messages (up to 256 bytes), the port's message queue is used as intermediate storage, and the messages are copied from one process to the other.
    - Larger messages must be passed through a section object, which is a region of shared memory associated with the channel.
    - When the amount of data is too large to fit into a section object, an API is available that allows server processes to read and write directly into the address space of a client.
  - It is important to note that the ALPC facility in Windows is not part of the Windows API and hence is not visible to the application programmer.
    - アプリケーションは Windows API を経由して、間接的に ALPC を利用する

### 3.6: Communication in Client-Server Systems

- 3.4 で述べたような shared memory や message passing の方法は client-server systems の communication でも利用できる。ここではそれ以外のやり方 sockets / remote procedure calls (RPCs) / pipes について述べる
- Sockets
  - A socket is defined as an endpoint for communication. A pair of processes communicating over a network employs a pair of sockets - one for each process.
  - A socket is identified by an IP address concatenated with a port number. In general, sockets use a client-server architecture.
    - The server waits for incoming client requests by listening to a specified port.
    - Once a request is received, the server accepts a connection from the client socket to complete the connection.
    - Servers implementing specific services listen to well-known ports (a telnet server listens to port 23; an FTP server listens to port 21; and a web, or HTTP, server listens to port 80).
    - All ports below 1024 are considered well known; we can use them to implement standard services.
  - loopback
    - The IP address 127.0.0.1 is a special IP address known as the loopback. When a computer refers to IP address 127.0.0.1, it is referring to itself.
- Remote Procedure Calls
  - The RPC was designed as a way to abstract the procedure-call mechanism for use between systems with network connections. It is similar in many respects to the IPC mechanism. Here, however, because we are dealing with an environment in which the processes are executing on separate systems, we must use a message-based communication scheme to provide remote service.
  - The RPC system hides the details that allow communication to take place by providing a stub on the client side.
    - On Windows systems, stub code is compiled from a specification written in the Microsoft Interface Definition Language (MIDL), which is used for defining the interfaces between client and server programs.
  - One issue that must be dealt with concerns differences in data representation on the client and server machines.
  - 他にも call が失敗することもあるので、"exactly once"や"at most once"の保証を考える必要がある
  - client が server の port を知るためのやり方は主に 2 つ
    - First, the binding information may be predetermined, in the form of fixed port addresses.
    - Second, binding can be done dynamically by a rendezvous (also called a matchmaker) daemon on a fixed RPC port.
      - randezvous daemon に request して（こっちは fixed）、該当の port の情報を得る
- Pipes
  - A pipe acts as a conduit allowing two processes to communicate.
  - In implementing a pipe, four issues must be considered:
    - Does the pipe allow bidirectional communication, or is communication unidirectional?
    - If two-way communication is allowed, is it half duplex (data can travel only one way at a time) or full duplex (data can travel in both directions at the same time)?
    - Must a relationship (such as parent-child) exist between the communicating processes?
    - Can the pipes communicate over a network, or must the communicating processes reside on the same machine?
  - Ordinary Pipes
    - Ordinary pipes allow two processes to communicate in standard producer-consumer fashion: the producer writes to one end of the pipe (the write-end) and the consumer reads from the other end (the read-end).
    - As a result, ordinary pipes are unidirectional, allowing only one-way communication. If two-way communication is required, two pipes must be used, with each pipe sending data in different direction.
    - Note that ordinary pipes require a parent-child relationship between the communicating processes on both UNIX and Windows systems. This means that these pipes can be used only for communication between processes on the same machine.
  - Named Pipes
    - Ordinary pipes exist only while the processes are communicatingwith one another. On both UNIX and Windows systems, once the processes have finished communicating and have terminated, the ordinary pipes ceases to exist.
    - Named pipes provide a much more powerful communication tool. Communication can be bidirectional, and no parent-child relationship is required. Once a named pipe is established, several processes can use it for communication.
    - Additionally, named pipes continue to exist after communicating processes have finished.
    - UNIX では half-duplex で processes must reside on the same machine、Windows systems では full-duplex で processes may reside on either the same machine or different machines。

### 3.7: Summary

- これまでに書いている内容なので省略

## 4: Multithreaded Programming

- Chapter Objectives
  - To introduce the notion of a thread - a fundamental unit of CPU utilization that forms the basis of multithreaded computer systems.
  - To discuss the APIs for the Pthreads, Windows, and Java thread libraries.
  - To explore several strategies that provide implicit threading.
  - To examine issues related to multithreaded programming.
  - To cover operating system support for threads in Windows and Linux.

### 4.1: Overview

- A thread is a basic unit of CPU utilization; it comprise a thread ID, a program counter, a register set, and a stack.
- It shares with other threads belonging to the same process its code section, data section, and other operating-system resources, such as open files and signals.
- A traditional process has a single thread of control. If a process has multiple threads of control, it can perform more than one task at a time.
- Motivation
  - Most software applications that run on modern computers are multithreaded. An application typically is implemented as a separate process with several threads of control.
  - A web browser might have one thread display images or text while another thread retrieves data from the network, for example.
  - 似たような task を実行するなら、新しく process を作るより、thread を新しく作ったほうが効率がいい。
  - Typically, RPC servers are multithreaded. When a server receives a message, it services the message using a separate thread.
- Benefits
  - The benefits of multithreaded programming can be broken down into four major categories:
    - Responsiveness.
      - Multithreading an interactive application may allow a program to continue running even if part of it is blocked or is performing lengthy operation, thereby increasing responsiveness to the user.
    - Resource sharing
      - Processes can only share resources through techniques such as shared memory and message passing. Such techniques must be explicitly arranged by the programmer. However, threads share the memory and the resources of the process to which they belong by default.
    - Economy.
      - Allocating memory and resources for process creation is costly. Because threads share the resources of the process to which they belong, it is more economical to create and context-switch threads.
    - Scalability.
      - The benefits of multithreading can be even greater in a multiprocessor architecture, where threads may be running in parallel on different processing core. A single-threaded process can run on only one processor, regardless how many are available.

### 4.2: Multicore Programming

- Multithreaded programming provides a mechanism for more efficient use of these multiple computing cores and improved concurrency.
- Notice the distinction between parallelism and concurrency in this discussion.
  - A system is parallel if it can perform more than one task simultaneously.
  - In contrast, a concurrent system supports more than one task by allowing all the tasks to make progress.
    - 高速にスイッチすることですべてのタスクを進めることは running concurrently だが、parallel ではない
- Programming Challenges
  - In general, five areas present challenges in programming for multicore systems:
    - Identifying tasks.
      - This involves examining applications to find areas that can be divided into separate, concurrent tasks.
    - Balance.
      - When identifying tasks that can run in parallel, programmers must also ensure that the tasks perform equal work of equal value.
    - Data splitting.
      - Just as applications are divided into separate tasks, the data accessed and manipulated by the tasks must be divided to run on separate cores.
    - Data dependency.
      - When one task depends on data from another, programmers must ensure that the execution of the tasks is synchronized to accommodate the data dependency.
    - Testing and debugging.
      - When a program is running in parallel on multiple cores, many different execution paths are possible. Testing and debugging such concurrent programs is inherently more difficult than testing and debugging single-threaded applications.
- Types of Parallelism
  - In general, there are two types of parallelism:
    - data parallelism
    - task parallelism
  - Data parallelism focuses on distributing subsets of the same data across multiple computing cores and performing the same operation on each core.
  - Task parallelism involves distributing not data but tasks (threads) across multiple computing cores.
  - In most instances, applications use a hybrid of these two strategies.

### 4.3: Multithreading Models

- Support for threads may be provided either at the user level, for user threads, or by the kernel, for kernel threads.
- User threads are supported above the kernel and are managed without kernel support, whereas kernel threads are supported and managed directly by the operating system.
- Ultimately, a relatioship must exist between user threads and kernel threads. In this section, we look at three common ways of establishing such a relationship: the many-to-one model, the one-to-one-model, and the many-to-many model.
- Many-to-One Model
  - The many-to-one model maps many user-level threads to one kernel threads.
  - Because only one thread can access the kernel at the time, multiple threads are unable to run in parallel on multicore systems.
  - Very few systems continue to use the model because of its inability to take advantage of multiple processing cores.
- One-to-One Model
  - The one-to-one model maps each user thread to a kernel thread.
  - It provides more concurrency than the many-to-one model by allowing another thread to run when a thread makes a blocking system call.
  - The only drawback to this model is that creating a user thread requires creating the corresponding kernel thread.
  - Linux, along with the family of Windows operating systems, implement the one-to-one model.
- Many-to-Many Model
  - The many-to-many model multiplexes many user-level threads to a smaller or equal number of kernel threads.
  - これは複数の threads を同時に実行できるし、たくさんの kernel threads を作らなければならないわけでもない。

### 4.4: Thread Libraries

- A thread library provides the programmer with an API for creating and managing threads.
- There are two primary ways of implementing a thread library.
  - The first approach is to provide a library entirely in user space with no kernel support.
  - The second approach is to implement a kernel-level library supported directly by the operaging system.
- Two general strategies for creating multiple threads:
  - asynchronous threading
    - With asynchronous threading, once the parent creates a child thread, the parent resumes its execution, so that the parent and child execute concurrently.
  - synchronous threading
    - Synchronous threading occurs when the parent thread creates one or more children and then must wait for all of its children to terminate before it resumes - the so-called fork-join strategy.
    - Typically, synchronous threading involves significant data sharing among threads.
- Pthreads
  - Pthreads refers to the POSIX standard defining an API for thread creation and synchronization. This is a specification for thread behavior, not an implementation. Operating-system designers may implement the specification in any way they wish.
- Windows Threads
  - The technique for creating threads using the Windows thread library is similar to the Pthreads technique in several ways.
- Java Threads
  - Threads are the fundamental model of program execution in a Java program, and the Java language and its API provide a rich set of features for the creation and management of threads.

### 4.5: Implicit Threading

- 4.2 であげたような challenges 以外にも、multicore threading が進化する中で、いくつか困難がある。One way to address these difficulties and better support the design of multithreaded applications is to transfer the creation and management of threading from application developers to compilers and run-time libraries.
- This strategy, termed implicit threading, is a popular trend today. In this section, we explore three alternative approaches for designing multithreaded programs that can take advantage of multicore processors through implicit threading.
- Thread Pools
  - Whereas creating a separate thread is certainly superior to creating the separate process, a multithreaded server nonetheless has potential problems.
    - The first issue concerns the amount of time required to create the thread, together with the fact that the thread will be discarded once it has completed its work.
    - The second issue is more troublesome. If we allow all concurrent requests to be serviced in a new thread, we have not placed a bound on the number of threads concurrently active in the system.
      - One solution to this problem is to use a thread pool.
  - The general idea behind a thread pool is to create a number of threads at process startup and place them into a pool, where they sit and wait for work.
  - When a server receives a request, it awakens a thread from this pool - if one is available - and passes it the request for service. Once the thread completes its service, it returns to the pool and awaits more work. If the pool contains no available thread, the server waits until one becomes free.
  - Thread pool offer these benefit:
    - Servicing a request with an existing thread is faster than waiting to create a thread.
    - A thread pool limits the number of threads that exist at any one point. This is particularly important on systems that cannot support a large number of concurrent threads.
    - Separating the task to be performed from the mechanics of creating the task allows us to use different strategies for running the task. For example, the task could be scheduled to execute after a time delay or to execute periodically.
- OpenMP
  - OpenMP is a set of compiler directives as well as an API for programs written in C, C++, or FORTRAN that provides support for parallel programming in shared-memory environments.
  - OpenMP identifies parallel regions as blocks of code that may run in parallel.
  - In addition to providing directives for parallelization, OpenMP allows developers to choose among several levels of parallelism.
    - データを share するか、private にするかなど
- Grand Central Dispatch
  - Grand Central Dispatch (GCD) - a terminology for Apple's Mac OS X and iOS operating systems - is a combination of extensions to the C language, an API, and a run-time library that allows application developers to identify sections of code to run in parallel.
  - GCD schedules blocks for run-time execution by placing them on a dispatch queue. When it removes a block from a queue, it assigns the block to an available thread from the thread pool it manages.
  - GCD identifies two types of dispatch queue: serial and concurrent.
    - Blocks placed on a serial queue are removed in FIFO order. Once a block has been removed from the queue, it must complete execution before another block is removed.
    - Blocks placed on a concurrent queue are also removed in FIFO order, but several blocks may be removed at a time, thus allowing multiple blocks to execute in parallel.
      - There are three system-wide concurrent dispatch queues, and they are distinguished according to priority: low, default, and high.

### 4.6: Threading Issues

- The fork() and exec() System Calls
  - Chapter 3 で fork() の system call をした時は、a separate, duplicate process を作成すると書いた。
  - multithreaded program の場合、fork() を call したら、すべての threads をコピーするか、fork() を call した thread だけをコピーするべきだろうか？
  - いくつかの UNIX systems では、両方のバージョンを持っている。
- Signal Handling
  - A signal is used in UNIX systems to notify a process that a particular event has occured.
  - synchronous なものと asynchronous なものがある
  - A signal may be handled by one of two possible handlers:
    - A default signal handler
    - A user-defined signal handler
  - Every signal has a default signal handler that the kernel runs when handling that signal. This default actin can be overridden by a user-defined signal handler that is called to handle the signal.
  - Delivering signals is more complicated in multithreaded programs. Where should a signal be delivered? In general, the following options exist:
    - Deliver the signal to the thread to which the signal applies.
    - Deliver the signal to every thread in the process.
    - Deliver the signal to certain threads in the process.
    - Assign a specific thread to receive all signals for the process.
  - The standard UNIX function for delivering a signal is `kill(pid_t pid, int signal)`. This function specifies the process (pid) to which a particular signal (signal) is to be delivered.
  - Although Windows does not explicitly provide support for signals, it allows us to emulate them using asynchronous procedure calls (APCs).
- Thread Cancellation
  - Thread cancellation involves terminating a thread before it has completed.
  - A thread that is to be canceled is often referred to as the target thread.
  - Cancellation of a target thread may occur in two different scenarios.
    - Asynchronous cancellation. One thread immediately terminates the target thread.
    - Deferred cancellation. The target thread periodically checks whether it should terminate, allowing it an opportunity to terminate itself in an orderly fashion.
  - The difficulty with cancellation occurs in situations where resources have been allocated to a canceled thread or where a thread is canceled while in the midst of updating data it is sharing with other threads.
- Thread-Local Storage
  - Threads belonging to a process share the data of the process. However, in some circumstances, each thread might need its own copy of certain data.
    - We will call such data thread-local storage (or TLS).
  - Most thread libraries - including Windows and Pthreads - provide some form of support for thread-local storage; Java provides support as well.
- Scheduler Activations
  - A final issue to be considered with multithreaded programs concerns communication between the kernel and the thread library, which required by the many-to-many and two-level models discussed in Section 4.3.3.
    - two-level model は、 many-to-many model だけじゃなく 1:1 対応も認める model。
  - Many systems implementing either the many-to-many or the two-level model place an intermediate data structure between the user and kernel threads. This data structure is typically known as a lightweight process, or LWP.
  - One scheme for communication between the user-thread library and the kernel is known as scheduler activation.

### 4.7: Operating-System Examples

- Windows Threads
  - A Windows application runs as a separate process, and each process may contain one or more threads.
  - Windows uses the one-to-one mapping, where each uesr-level thread maps to an associated kernel thread.
  - The general components of a thread include:
    - A thread ID uniquely identifying of the thread
    - A register set representing the status of the processor
    - A user stack, employed when the thread is running in user mode, and a kernel stack, employed when the thread is running in kernel mode
    - A private storage area used by various run-time libraries and dynamic link libraries (DLLs)
  - The register set, stacks, and private storage area are known as the context of the thread.
- Linux Threads
  - Linux also provides the ability to create threads using the clone() system call.
  - Linux does not distinguish between processes and threads. In fact, Linux uses the term task - rather than process or thread - when referring to a flow of control within a program.
  - When clone() is invoked, it is passed a set of flags that determine how much sharing is to take place between the parent and child tasks.
  - When folk() is invoked, a new task is created, along with a copy of all the associated data structures of the parent process.

### 4.8: Summary

- これまでに書いている内容なので省略

## 5: Process Scheduling

- CPU scheduling is the basis of multiprogrammed operating systems. By switching the CPU among processes, the operating system can make the computer more productive.
- Chapter Objectives
  - To introduce CPU-scheduling, which is the basis for multiprogrammed operating systems.
  - To describe various CPU-scheduling algorithms.
  - To discuss evaluation criteria for selecting a CPU-scheduling algorithm for a particular system.
  - To examine the scheduling algorithms of several operating systems.

### 5.1: Basic Concepts

- CPU-I/O Burst Cycle
  - The success of CPU scheduling depends on an observed property of processes: process execution consists of a cycle of CPU execution and I/O wait. Processes alternate between these two states.
  - Process execution begins with a CPU burst. That is followed by an I/O burst, which is followed by another CPU burst, then another I/O burst, and so on.
- CPU Scheduler
  - Whenever the CPU becomes idle, the operating system must select one of the processes in the ready queue to be executed. The selection process is carried out by the short-term scheduler, or CPU scheduler.
  - Note that the ready queue is not necessarily a first-in, first-out queue.
- Preemptive Scheduling
  - CPU-scheduling decisions may take place under the following four circumstances:
    1. When a process switches from the running state to the waiting state
    2. When a process switches from the running state to the ready state
    3. When a process switches from the waiting state to the ready state
    4. When a process terminates
  - When scheduling takes place only under circumstances 1 and 4, we say that the scheduling scheme is nonpreemptive or cooperative. Otherwise, it is preemptive.
  - Under nonpreemptive scheduling, once the CPU has been allocated to a process, the process keeps the CPU until it releases the CPU either by terminating or by switching to the waiting state.
  - Cooperative scheduling is the only method that can be used on certain hardware platforms, because it does not require the special hardware needed for preemptive scheduling.
  - Unfortunately, preemptive scheduling can result in race conditions when data are shared among several processes.
  - Because interrupts can, by definition, occur at any time, and because they cannot always be ignored by the kernel, the sections of code affected by interrupts must be guarded from simultaneous use.
- Dispatcher
  - This function involves the following:
    - Switching context
    - Switching to user mode
    - Jumping to the proper location in the user program to restart that program
  - The time it takes for the dispatcher to stop one process and start another running is known as the dispatch latency.

### 5.2: Scheduling Criteria

- Which characteristics are used for comparison can make a substantial difference in which algorithm is judged to be best.
- The criteria include the following:
  - CPU utilization.
  - Throughput.
  - Turnaround time.
    - The interval from the time of submission of a process to the time of completion is the turnaround time. Turnaround time is the sum of the periods spent waiting to get into memory, waiting in the ready queue, executing on the CPU, and doing I/O.
  - Waiting time.
    - Waiting time is the sum of the periods spent waiting in the ready queue.
  - Response time.
    - Time from the submission of a request until the first response is produced. response の output までの時間ではなく、responding を始めるまでの時間なので注意。

### 5.3: Scheduling Algorithms

- First-Come, First-Served (FCFS) Scheduling
  - With this scheme, the process that requests the CPU first is allocated the CPU first.
  - The code for FCFS scheduling is simple to write and understand.
  - On the negative side, the average waiting time under the FCFS policy is often quite long.
  - There is a convoy effect as all the other processes wait for the one big process to get off the CPU.
    - This effect results in lower CPU and device utilization than might be possible if the shorter processes were allowed to go first.
  - Note also that the FCFS scheduling algorithm is nonpreemptive. Once the CPU has been allocated to a process, that process keeps the CPU until it releases the CPU, either by terminating or by requesting I/O.
- Shortest-Job-First (SJF) Scheduling
  - When the CPU is available, it is assigned to the process that has the smallest next CPU burst. トータルの job の長さではなく、CPU burst の長さで判定しているので注意。
  - The SJF scheduling algorithm is provably optimal, in that it gives the minimum average waiting time for a given set of processes.
  - The real difficulty with the SJF algorithm is knowing the length of the next CPU request.
  - SJF scheduling is used frequently in long-term scheduling.
  - Although the SJF algorithm is optimal, it cannot be implemented at the level of short-term CPU scheduling.
    - With short-term scheduling, there is no way to know the length of the next CPU burst.
  - The next CPU burst is generally predicted as an exponential average of the measured lengths of previous CPU bursts.
  - The SJF algorithm can be either preemptive or nonpreemptive.
    - The choice arises when a new process arrives at the ready queue while a previous process is still executing.
  - Preemptive SJF scheduling is sometimes called shortest-remainig-time-first scheduling.
- Priority Scheduling
  - The SJF algorithm is a special case of the general priority-scheduling algorithm. A priority is associated with each process, and the CPU is allocated to the process with the highest priority.
  - Priority can be defined either internally or externally.
  - Priority scheduling can be either preemptive or nonpreemptive.
  - A major problem with priority scheduling algorithms is indefinite blocking, or starvation.
    - 優先度の低い process が waiting 状態でブロックされているのに、無限に実行されない問題
    - A solution to the problem of indefinite blockage of low-priority processes is aging. Aging involves gradually increasing the priority of processes that wait in the system for long time.
- Round-Robin Scheduling
  - THe round-robin (RR) scheduling algorithm is designed especially for time-sharing systems. It is similar to FCFS scheduling, but preemption is added to enable the system to switch between processes.
  - A small unit of time, called time quantum or time slice, is defined. A time quantum is generally from 10 to 100 milliseconds in length.
  - The CPU scheduler goes around the ready queue, allocating the CPU to each process for a time interval of up to 1 time quantum.
  - The average waiting time under the RR policy is often long.
  - The performance of the RR algorithm depends heavily on the size of the time quantum.
    - Although the time quantum should be large compared with the context-switch time, it should not be too large. too large だと FCFS と同じになってしまう。
    - A rule of thumb is that 80 percent of the CPU bursts should be shorter than the time quantum.
- Multilevel Queue Scheduling
  - A multilevel queue scheduling algorithm partitions the ready queue into several separate queues. The processes are permanently assigned to one queue, generally based on some property of the process, such as memory size, process priority, or process type.
  - Let's look at an example of a multilevel queue scheduling algorithm with five queues, listed below in order of priority:
    - System processes
    - Interactive processes
    - Interactive editing processes
    - Batch processes
    - Student processes
  - batch queue を実行するには上位の 3 つがすべてない状態でないといけないし、もし実行中により優先順位の高いものが来たら batch queue は中断されて、そちらを先に実行する
  - Another possibility is to time-slice among the queues. Here, each queue gets a certain portion of the CPU time, which it can then schedule among its various processes.
    - こちらだと、ある queue に 80% / ある queue に 20% という感じで、優先度の低い queue にもある割合の時間が割かれる
- Multilevel Feedback Queue Scheduling
  - The multilevel feedback queue scheduling algorithm, in contrast, allows a process to move between queues.
  - If a process uses too much CPU time, it will be moved to a lower-priority queue. In addition, a process that waits too long in a lower-priority queue may be moved to a high-priority queue.
  - In general, a multilevel feedback queue scheduler is defined by the following parameters:
    - The number of queues
    - The scheduling algorithm for each queue
    - The method used to determine when to upgrade a process to a higher-priority queue
    - The method used to determine when to demote a process to a lower-priority queue
    - The method used to determine which queue a process will enter when that process needs service

### 5.4: Thread Scheduling

- In this section, we explore scheduling issues involving user-level and kernel-level threads and offer specific examples of scheduling for Pthreads.
- Contention Scope
  - On systems implementing the many-to-one and many-to-many models, the thread library schedules user-level threads to run on an available LWP. This scheme is known as process-contention scope (PCS), since competition for the CPU takes place among threads belonging to the same process.
  - To decide which kernel-level thread to schedule onto a CPU, the kernel uses system-contention scope (SCS). Competition for the CPU with SCS scheduling takes place among al threads in the system.
  - Typically, PCS is done according to priority - the scheduler selects the runnable thread with the highest priority to run. User-level thread priorities are set by the programmer and are not adjusted by the thread library.
- Pthread Scheduling
  - Pthreads identifies the following contention scope values:
    - PTHREAD_SCOPE_PROCESS schedules threads using PCS scheduling
    - PTHREAD_SCOPE_SYSTEMS schedules threads using SCS scheduling
  - Note that on some systems, only certain contention scope values are allowed. For example, Linux and Mac OS X systems allow only PTHREAD_SCOPE_SYSTEMS

### 5.5: Multiple-Processor Scheduling

- If multiple CPUs are available, load sharing becomes possible - but scheduling problems become correspondingly more complex.
- Here, we discuss several concerns in multiprocessor scheduling.
- Approaches to Multiple-Processor Scheduling
  - One approach to CPU scheduling in a multiprocessor system has all scheduling decisions, I/O processing, and other system activities handled by a single processor - the master server.
    - This asymmetric multiprocessing is simple because only one processor accesses the system data structures, reducing the need for data sharing.
  - A second approach uses symmetric multiprocessing (SMP), where each processor is self-scheduling.
    - All processes may be in a common ready queue, or each processor may have its own private queue of ready processes.
    - Regardless, scheduling proceeds by having the scheduler for each processor examine the ready queue and select a process to execute.
    - Virtually all modern operating systems support SMP, including Windows, Linux, and Mac OS X. In the remainder of this section, we discuss issues concerning SMP systems.
- Processor Affinity
  - Because of the high cost of invalidating and repopulating caches, most SMP systems try to avoid migraion of processes from one processor to another and instead attempt to keep a process running on the same processor.
    - This is known as processor affinity - that is, a process has an affinity for the processor on which it is currently running.
    - ある processor でキャッシュされていた process を別の processor で実行しようとすると、キャッシュを破棄し、再度作り直す必要があるのでコストが高い。
  - Processor affinity takes several forms.
    - When an operating system has a policy of attempting to keep a process running on the same processor - but not guaranteeing that it will do so - we have a situation known as soft affinity.
    - In contrast, some system provide system calls that support hard affinity, thereby allowing a process to specify a subset of processors on which it may run.
  - Maany systems provide both soft and hard affinity.
- Load Balancing
  - Load balancing attempts to keep the workload evenly distributed across all processors in an SMP system.
  - It is important to note that load balancing is typically necessary only on systemss where each processor has its own private queue of eligible processes to execute.
    - common queue しかない場合は必要ない。
  - There are two general approaches to load balancing: push migration and pull migration.
    - With push migration, a specific task periodically checks the load on each processor and - if it finds an imbalance - evenly distributes the load by moving (or pushing) processes from overloaded to idle or less-busy processors.
    - Pull migration occurs when an idle processor pulls a waiting task from a busy processor.
  - Push and pull migration need not be mutually exclusive and are in fact often implemented in parallel on load-balancing systems.
- Multicore Processors
  - SMP systems that use multicore processors are faster and consume less power than systems in which each processor has its own physical chip.
  - Researchers have discovered that when a processor accesses memory, it spends a significant amount of time waiting for the data to become available. This situation, known as a memory stall, may occur for various reasons, such as a cache miss.
    - To remedy this situation, many recent hardware designs have implemented multithreaded processor cores in which two (or more) hardware threads are assigned to each core.
  - In general, there are two ways to multithread a processing core: coarse-grained and fine-grained multithreading.
    - With coarse-grained multithreading, a thread executes on a processor until a long-latency event such as a memory stall occurs.
    - Fine-grained (or interleaved) multithreading switches between threads at a much finer level of granularity - typically at the boundary of an instruction cycle.
  - Notice that a multithreaded multicore processor actually requires two different levels of scheduling.
    - 1 つはどの software thread をどの hardware thread (logical processor) で実行するかということ
    - もう 1 つは各 core でどの thread を実行するか決めること

### 5.6: Real-Time CPU Scheduling

- In general, we can distinguish between soft real-time systems and hard real-time systems.
  - Soft real-time systems provide no guarantee as to when a critical real-time process will be scheduled.
  - Hard real-time systems have stricter requirements. A task must be serviced by its deadline; service after deadline has expired is the same as no service at all.
- Minimizing Latency
  - We refer to event latency as the amount of time that elapses from when an event occurs to when it is serviced.
  - Two types of latencies affect the performance of real-time systems:
    - Interrupt latency
      - Interrupt latency refers to the period of time from the arrival of an interrupt at the CPU to the start of the routine that services the interrupt.
    - Dispatch latency
      - The amount of time required for the scheduling dispatcher to stop one process and start another is known as dispatch latency.
      - The most effective technique for keeping dispatch latency low is to provide preemptive kernels.
      - The conflict phase of dispatch latency has two components:
        - Preemption of any process running in the kernel
        - Release by low-priority processes of resources needed by a high-priority process
- Priority-Based Scheduling
  - If the scheduler also supports preemption, a process currently running on the CPU will be preempted if a higher-priority process becomes available to run.
- Rate-Monotonic Scheduler
  - The rate-monotonic scheduling algorithm schedules periodic tasks using a static priority policy with preemption.
  - The shorter the period, the higher the priority; the longer the period, the lower the priority.
    - period は CPU burst の期間ではなく、deadline までの期間
  - Despite being optimal, then, rate-monotonic scheduling has a limitation:
    - CPU utilization is bounded, and it is not always possible fully to maximize CPU resources.
- Earliest-Deadline-First Scheduling
  - Earliest-deadline-first (EDF) scheduling dynamically assigns priorities according to deadline.
  - Unlike the rate-monotonic algorithm, EDF scheduling does not require that processes be periodic, nor must a process require a constant amount of CPU time per burst.
  - 理論的には CPU 使用率 100% を実現できるが、context switching のコストがあるため、現実的には 100% とはならない
- Proportional Share Scheduling
  - 各 application の重みに比例して、リソースを配分するやり方
  - Proportional share schedulers must work in conjunction with an admission-control policy to guarantee that an application receives its allocated shares of time.
  - An admission-control policy will admit a client requesting a particular number of shares only if sufficient shares are available.
- POSIX Real-Time Scheduling
  - POSIX defines two scheduling classes for real-time threads:
    - SCHED_FIFO
    - SCHED_RR
    - SCHED_OTHER
      - 実装は定義されていない、各 system による

### 5.7: Operating-System Examples

- Example: Linux Scheduling
  - Scheduling in the Linux system is based on scheduling classes. Each class is assigned a specific priority.
  - Standard Linux kernels implement two scheduling classes:
    - a default scheduling class using the CFS (Completely Fair Scheduler) scheduling algorithm
    - a real-time scheduling class
- Example: Windows Scheduling
  - Windows schedules threads using a priority-based, preemptive scheduling algorithm.
  - The portion of the Windows kernel that handles scheduling is called the dispatcher.
  - Priorities are divided into two classes.
    - The variable class contains threads having priorities from 1 to 15, and the real-time class contains threads which priorities ranging from 16 to 31.
  - Windows7 introduced user-mode scheduling (UMS), which allows applications to create and manage threads independently of the kernel.
- Example: Solaris Scheduling
  - Solaris uses priority-based thread scheduling.
  - By default, there is an inverse relationship between priorities and time slices.

### 5.8: Algorithm Evaluation

- Criteria are often defined in terms of CPU utilization, response time, or throughput.
- Deterministic Modeling
  - One major class of evaluation methods is analytic evaluation. Analytic evaluation uses the given algorithm and the system workload to produce a formula or number to evaluate the performance of the algorithm for that workload.
  - Deterministic modeling is one type of analytic evaluation. This method takes a particular predetermined workload and defines the performance of each algorithm for that workload.
  - Deterministic modeling is simple and fast. It gives us exact numbers, allowing us to compare the algorithms. However, it requires exact numbers for input, and its answers apply only to those cases.
  - The main use of deterministic modeling are in describing scheduling algorithms and providing examples.
- Queueing Models
  - On many systems, the processes that are run vary from day to day, so there is no static set of processes (or times) to use for deterministic modeling. What can be determined, however, is the distribution of CPU and I/O bursts.
  - Similarly, we can describe the distribution of times when processes arrive in the system (the arrival-time distribution).
  - From these two distribution, it is possible to compute the average throughput, utilization, waiting time, and so on for most algorithms.
  - 予測の数字は正確なものではないので、Queing models are often only approximations of real systems, and the accuracy of the computed results may be questionable.
- Simulations
  - To get a more accurate evaluation of scheduling algorithms, we can use simulations. Running simulations invoves programming a model of the computer system.
  - The data to drive the simulation can be generated in several ways. The most common method uses a random-number generator that is programmed to generate processes, CPU burst times, arrivals, departures, and so on, according to probability distribution.
- Implementation
  - Even a simulation is of limited accuracy. The only completely accurate way to evaluate a scheduling algorithms is to code it up, put it in the operating system, and see how it works.
  - The major difficulty with this approach is the high cost.
  - Another difficulty is that the environment in which the algorithm is used will change.

### 5.9: Summary

- これまでに書いている内容なので省略

## 6: Synchronization

- A cooperating process is one that can affect or be affected by other processes executing in the system. Cooperating processes can either directly share a logical address space (that is, both code and data) or be allowed to share data only through files or messages.
- In this chapter, we discuss various mechanisms to ensure the orderly execution of cooperating processes that share a logical address space, so that data consistency is maintained.
- Chapter Objectives
  - To introduce the critical-section problem, whose solutions can be used to ensure the consistency of shared data.
  - To present both software and hardware solutions of the critical-section problem.
  - To examine several classical process-synchronization problems.
  - To explore several tools that are used to solve process synchronization problems.

### 6.1: Background

- A situation, where several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the access takes place, is called a race condition.
- To guard against the race condition above, we need to ensure that only one process at a time can be manipulating the variable counter. To make such a guarantee, we require that the processes be synchronized in some way.
- We devote a major portion of this chapter to process synchronization and coordination among cooperating processes.

### 6.2: The Critical-Section Problem

- Consider a system of consisting of n processes {P0, P1, ..., Pn-1}. Each process has a segment of code, called a critical section, in which the process may be changing common variables, updating a table, writing a file, and so on.
- The important feature of the system is that, when one process is executing in its critical section, no other process is allowed to execute in its critical section.
- The critical-section problem is to design a protocol that the processes can use to cooperate.
- Each process must request permission to enter its critical section. The section of code implementing this request is the entry section. The remaining code is the remainder section.
- A solution to the critical-section problem must satisfy the following three requirements:
  - Mutual exclusion.
  - Progress.
  - Bounded waiting.
    - There exists a bound, or limit, on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.
- Two general approaches are used to handle critical sections in operating systems: preemptive kernels and nonpreemptive kernels.
  - nonpreemptive の場合は、race conditions の問題は起きない。だが、preemptive kernel のほうが real-time programming に適している。

### 6.3: Peterson's solution

- Next, we illustrate a classic software-based solution to the critical-section problem known as Peterson's solution.
- Peterson's solution では turn と flag の 2 つの変数を使う。
  - The variable turn indicates whose turn it is to enter its critical section. turn == i なら Pi が実行可能。
  - The flag array is used to indicate if a process is ready to enter its critical section. もし flag[i] が true なら Pi is ready to enter its critical section.
- 以下のようになる。Pi が critical section に入ろうとしている。turn = j に設定して、j が critical section に入ろうとしていないか確認する。もし j が入ろうとしていたら、入っている間は何もしない。j が終わったあとで、critical section に入り、出る時に自身の flag を false にする。

```
do {
  flag[i] = true;
  turn = j;
  while (flag[j] && turn ==j); // do nothing

  // critical section

  flag[i] = false;

  // remainder section

} while (true);
```

### 6.4: Synchronization Hardware

- Software-based solutions such as Peterson's are not guaranteed to work on modern computer architectures.
- In the following discussions, we explore several more solutions to the critical-section problem using techniques ranging from hardware to software-based APIs available to both kernel developers and application programmers.
- All these solutions are based on the premise of locking - that is, protecting critical regions through the use of locks.
- test_and_set と compare_and_swap のやり方を以下に記載する。

```
// 定義
boolean test_and_set(boolean *target) {
  boolean rv = *target;
  *target = true;

  return rv;
}

// 実行
do {
  while (test_and_set(&lock)); // do nothing

  // critical section

  lock = false;

  // remainder section

} while (true)
```

```
// 定義
boolean compare_and_swap(int *value, int expected, int new_value) {
  int temp = *value;
  if (*value == expected) {
    *value = new_value;
  }

  return temp;
}

// 実行
do {
  while (compare_and_swap(&lock, 0, 1) != 0); // do nothing

  // critical section

  lock = 0;

  // remainder section

} while (true)
```

### 6.5: Mutex Locks

- The term mutex is short for mutual exclusion.
- A mutex lpock has a boolean variable available whose value indicates if the lock is available or not.
- The acquire() function acquires the lock, and the release() function releases the lock.

```
acquire() {
  while (!available); // busy wait
  available = false;
}

do {
  // acquire lock

  // critical section

  // release lock

  // remainder section

} while (true)
```

- The main disadvantage of the implementation given here is that it requires busy waiting.
  - While a process is in its critical section, any other process that tries to enter its critical section must loop continuously in the call to acquire().
- In fact, this type of mutex lock is also called a spinlock because the process "spins" while waiting for the lock to become available.
- Busy waiting wastes CPU cycles that some other process might be able to use productively.
- Spinlocks do have an advantage, however, in that no context switch is required when a process must wait on a lock, and a context switch may take considerable time.
  - Thus, when locks are expected to be held for short times, spinlocks are useful.

### 6.6: Semaphores

- In this section, we examine a more robust tool that can behave similarly to a mutex lock but can also provide more sophisticated ways for processes to synchronize their activities.
- A semaphore S is an integer variable that, apart from initialization, is accessed only through two standard atomic operations: wait() and signal().
- Semaphore Usage
  - Operating systems often distinguish between counting and binary semaphore.
    - The value of a counting semaphore can range over an unrestricted domain.
    - The value of a binary semaphore can range only between 0 and 1.
- Semaphore Implementation
  - To overcome the need for busy waiting, we can modify the definition of the wait() and signal() operations as follows:
    - When a process executes the wait() operation and finds that the semaphore value is not positive, it must wait. However, rather than engaging in busy waiting, the process can block itself. The state of the process is switched to the waiting state.
- We define a semaphore as follows:

```
typedef struct {
  int value;
  struct process *list;
} semaphore
```

- Deadlocks and Starvation
  - The implementation of a semaphore with a waiting queue may result in a situation where two or more processes are waiting indefinitely for an event that can be caused only by one of the waiting process. When such a state is reached, these processes are said to be deadlocked.
  - Another problem related to deadlocks is indefinite blocking or starvation, a situation in which processes wait indefinitely within the semaphore.
- Priority Inversion
  - higher-priority process が lower-priority process に掴まれている data にアクセスしたい、という問題が起こる場合がある
    - L, M, H の 3 つの processes があって、L < M < H の優先度とする
    - H が resource R にアクセスしたい時、L がその前に R を握っているとする
    - 通常は H が L の処理が終わるのを待つが、この時、M が実行可能になったとすると、L は M に実行を譲ってしまう
    - そうすると、H は L が R を離すまで待たなければならない
  - This problem is known as priority inversion. It occurs only in systems with more than two priorities, so one solution is to have only two priorities.
  - Typically, these systems solve the problem by implementing a priority-inheritance protocol.
    - According to this protocol, all processes that are accessing resources needed by a higher-priority process inherit the higher priority until they are finished with the resources in question.
    - When they are finished, theire priorities revert to their original values.

### 6.7: Classic Problems of Synchronization

- The Bounded-Buffer Problem
  - 6.1 で紹介されたやつ
- The Readers-Writers Problem
  - database の値を only to read したい process が readers、to update したい process が writer。
  - If a writer and some other process (either a reader or writer) access the database simultaneously, chaos may ensue.
- The Dining-Philosophers Problem
  - 5 人の哲学者が円形に座っていて、各自の両側にお箸が 1 本ずつある。両方の箸を取れたら料理を食べることができるが、片方しか取れなかった場合は食べられない、というやつ

### 6.8: Monitors

- Although semaphores provide a convenient and effective mechanism for process synchronization, using them incorrectly can result in timing errors that are difficult to detect, since these errors happen only if particular execution sequences take place and these sequences do not always occur.
- programmer が以下のようなミスをすると、相互排他が壊されたり、deadlock が起きたりする
  - wait() と signal() の順番を逆にする
  - signal() を wait() にしてしまう
  - wait() を抜かす / signal() を抜かす
- To deal with such errors, researchers have developed high-level language constructs. In this section, we describe one fundamental high-level synchronization construct - the monitor type.
- Monitor Usage
  - An abstract data type - or ADT - encapsulates data with a set of functions to operate on that data that are independent of any specific implementation of the ADT.
  - A monitor type is an ADT that includes a set of programmer-defined operations that are provided with mutual exclusion within the monitor.
  - The monitor construct ensures that only one process at a time is active within the monitor.
    - Consequently, the programmer does not need to code this synchronization constraint explicitly.
  - monitor construct だけでは synchronization scheme を作るのに不十分なので、condition construct を追加する。
    - The only operations that can be invoked on a condition variable are wait() and signal().
  - Many programming languages have incorporated the idea of the monitor as described in this section, including Java and C#.
- Dining-Philosophers Solution Using Monitors
  - `enum {THINKING, HUNGRY, EATING} state[5]` を定義して解く。5 人のうち、i 番目の哲学者の state = EATING の時、state[(i + 4) % 5] != EATING かつ state[(i + 1) % 5] != EATING となる
  - また `condition self[5]` も定義する
- Implementing a Monitor Using Semaphores
  - semaphore mutex と next を定義して解く
- Resuming Processes within a Monitor
  - 複数の process が待機状態である時、どれから resume していくか。FCFS (first-come, first-serve) でも良いが、たいていの場合は適していない。
  - For this purpose, the conditional-wait construct can be used.
  - wait を実行する時に、x.wait(c) のような形で実行する。c は priority number と呼ばれ、待機状態の process の中で、最もこの値が小さいものを次の実行対象とする。
  - We must check two conditions to establish the correctness of this system.
    - First, user processes must always make their calls on the monitor in a correct sequence.
    - Second, we must be sure that an that uncooperative process does not simply ignore the mutual-exclusion gateway provided by the monitor and try to access the shared resource directly, without using the access protocols.
  - Only if these two conditions can be ensured can we guarantee that no time-dependent errors will occur and that the scheduling algorithm will not be defeated.

### 6.9: Synchronization Examples

- Synchronization in Windows
  - The Windows operating system is a multithreaded kernel that provides support for real-time applications and multiple processors.
  - For thread synchronization outside the kernel, Windows provides dispatcher objects.
  - Dispatcher objects may be in either a signaled state or a nonsignaled state.
    - An object in a signaled state is available, and a thread will not block when acquiring the object.
    - An object in a nonsignaled state is not available, and a thread will block when attempting to acquire the object.
  - A critical-section object is a user-mode mutex that can often be acquired and released without kernel intervention.
- Synchronization in Linux
  - Linux provides several different mechanisms for synchronization in the kernel.
  - All math operation using atomic integers are performed without interruption.
- Synchronization in Solaris
  - To control access to critical sections, Solaris provides adaptive mutex locks, condition variables, semaphores, reader-writer locks, and turnstiles.
  - An adaptive mutex protects access to every critical data item.
  - To optimize Solaris performance, developers have refined and fine-tuned the locking methods. Because locks are used frequently and typically are used for crucial kernel functions, tuning their implemention and use can produce great performance gains.
- Pthreads Synchronization
  - The Pthreads API is available for programmers at the user level and is not part of any particular kernel.
  - There are other extensions to the Pthreads API - including spinlocks - but it is important to note that not all extensions are considered portable from one implementation to another.

### 6.10: Alternative Approaches

- Transactional Memory
  - Quite often in computer science, ideas from one area of study can be used to solve problems in other areas. The concept of transactional memory originated in database theory, for example, yet it prvides a strategy for process synchronization.
  - A memory transaction is a sequence of memory read-write operations that are atomic.
  - If all operations in a transaction are completed, the memory transaction is committed. Otherwise, the operations must be aborted and rolled back.
  - The advantage of using such a mechanism rather than lock is that the transactional memory system - not the developer - is responsible for guaranteeing atomicity.
  - Transactional memory can be implemented in either software or hardware.
    - Software transactional memory (STM), as the name suggests, implements transactional memory exclusively in software - no special hardware is needed.
    - Hardware transactional memory (HTM) uses hardware cache hierarchies and cache coherency protocols to manage and resolve conflicts involving shared data residing in separate processor's caches.
- OpenMP
  - The advantage of OpenMP is that thread creation and management are handled by the OpenMP library and are not the responsibility of application developers.
  - The critical-section compiler directive behaves much like a binary semaphore or mutex lock, ensuring that only one thread at a time is active in the critical section.
  - An advantage of using the critical-section compiler directive in OpenMP is that it is generally considered easier to use than standard mutex locks.
  - However, a disadvantage is that application developers must still identify possible race conditions and adequately protect shared data using the compiler directive.
- Functional Programming Language
  - Most well-known programming languages - such as C, C++, Java, and C# - are known as imperative (or procedual) languages. Imperative languages are used for implementing algorithms that are state-based.
  - With the current emphasis on concurrent and parallel programming for muticore systems, there has been greater focus on functional programming languages, which follow a programming paradigm much different from that offered by imperative languages.
    - The fundamental difference from between imperative and functional languages is that functional languages do not maintain state.

### 6.11: Summary

- これまでに書いている内容なので省略

## 7: Deadlocks

- Sometimes, a waiting process is never again able to change state, because the resources it has requested are held by other waiting processesl. This situation is called a deadlock.
- Chapter Objectives
  - To develop a description of deadlocks, which prevent sets of concurrent processes from completing their tasks.
  - To present a number of different methods for preventing or avoiding deadlocks in a coputer system.

### 7.1: System Model

- Under the normal mode of operation, a process may utilize a resource in only the following resource:
  - Request.
  - Use.
  - Release.
- A set of processes is in a deadloked state when every process in the set is waiting for an event that can be caused only by another process in the set. The events with which we are mainly concerned here are resource acquisition and release.

### 7.2: Deadlock Characterization

- Before we discuss the various methods for dealing with the deadlock problem, we look more closely at features that characterize deadlocks.
- Necessary Conditions
  - A deadlock situation can arise if the following four conditions hold simultaneously in a system:
    - Mutual exclusion.
    - Hold and wait.
    - No preemption.
    - Circular wait.
- Resource-Allocation Graph
  - Deadlocks can be described more precisely in terms of a directed graph called a system resource-allocation graph.
  - resource-allocation graph で循環があれば deadlock が存在する可能性があり（存在しない場合もある）、なければどの process も deadlock になっていない。こちらは必ず存在しない。

### 7.3: Methods for Handling Deadlocks

- Generally speaking, we can deal with the deadlock problem in one of three ways:
  - We can use a protocol to prevent or avoid deadlocks, ensuring that the system will never enter a deadlocked state.
    - Deadlock Prevention
  - We can allow the system to enter a deadlocked state, detect it, and recover.
  - We can ignore the problem altogether and pretend that deadlocks never occur in the system.

### 7.4: Deadlock Prevention

- By ensuring that at least one of these conditions cannot hold, we can prevent the occurrence of a deadlock. We elaborate on this approach by examining each of the four necessary conditions separately.
- Mutual Exclusion
  - Sharable resources, in contrast, do not require mutually exclusive access and thus cannot be involved in a deadlock.
  - In general, however, we cannot prevent deadlocks by denying the mutual-exclusion condition, because some resources are intrinsically nonsharable.
- Hold and Wait
  - To ensure that the hold-and-wait condition never occurs in the systems, we must guarantee that, whenever a process requests a resource, it does not hold any other resources.
  - One protocol that we can use requires each process to request and be allocated all its resources before it begins execution.
  - An alternative protocol allows a process to request resources only when it has none.
  - Both these protocols have two main disadvantages.
    - First, resource utilization may be low, since resources may be allocated but unused for a long period.
    - Second, starvation is possible.
- No Preemption
  - To ensure that this condition does not hold, we can use the following protocol.
    - If a process is holding some resources and requests another resource that cannot be immediately allocated to it (that is, the process must wait), then all resources the process is currently holding are preempted.
    - Alternatively, if a process requests some resources, we first check whether they are available.
  - This protocol is often applied to resources whose state can be easily saved and restored later, such as CPU registers and memory space. It cannot be applied to such resources as mutex locks and semaphores.
- Circular Wait
  - One way to ensure that this condition never holds is to impose a total ordering of all resource types and to require that each process requests resources in an increasing order of enumeration.
    - リソースに数字を割り振り、あるプロセスが最初にアクセスしたリソースの数字が i だとすると、その次にアクセスできるリソースは i より大きい数字に対応したリソースのみ
  - Alternatively, we can require that a process requesting an instance of resource type Rj must have released any resources Ri such that F(Ri) >= F(Rj).
  - If these two protocols are used, then the circular-wait condition cannot hold.
  - Keep in mind that developing an ordering, or hierarchy, does not in itself prevent deadlock.
  - It is also important to note that imposing a lock ordering does not guarantee deadlock prevention if locks can be acquired dynamically.

### 7.5: Deadlock Avoidance

- Possible side effects of preventing deadlocks by this method, however, are low device utilization and reduced system throughput.
- An alternative method for avoiding deadlocks is to require additional information about how resources are to be requested.
- The simplest and most useful model requires that each process declare the maximum number of resources of each type that it may need.
- Safe State
  - A state is safe if the system can allocate resources to each process in some order and still avoid a deadlock.
  - Most formally, a system is in a safe state only if there exists a safe sequence.
  - Given the concept of a safe state, we can define avoidance algorithms that ensure that the system will never deadlock.
  - The idea is simply to ensure that the system will always remain in a safe state.
- Resource-Allocation Graph Algorithm
  - 7.2.2 で resource-allocation graph を扱った
  - In addition to the request and assignment edges already described, we introduce a new type of edge, called a claim edge.
    - A claim edge Pi → Rj indicates that process Pi may request resource Rj at some time in the future.
    - If no cycle exists, then the allocation of the resource will leave the system in a safe state.
- Banker's Algorithm
  - The resource-allocation-graph algorithm is not applicable to a resource-allocation system with multiple instances of each resource type.
  - banker's algorithm は各 resource type に複数のインスタンスがあっても適用できる。しかし、resource-allocation graph と比べて少し効率が悪い。
  - When a new process enters the system, it must declare the maximum number of instances of each resource type that it may need. When a user requests a set of resources, the system must determine whether the allocation of these resources will leave the system in a safe state. If it will, the resources are allocated; otherwise, the process must wait until some other process releases enough resources.

### 7.6: Deadlock Detection

- If a system does not employ either a deadlock-prevention or deadlock-avoidance algorithm, then a deadlock situation may occur. In this environment, the system may provide:
  - An algorithm that examines the state of the system to determine whether a dead lock has occured
  - An algorithm to recover from the deadlock
- Single Instance of Each Resource Type
  - If all resources have only a single instance, then we can define a deadlock-detection algorithm that uses a variant of the resource-allocation graph, called a wait-for graph.
  - We obtain this graph from resource-allocation graph by removing the resource nodes and collapsing the appropriate edge.
  - To detect deadlocks, the system needs to maintain the wait-for graph and periodically invoke an algorithm that searches for a cycle in the graph.
- Several Instances of a Resource Type
  - wait-for graph では複数インスタンスのタイプに適用できないので、ここでは複数インスタンスに適用できるアルゴリズムを考える。
- Detection-Algorithm Usage
  - When should we invoke the detection algorithm? The answer depends on two factors:
    - How often is a deadlock likely to occur?
    - How many processes will be affected by deadlock when it happens?
  - If deadlocks occur frequently, then the detection algorithm should be invoked frequently.

### 7.7: Recovery from Deadlock

- When a detection algorithm determines that a deadlock exists, several alternatives are available.
  - One possibility is to inform the operator that a deadlock has occurred and to let the operator deal with the deadlock manually.
  - Another possibility is to let the system recover from the deadlock automatically.
- There are two options for breaking a deadlock.
  - One is simply to abort one or more processes to break the circular wait.
  - The other is to preempt some resources from one or more of the deadlocked processes.
- Process Termination
  - To eliminate deadlocks by aborting a process, we use one of two methods.
    - Abort all deadlocked processes.
      - This method cleary will break the deadlocke cycle, but at great expense.
    - Abort one process at a time until the deadlock cycle is eliminated.
      - This method incurs considerable overhead, since after each process is aborted, a deadlock-detection algorithm must be invoked to determine whether any processes are stil deadlocked.
  - pertial termination method を実行する場合は、どのプロセスから終了させていくか考えなければいけない。minimum cost を考えるとしても、以下のようなことを考える必要がある
    - What the priority of the process is
    - How long the process has computed and how much longer the process will compute before completing its designated task
    - How many and what types of resources the process has used
    - など
- Resource Preemption
  - 3 つ考えることがある
    - Selecting a victim.
    - Rollback.
      - 途中で処理をやめたら We must roll back the process to some safe state and restart it from that state.
      - Since, in general, it is difficult to determine what a safe state is, the simplest solution is a total rollback: abort the process and then restart it.
    - Starvation.
      - How can we guarantee that resources will not always be preempted from the same process?
      - The most common solution is to include the number of rollbacks in the cost factor.

### 7.8: Summary

- これまでに書いている内容なので省略

## 8: Memory-Management Strategies

- The memory-management algorithms vary from a primitive bare-machine approach to paging and segmentation strategies.
- Chapter Objectives
  - To provide a detailed description of various ways of organizing memory hardware.
  - To explore various techniques of allocating memory to processes.
  - To discuss in detail how paging works in contemporary computer systems.

### 8.1: Background

- Basic Hardware
  - Main memory and the registers built into the processor itself are the only general-purpose storage that the CPU can access directly.
  - Therefore, any instructions in execution, and any data being used by the instructions, must be in one of these direct-access storage devices.
  - We first need to make sure that each process has a separate memory space.
  - To separate memory spaces, we need the ability to determine the range of legal addresses that the process may access and to ensure that the process can access only these legal addresses.
    - base register と limit register の 2 つで実現する
    - The base register holds the smallest legal physical memory address; the limit register specifies the size of the range.
      - base register が 300040、limit register が 120900 を持っていたら、300040 から 420939 までアクセス可能
  - Protection of memory space is accomplished by having the CPU hardware compare every address generated in user mode with the registers.
  - The operating system, executing in kernel mode, is given unrestricted access to both operating-system memory and use's memory.
- Address Binding
  - Usually, a program resides on a disk as a binary executable file. To be executed, the program must be brought into memory and placed within a process.
  - The processes on the disk that are waiting to be brought into memory for execution form the input queue.
  - Classically, the binding of instructions and data to memory addresses can be done at any step along the way:
    - Compile time.
      - source program がコンパイルされて object module になる
    - Load time.
      - 上の object modules と other object modules をあわせて linkage editor が load module に変換する
      - system library と load module が loader に渡される
    - Execution time.
      - dynamically loaded system library とあわせて in-memory binary memory image に渡される
- Logical Versus Physical Address Space
  - An address generated by the CPU is commonly referred to as a logical address, whereas an address seen by the memory unit - that is, the one loaded into the memory-address register of the memory - is commonly referred to as physical address.
  - logical address と virtual address は同じ意味で使う
  - The run-time mapping from virtual to physical addresses is done by a hardware device called the memory-management unit (MMU).
    - The base register is now called a relocation register.
  - The user program never sees the real physical addresses.
- Dynamic Loading
  - To obtain better memory-space utilization, we can use dynamic loading. With dynamic loading, a routine is not loaded until it is called.
  - The advantage of dynamic loading is that a routine is loaded only when it is needed.
  - Dynamic loading does not require special support from the operating system. It is the responsibility of the users to design their programs to take advantage of such a method.
- Dynamic Linking and Shared Libraries
  - Dynamically linked libraries are system libraries that are linked to user programs when the programs are run.
  - Some operating systems support only static linking, in which system libraries are treated like any other object module and are combined by the loader into the binary program image.
  - dynamic linking は実行時に動的にリンクされる。
  - With dynamic linking, a stub is included in the image for each library-routine reference. The stub is a small piece of code that indicates how to locate the appropriate memory-resident library routine or how to load the library if the routine is not already present.
  - Unlike dynamic loading, dynamic linking and shared libraries generally require help from the operating system.

### 8.2: Swapping

- A process swapped temporarily out of memory to a backing store and then brought back into memory for continued execution.
- Standard Swapping
  - Standard swapping involves moving processes between main memory and a backing store.
  - The system maintains a ready queue consisting of all processes whose memory images are on the backing store or in memory and are ready to run.
  - Notice that the major part of the swap time is transfer time.
  - Standard swapping is not used in modern operating systems.
    - It requires too much swapping time and provides too little execution time to be a reasonable memory-management solution.
  - 以下のような対応が取られている。
    - In one common variation, swapping is normally disabled but will start if the amount of free memory falls below a threshold amount.
    - Another variation involves swapping portions of processes - rather than entire processes - to decrease swap time.
- Swapping on Mobile Systems
  - Although most operating systems for PCs and servers support some modified version of swapping, mobile systems typically do not support swapping in any form.
  - mobile device は hard disk ではなく flash memory を使っていて、書き込みの回数に制限があるため
  - Instead of using swapping, when free memory falls below a certain threashold, Apple's iOS asks applications to voluntarily relinquish allocated memory.

### 8.3: Contiguous Memory Allocation

- The main memory must accommodate both the operating system and the various user processes.
- In contiguous memory allocation, each process is contained in a single section of memory that is contiguous to the section containing the next process.
- Memory Protection
  - We can prevent a process from accessing memory it does not own by combining two ideas previously discussed.
    - If we have a system with a relocation register, together with a limit register, we accomplish our goal.
    - The relocation register contains the value of the smallest physical address; the limit register contains the raange of logical addresses.
  - When the CPU scheduler selects a process for execution, the dispatcher loads the relocation and limit registers with the correct values as part of the context switch.
  - Because every address generated by a CPU is checked against these registers, we can protect both the operating system and the other users' programs and data from being modified by this running process.
- Memory Allocation
  - One of the simplest methods for allocating memory is to divide memory into several fixed-sized partitions.
    - In this multiple-partition method, when a partition is free, a process is selected from the input queue and is loaded into the free partition. When the process terminates, the partition becomes available for another process.
  - In the variable-partition scheme, the operating system keeps a table indicating which parts of memory are available and which are occupied.
    - This procedure is a particular instance of the general dynamic storage-allocation, problem, which concerns how to satisfy a request of size n from a list of free holes.
    - The first-fit, best-fit, and worst-fit strategies are the ones most commonly used to select a free hole from the set of available holes.
      - First fit.
        - Allocate the first hole that is big enough.
      - Best fit.
        - Allocate the smallest hole that is big enough.
      - Worst fit.
        - Allocate the largest hole.
- Fragmentation
  - Both the first-fit and best-fit strategies for memory allocation suffer from external fragmentation.
    - As processes are loaded and removed from memory, the free memory space is broken into little pieces.
    - External fragmentation exists when there is enough total memory space to satisfy a request but the available spaces are not contiguous.
  - Memory fragmentation can be internal as well as external.
  - One solution to the problem of external fragmentation is compaction.
    - The goal is to shuffle the memory contents so as to place all free memory together in one large block. Compaction is not always possible, however.
  - Another possible solution to the external-fragmentation problem is to permit the logical address space of the processes to be noncontiguous, thus allowing a process to be allocated physical memory wherever such memory is available.
    - Two complementary techniques achieve this solution: segmentation and paging.

### 8.4: Segmentation

- Basic Method
  - Each segment has a name and a length. The addresses specify both the segment name and the offset within the segment.
  - A C compiler might create separate segments for the following:
    - The code
    - Global variables
    - The heap, from which memory is allocated
    - The stacks used by each thread
    - The standard C library
- Segmentation Hardware
  - This mapping is effected by a segment table. Each entry in the segment table has a segmenta base and a segment limit.

### 8.5: Paging

- Segmentation permits the physical address space of a process to be noncontiguous. Paging is another memory-management scheme that offers this advantage.
  - However, paging avoids external fragmentation and the need for compaction, whereas segmentation does not.
- Paging is implemented through cooperation between the operating system and the computer hardware.
- Basic Method
  - The basic method for implementing paging involves breaking physical memory into fixed-sized blocks called frames and breaking logical memory into blocks of ghe same size called pages.
  - Every address generated by the CPU is divided into two parts: a page number and a page offset. The page number is used as an index into a page table.
  - The page size (like the frame size) is defined by the hardware.
  - When we use a paging scheme, we have no external fragmentation: any free frame can be allocated to a process that needs it. However, we may have some internal fragmentation.
    - page size を小さくしたほうが internal fragmentation は小さくなるが、page size を大きくしたほうが overhead は小さくなる。
  - Each page of the process needs one frame.
  - operating system は frame table で frame の情報を管理する。
- Hardware Support
  - The hardware implementation of the page table can be done in several ways.
  - In the simplest case, the page table is implemented as a set of dedicated registers.
    - These registers should be built with very high-speed logic to make the paging-address translation efficient.
    - The use of registers for the page table is satisfactory if the page table is reasonably small. Most contemporary computers, however, allow the page table to be very large.
  - The page table is kept in main memory, and a page-table base register (PTBR) points to the page table. Changing page tables requires changing only this one register, substantially reducing context-switch time.
    - The problem with this approach is the time required to access a user memory location.
    - This delay would intolerable under most circumstances.
  - The standard solution to this problem is to use a special, small, fast-lookup hardware cache called a translation look-aside buffer (TLB).
    - The TLB is associative, high-speed memory.
    - Each entry in the TLB consists of two parts: a key (or tag) and a value.
    - Replacement policies range from least recently used (LRU) through round-robin to random.
    - The percentage of times that the page number of interest is found in the TLB is called the hit ratio.
    - 今日の CPU は複数レベルの TLB を持っている。
    - TLBs are a hardware feature and therefore would seem to be of little concern to operating systems and their designers.
- Protection
  - Memory protection in a paged environment is accomplished by protection bits associated with each frame.
  - One bit can define a page to be read-write or read-only.
  - One additional bit is generally attached to each entry in the page table: a valid-invalid bit.
    - When this bit is set to valid, the associated page is in the process's logical address space and is thus a legal (or valid) page.
- Shared Pages
  - An advantage of paging is the possibility of sharing common code.
  - Figure 8.16 が paging environment でコードをシェアしている例

### 8.6: Structure of the Page Table

- Hierarchical Paging
  - 今日では large logical address がサポートされていて、page table 自体もとても大きくなっている。
  - One simple solution to this problem is to divide the page table into smaller pieces. We can accomplish this division in several ways.
  - One way is to use a two-level paging algorithm, in which the page table itself is also paged.
- Hashed Page Table
  - A common approach for handling address spaces larger than 32 bits is to use a hashed page table, with the hash value being the virtual page number.
- Inverted Page Tables
  - An inverted page table has one entry for each real page (or frame) of memory.
  - Although this scheme decreases the amount of memory needed to store each page table, it increases the amount of time needed to search the table when a page reference occurs.
  - Systems that use inverted page tables have difficulty implementing shared memory.
    - 通常は複数の virtual addresses が 1 つの physical address を指すが、inverted page table ではこのやり方が使えないため。
- Oracle SPARC Solaris
  - Consider as a final example of a modern 64-bit CPU and operating system that are tightly integrated to provide low-overhead virtual memory.
  - Solaris running on the SPARC CPU is a fully 64-bit operating system and as such has to solve the problem of virtual memory without using up all of its physical memory by keeping multiple levels of page tables.