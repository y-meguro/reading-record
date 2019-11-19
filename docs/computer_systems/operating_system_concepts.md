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
  - Whereas iOS is designed to run on Apple mobile devices and is close-sourced, Android runs on a vriety of mobile platforms and is open-sourced, partly explaining its rapid rise in popularity.

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
