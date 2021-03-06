# 概要

## 本

- [Operating System Concepts | Abraham Silberschatz, Peter B. Galvin, Greg Gagne](https://amzn.to/2CN0bst)
  - 参考
    - [Operating System Concepts - 9th edition](https://www.os-book.com/OS9/index.html)
  - 最新はおそらく第 10 版
    - [Operating System Concepts 10e Wiley E-Text Student Package | Abraham Silberschatz](https://amzn.to/2qUswe8)
  - 日本語版は第 7 版が最新
    - [オペレーティングシステムの概念 | Abraham Silberschatz, Peter Baer Galvin, Greg Gagne, 土居 範久, 大谷 真, 加藤 和彦, 光来 健一, 清水 謙多郎, 高田 眞吾, 高田 広章, 千葉 滋, 野口 健一郎](https://amzn.to/37Z0SNt)

## かかった時間

- 116.3 時間

## 読む前の状態

- [作って理解するOS](create_and_understand_os.md) だけ読んだが、まだ理解の怪しい部分があったので、理解を深めようと読むことにした

## 読む前後の変化

- operating system の基本的な理解が深まった
  - ユーザーモードとカーネルモード
  - プロセスとスレッド
  - メモリ管理
  - ファイルシステムとストレージ管理
- いまなら「詳解 LINUX カーネル」読み始められそう
- 詳細になりすぎず、基本的なことがわかりやすくまとめられていて、読み応えのある本だった

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
    - Their operating system is designed to compromise between individual usability and resource utilization.
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
  - For a computer to start running it needs to have an initial program to run. This initial program, or bootstrap program, tends to be simple. The bootstrap program must know how to load the operating system and how to start executing that system.
    - To accomplish this goal, the bootstrap program must locate the operating-system kernel and load it into memory.
  - THe occurrence of an event is usually signaled by an interrupt from either the hardware or the software.
- Storage Structure
  - The CPU can load instructions only from memory, so any programs to run must be stored there. General-purpose computers run most of their programs from rewritable memory, called main memory.
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
  - The protocols that create a distributed system can greatly affect that system's utility and popularity.
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
  - Frequently, such systems started as small, simple, and limited systems and then grow beyond their original scope.
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
  - これは複数の threads を同時に実行できるし、たくさんの kernel threads を作る必要もない。

### 4.4: Thread Libraries

- A thread library provides the programmer with an API for creating and managing threads.
- There are two primary ways of implementing a thread library.
  - The first approach is to provide a library entirely in user space with no kernel support.
  - The second approach is to implement a kernel-level library supported directly by the operating system.
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
  - The SJF scheduling algorithm is probably optimal, in that it gives the minimum average waiting time for a given set of processes.
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
- 以下のようになる。Pi が critical section に入ろうとしている。turn = j に設定して、j が critical section に入ろうとしていないか確認する。もし入ろうとしていたら、入っている間は何もしない。j が終わったあとで、critical section に入り、出る時に自身の flag を false にする。

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
    - When they are finished, their priorities revert to their original values.

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
  - The basic method for implementing paging involves breaking physical memory into fixed-sized blocks called frames and breaking logical memory into blocks of the same size called pages.
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

### 8.7: Example: Intel 32 and 64-bit Architectures

- In this section, we examine address translation for both IA-32 and x86-64 architectures.
- IA-32 Architecture
  - Memory management in IA-32 systems is divided into two components - segmentation and paging - and works as follows: The CPU generates logical addresses, which are given to the segmentation unit.
  - The segmentation unit produces a linear address for each logical address. The linear address is then given to the paging unit, which in turn generates the physical address in main memory.
  - Thus, the segmentation and paging units form the equivalent of the memory-management unit (MMU).
  - IA-32 Segmentation
    - 16 bit のうち、最初の 13 bit は selector と offset のペア。次の 1 bit が LDT (local descriptor table) / GDT (global descriptor table) のどちらに入っているか示し、最後の 2 bit が protection を表す。
    - ここから 32 bit の linear address が決定する。
  - IA-32 Paging
    - The IA-32 architecture allows a page size of either 4KB or 4MB.
    - 4KB の場合は、最上位の 10 桁が page directory を指し、真ん中の 10 桁が page table を指し、最後の 12 桁が offset を表す。
    - 4MB の場合は、最上位の 10 桁が page directory を指し、下位の 22 桁が offset を表す。
    - As software developers began to discover the 4-GB memory limitations of 32-bit architectures, Intel adopted a page address extension (PAE), which allows 32-bit processors to access a physical address space larger than 4GB.
      - 従来の 2 level の構造から 3 level に拡張する。
      - 4KB の場合、最上位の 2 桁が page directory pointer table を指し、次の 9 桁が page directory、その次の 9 桁が page table を指し、最後の 12 桁が offset を表す。
    - It is important to note that operating systems support is required to use PAE. Both Linux and Mac OS X support PAE.
- x86-64
  - linear address の最上位の 16 桁は unused、次の 9 桁が page map level 4、次の 9 桁が page directory pointer table、次の 9 桁が page directory、その次の 9 桁が page table を指し、最後の 12 桁が offset を表す。

### 8.8: Example: ARM Architecture

- Although Intel chips have dominated the personal computer market for over 30 years, chips for mobile devices such as smartphones and tablet computers often instead run on 32-bit ARM processors.
- Interestingly, whereas Intel both designs and manufactures chips, ARM only designs them. It then licenses its designs to chip manufactures.
- one-level paging では 1MB or 16MB の section をサポート、two-level paging では 4KB or 16KB の page をサポート。
- また ARM architecture では 2 段階の TLB をサポートしていて、outer level が micro TLB、inner level が main TLB となっている。

### 8.9: Summary

- これまでに書いている内容なので省略

## 9: Virtual-Memory Management

- Virtual memory is a technique that allows the execution of processes that are not completely in memory.
  - One major advantage of this scheme is that programs can be larger than physical memory.
  - Further, virtual memory abstracts main memory into an extremely large, uniform array of storage, separating logical memory as viewed by the user from physical memory.
- Chapter Objectives
  - To describe the benefits of a virtual memory system.
  - To explain the concepts of demand paging, page-replacement algorithms, and allocation of page frames.
  - To discuss the principles of the working-set model.
  - To examine the relationship between shared memory and memory-mapped files.
  - To explore how kernel memory is managed.

### 9.1: Background

- The instructions being executed must be in physical memory.
  - The requirement that instructions must be in physical memory to be executed seems both necessary and reasonable; but it is also unfortunate, since it limits the size of a program to the size of physical memory.
- The ability to execute a program that is only partially in memory would confer many benefits:
  - A program would no longer be constrained by the amount of physical memory that is available. Users would be able to write programs for an extremely large virtual address space, simplifying the programming task.
  - Because each user program could take less physical memory, more programs could be run at the same time, with a corresponding increase in CPU utilization and throughput but with no increase in response time or turnaround time.
  - Less I/O would be needed to load or swap user programs into memory, so each user program would run faster.
- Virtual memory makes the task of programming much easier, because the programmer no longer needs to worry about the amount of physical memory available.
- In addition to separating logical memory from physical memory, virtual memory allows files and memory to be shared by two or more processes through page sharing.

### 9.2: Demand Page

- Suppose a program starts with a list of available options from which the user is to select. Loading the entire program into memory results in loading the executable code for all options, regardless of whether or not an option is ultimately selected by the user.
- An alternative strategy is to load pages only as they are needed. This technique is known as demand paging and is commonly used in virtual memory systems.
- Basic Concept
  - When a process is to be swapped in, the pager guess which pages will be used before the process is swapped out again.
  - Instead of swapping in a whole process, the pager brings only those pages into memory.
  - Thus, it avoids reading into memory pages that will not be used anyway, decreasing the swap time and the amount of physical memory needed.
  - 8.5.3 で述べた valid-invalid bit scheme がここでも使え、valid の場合は the aassociated page is both legal and in memory. invalid の場合は the page either is not valid or is valid but is currently on the disk.
  - もし memory に存在しないページにアクセスしたら（invalid な page にアクセスしたら）page fault が引き起こされる。
  - The procedure for handling this page fault is straightforward:
    - We check an internal table for this process to determine whether the reference was a valid or an invalid memory access.
    - If the reference was invalid, we terminate the process. If it was valid but we have not yet brought in that page, we now page it in.
    - We find a free frame.
    - We schedule a disk operation to read the desired page into the newly allocated frame.
    - When the disk read is complete, we modify the internal table kept with the process and the page table to indicate that the page is now in memory.
    - We restart the instruction that was interrupted by the trap. The process can now access the page as through it had always been in memory.
  - A crucial requirement for demand paging is the ability to restart any instruction after a page fault.
- Performance of Demand Paging
  - Demand paging can significantly affect the performance of a computer system.
  - `evvective access time = (1 - p) * ma + p * page fault time`
    - p は page fault が起きる確率。ma は memory access time。
  - In any case, we are faced with three major components of the page-fault service time:
    - Service the page-fault interrupt.
    - Read in the page.
    - Restart the process.
  - Mobile operating systems typically do not support swapping. Instead, these systems demand-page from the file system and reclaim read-only pages from applications if memory becomes constrained.

### 9.3: Copy-on-Write

- We can use a technique known as copy-on-write, which works by allowing the parent and child processes initialy to share the same pages. These shared pages are marked as copy-on-write pages, meaning that if either process writes to a shared page, a copy of the shared page is created.

### 9.4: Page Replacement

- If we increase our degree of multiprogramming, we are over-allocating memory.
- Over-allocation of memory manifests itself as follows. While a user process is executing, a page fault occurs. The operating system determines where the desired page is residing on the disk but then finds that there are no free frames on the free-frame list; all memory is in use.
- Basic Page Replacement
  - Page replacement takes teh following approach.
    - If no frame is free, we find one that is not currently being used and free it.
    - We can free a frame by writing its contents to swap space and changing the page table to indicate that the page is no longer in memory.
    - We can now use the freed frame to hold the page for which the process faulted.
  - We must solve two major problems to implement demand paging: we must develop a frame-allocation algorithm and a page-replacement algorithm.
    - 各 process にどれだけの frame を与えるか、また replace が必要な場合はどのように選ぶかを決める必要がある。
  - There are many different page-replacement algorithms. Every operating system probably has its own replacement scheme. In general, we want the one with the lowest page-fault rate.
  - We evaluate an algorithm by running it on a paticular string of memory references and computing the number of page faults.
    - The string of memory references is called a reference string.
  - To determine the number of page faults for a particular reference string and page-replacement algorithm, we also need to know the number of page frames available.
- FIFO Page Replacement
  - The simplest page-replacement algorithm is a first-in, first-out (FIFO) algorithm.
  - The FIFO page-replacement algorithm is easy to understand and program. However, its performance is not always good.
- Optimal Page Replacement
  - It is simply this: Replace the page that will not be used for the longest period of time.
  - Unfortunately, the optimal page-replacement algorithm is difficult to implement, because it requires future knowledge of the reference string.
- LRU Page Replacement
  - If the optimal algorithm is not feasible, perhaps an approximation of the optimal algorithm is possible.
    - If we use the recent past as an approximation of the near future, then we can replace the page that has not been used for the longest period of time.
    - This approach is the least recently used (LRU) algorithm.
    - 実装方法は counter を使うものと stack を使うものがある。
- LRU-Approximation Page Replacement
  - Few computer systems provide sufficient hardware support for true LRU page replacement.
    - In fact, some systems provide no hardware support, and other page-replacement algorithms must be used.
    - Many systems provide some help, however, in the form of a reference bit.
  - reference bit を見ることで、その page が利用されたかどうかがわかる。ただし、利用されたことはわかっても、どの順で利用されたかはわからない。
  - Additional-Reference-Bits Algorithm
    - We can gain additional ordering information by recording the reference bits at regular intervals.
    - We can keep an 8-bit byte for each page in a table in memory.
    - 1 bit ずつ右にずらし、low-order bit を捨てていく。直近 8 回 1 度も利用されなければ 00000000、毎回利用されていれば 11111111。2 回目、3 回目、5 回目に利用されていたら 01101000
  - Second-Chance Algorithm
    - The basic algorithm of second-chance replacement is a FIFO replacement algorithm.
    - When a page has been selected, however, we inspect its reference bit. If the value is 0, we proceed to replace this page; but if the reference bit is set to 1, we give the page a second chance and move on to select the next FIFO page.
    - One way to implement the second-chance algorithm (sometimes referred to as the clock algorithm) is as a circular queue.
      - reference bit が 0 のものが見つかるまで順々に進んでいき、見つかったら置き換える。1 のものは 0 に置き換える。
  - Enhanced Second-Chance Algorithm
    - reference bit と modify bit のペアを考えて、second-chance algorithm を拡張する。
      - (0, 0): neither recently used nor modified - best page to replace
      - (0, 1): not recently used but modified - not quite as good, because the page will need to be written out before replacement
      - (1, 0): recently used but clean - probably will be used again soon
      - (1, 1): recently used be and modified - probably will be used again soon, and the page will be need to written out on disk before it can be replaced.
  - Counting-Based Page Replacement
    - The least frequently used (LFU) page-replacement algorithm requires that the page with the smallest count be replaced.
    - The most frequently used (MFU) page-replacement algorithm is based on the argument that the page with the smallest count was probably just brought in and has yet to be used.
  - Page-Buffering Algorithm
    - Other procedures are often used in addition to a specific page-replacement algorithm.
      - 例えば、systems commonly keep a pool of free frames.
  - Applications and Page Replacement
    - In certain cases, applications accessing data through the operating system's virtual memory perform worse than if the operating system provided no buffering at all.
    - Because of such problems, some operating systems give special programs the ability to use a disk partition as a large sequential array of logical blocks, without any file-system data structures.

### 9.5: Allocation of Frames

- How do we allocate fixed amount of free memory among the various processes?
- The simplest case is the simple-user system.
- Minimum Number of Frames
  - We must allocate at least a minimum number of frames.
  - The minimum number of frames is defined by the computer architecture.
  - Whereas the minimum number of frames per process is defined by the architecture, the maximum number is defined by the amount of available physical memory.
- Allocation Algorithms
  - The easiest way to split m frames among n processes is to give everyone an equal share, m/n frames.
    - This scheme is called equal allocation.
  - An alternative is to recognize that various processes will need differing amounts of memory.
    - We can use proportional allocation, in which we allocate available memory to each process according to its size.
  - both equal and proportional allocation どちらにとっても multiprogramming level が変われば allocation は変わる
  - If the multiprogramming level is increased, each process will lose some frames to provide the memory needed for the new process.
  - 他にも process の優先度によって allocation を変えることもできる。
- Global versus Local Allocation
  - We can classify page-replacement algorithms into two broad categories: global replacement and local replacement.
    - global replacement はすべての frame の中から選ぶが、local replacement は own set of allocated frames から選ぶ。
    - これを使うと、high-priority processes の数と low-priority processes の数を常に一定に保てたりする。
  - Non-Uniform Memory Access
    - Systems in which memory access times vary significantly are known collectively as non-uniform memory access (NUMA) systems, and without exception, they are slower than systems in which memory and CPUs are located on the same motherboard.

### 9.6: Thrashing

- The high paging activity is called thrashing. A process is thrashing if it is spending more time paging than executing.
- Cause of Thrashing
  - Thrashing results in severe performance problems.
  - As the degree of multiprogramming increases, CPU utilization also increases, although more slowly, until a maximum is reached. If the degree of multiprogramming is increased even futher, thrashing sets in, and CPU utilization drops sharply. At this point, to increase CPU utilization and stop thrashing, we must decrease the degree of multiprogramming.
  - We can limit the effects of thrashing by using a local replacement algorithm (or priority replacement algorithm).
  - To prevent thrashing, we must provide a process with as many frames as it needs.
    - これを調べるにはいくつか方法があるが working-set strategy ではその process が実際にどのくらい frames を使うか見ている。これを locality model of process execution と定義する。
    - We see that localities are defined by the program structure and its data structures.
- Working-Set Model
  - The model uses a parameter, ⊿, to define the working-set window. The idea is to examine the most recent ⊿ page reference.
  - The set of pages in the most recent ⊿ page references is the working set.
  - The working set is an approximation of the program's locality.
  - working set の property で最も大事なのは size。
  - This working-set strategy prevents thrashing while keeping the degree of multiprogramming as high as possible.
- Page-Fault Frequency
  - The working-set model is successful, and knowledge of the working set can be useful for prepaging, but it seems a clumsy way to control thrashing.
  - page-fault frequency (PFF) のほうがより直接的なアプローチ。
  - Thrashing has a high page-fault rate. Thus, we want to control the page-fault rate.
  - We can establish upper and lower bounds on the disired page-fault rate.
    - もし上回ったら process に別の frame を提供し、下回ったらその process に割り当てている frame を減らす。
- Concluding Remarks
  - thrashing がパフォーマンスに与える影響は大きい
  - The current best practice in implementing a computer facility is to include enough physical memory, whenever possible, to avoid thrashing and swapping.

### 9.7: Memory-Mapped Files

- We can use the virtual memory techniques discussed so far to treat file I/O as routine memory access. This approach, known as memory mapping a file, allows a part of the virtual address space to be logically associated with the file.
- Basic Mechanism
  - Memory mapping a file is accomplished by mapping a disk block to a page (or pages) in memory.
  - Note that writes to the file mapped in memory are not necessarily immediate (synchronous) writes to the file on disk.
  - Some operating systems provide memory mapping only through a specific system call and use the standard system calls to perform all other file I/O.
  - Multiple processes may be allowed to map the same file concurrently, to allow sharing of data.
- Shared Memory in the Windows API
  - The general outline for creating a region of shared memory using memory-mapped files in the Windows API involves first creating a file mapping for the file to be mapped and then establishing a view of the mapped file in a process's virtual address space.
  - A second process can then open and create a view of the mapped file in its virtual address space.
  - The mapped file represents the shared-memory object that will enable communication to take place between the processes.
  - Finally, both processes remove the view of the mapped file with a call to UnmapViewOfFile().
- Memory-Mapped I/O
  - Each I/O controller includes registers to hold commands and the data being transferred. Usually, special I/O instructions allow data transfers between these registers and system memory.
  - To allow more convenient access to I/O devices, many computer architectures provide memory-mapped I/O.
  - Memory-mapped I/O is also convenient for other devices, such as the serial and parallel ports used to connect modems and printers to a computer.

### 9.8: Allocating Kernel Memory

- When a process running in user mode requests additional memory, pages are allocated from the list of free page frames maintained by the kernel.
- Kernel memory is often allocated from a free-memory pool different from the list used to satisfy ordinary user-mode processes.
- In the following sections, we examine two strategies for managing free memory that is assigned to kernel processes: the "buddy system" and slab allocation.
- Buddy System
  - The buddy system allocates memory from a fixed-size segment consisting of physically contiguous pages.
  - 例えば memory segment が 256 KB あって、kernel が 21KB のリクエストを出した場合、256 → 128 → 64 → 32 と分割して、32KB の buddy（2 つにわけた片方を buddy という）を与える。
  - An advantage of the buddy system is how quickly adjacent buddies can be combined to form larger segments using a technique known as coalescing.
  - The obvious drawback to the buddy system is that rounding up to the next highest power of 2 is very likely to cause fragmentation within allocated segments.
    - 例えば 33KB の要求でも 64KB 提供しないといけない。
- Slab Allocation
  - A slab is made up of one or more physically contiguous pages.
  - A cache consists of one or more slabs.
  - The slab-allocation algorithm uses caches to store kernel objects.
  - The slab allocator provides two main benefits:
    - No memory is wasted due to fragmentation.
    - Memory requests can be satisfied quickly.

### 9.9: Other Considerations

- Prepaging
  - The strategy is to bring into memory at one time all the pages that will be needed.
- Page size
  - The designers of an operating system for an existing machine seldom have a choice concerning the page size.
  - However, when new machines are being designed, a decision regarding the best page size must be made.
  - memory の使用率を高めるには page size が小さい方がいい。
  - I/O time は seek / latency / transfer times があり、page size を小さくすると transfer time が小さくなる。しかし transfer time は他の 2 つに比べると影響が小さく、page size を大きくしても seek time と latency time には影響しないため、I/O time を最小化するには page size を大きくしたほうが望ましい。
- TLB Reach
  - Related to the hit ratio is a similar metric: the TLB reach. The TLB reach refers to the amount of memory accessible from the TLB and is simply the number of entries multiplied by the page size.
  - If we double the number of entries in the TLB, we double the TLB reach.
  - Another approach for increasing the TLB reach is to either increase the size of the page or provide multiple page sizes.
- Inverted Page Tables
  - The inverted page table no longer contains complete information about the logical address space of a process, and that information is required if a referenced page is not currently in memory.
  - For the information to be available, an external page table must be kept.
- Program Structure
  - System performance can be improved if the user (or compiler) has an awareness of the underlying demand paging.
  - Careful selection of data structures and programming structures can increase locality and hence lower the page-fault rate and the number of pages in the working set.
  - At a later stage, the compiler and loader can have a significant effect on paging.
- I/O Interlock and Page Locking
  - When demand paging is used, we sometimes need to allow some of the pages to be locked in memory.
  - One such situation occurs when I/O is done to or from user (virtual) memory.
    - CPU が interrupt されて、再び処理が実行できるようになった時には、すでに利用していた page は他の process に利用されている。
  - There are two common solutions to this problem.
    - One solution is never to execute I/O to user memory.
    - Another solution is to allow pages to be locked into memory. Locked pages cannot be replaced. When the I/O is complete, the pages are unlocked.
  - Using a lock bit can be dangerous: the lock bit may get turned on but never turned off. Should this situation occur, the locked frame becomes unusable.

### 9.10: Operating-System Example

- In this section, we describe how Windows and Solaris implement virtual memory.
- Windows
  - Windows implements virtual memory using demand paging with clustering.
  - Clustering handles page faults by bringing in not only the faulting page but also several pages following the faulting page.
- Solaris
  - In Solaris, when a thread incurs a page fault, the kernel assigns a page to the faulting thread from the list of free pages it maintains.

### 9.11: Summary

- これまでに書いている内容なので省略

## 10: File System

- The file system consists of two distinct parts: a collection of files, each storing related data, and a directory structure, which organizes and provides information about all the files in the system.
- Chapter Objectives
  - To explain the function of file systems.
  - To describe the interfaces to file systems.
  - To discuss file-system design tradeoffs, including access methods, file sharing, file locking, and directory structures.
  - To explore file-system protection.

### 10.1: File Concept

- The operating system provides a uniform logical view of its storage devices to define a logical storage unit, the file.
- A file is a named collection of related information that is recorded on secondary storage.
- Files may be free form, such as text files, or may be formatted rigidly. In general, a file is a sequence of bits, bytes, lines, or records, the meaning of which is defined by the file's creator and user. The concept of a file is thus extremely general.
- File Attributes
  - A file is named, for the convenience of its human users, and is referred to by its name.
  - A file's attributes vary from one operating system to another but typically consist of these:
    - Name.
    - Identifier.
    - Type.
    - Location.
    - Size.
    - Protection.
      - Access-control information determines who can do reading, writing, executing, and so on.
    - Time, date, and user identification.
  - The information about all files is kept in the directory structure, which also resides on secondary storage.
- File Operations
  - A file is an abstract data type.
  - The operating system can provide system calls to create, write, read, reposition, delete, and truncate files.
  - Six basic file operations.
    - Creating a file.
      - First, space in the file system must be found for the file. Second, an entry for the new file must be made in the directory.
    - Writing a file.
      - To write a file, we make a system call specifying both the name of the file and the information to be written to the file. The system must keep a write pointer to the location in the file where the next write is to take place.
    - Reading a file.
      - The system needs to keep a read pointer to the location in the file where the next read is to take place.
    - Repositioning within a file.
    - Deleting a file.
      - Having found the associated directory entry, we release all file space, so that it can be reused by other files, and erase the directory entry.
    - Truncating a file.
      - The user may want to erase the contents of a file but keep its attributes.
  - The operating system keeps a table, called the open-file table, containing information about all open files.
  - In summary, several pieces of information are associated with an open file.
    - File pointer.
      - On systems that do not include a file offset as part of the read() and write() system calls, the system must track the last read-write location as a current-file-position pointer. この pointer は process ごとに別々。
    - File-open count.
    - Disk location of the file.
    - Access rights.
      - This information is stored on the per-process table so the operating system can allow or deny subsequent I/O requests.
  - Some operating systems provide facilities for locking an open file (or sections of a file).
    - A shared lock is akin to a reader lock in that several processes can acquire the lock concurrently.
    - A exclusive lock behaves like a writer lock; only one process at a time can acquire such a lock.
  - It is important to note that not all operating systems provide both types of locks: some systems only provide exclusive file locking.
  - Futhermore, operating systems may provide either mandatory or advisory file-locking mechanisms.
    - If the locking scheme is mandatory, the operating system ensures locking integrity. For advisory locking, it is up to software developers to ensure that locks are appropriately acquired and released.
    - As a general rule, Windows operating systems adopt mandatory locking, and UNIX systems employ advisory locks.
    - mandatory locking を使う場合は、synchronization の時と同じように deadlock を発生させないように注意する。
- File Types
  - When we design a file system we always consider whether the operating system should recognize and support file types.
  - A common technique for implementing file types is to include the type as part of the file name.
    - The name is split into two parts - a name and an extension, usually separated by a period.
  - The system uses the extension to indicate the type of the file and the type of operations that can be done on that file.
  - The UNIX system uses a crude magic number stored at the beginning of some files to indicate roughly the type of the file - executable program, shell script, PDF file, and so on.
- File Structure
  - File types also can be used to indicate the internal structure of the file.
  - 複数の file types に対応するためには、それぞれの file structures をサポートするコードを持つ必要がある。
  - operating system がサポートしていないタイプのファイルを定義した場合、severe problems may result.
  - Some operating systems impose a minimal number of file structures. This approach has been adopted in UNIX, Windows, and others.
- Internal File Structure
  - Disk systems typically have a well-defined block size determined by the size of a sector. All disk I/O is performed in units of one block, and all blocks are the same size.

### 10.2: Access Methods

- File store information. When it is used, this information must be accessed and read into computer memory.
- Sequential Access
  - The simplest access method is sequential access. Information in the file is processed in order, one record after the other.
- Direct Access
  - Another method is direct access (or relative access).
  - Here, a file is made up of fixed-length logical records that allow programs to read and write records rapidly in no particular order.
  - For direct access, the file is viewed as a numbered sequence of blocks or records.
  - Direct-access files are of great use for immediate access to large amounts of information.
  - The block number provided by the user to the operating system is normally a relative block number.
  - Not all operating systems support both sequential and direct access for files.
- Other Access Methods
  - Other access methods can be built on top of a direct-access method.
  - These methods generally involve the construction of an index for the file. The index like an index in the back of a book, contains pointers to the various blocks.

### 10.3: Directory and Disk Structure

- Next, we consider how to store files.
- There are typically thousands, millions, even millions of files within a computer.
- Files are stored on random-access storage devices, including hard disks, optical disks, and solid-state disks.
- A storage device can be used in its entirety for a file system. It can also be subdivided for finer-grained control.
- Partitioning is useful for limiting the sizes of individual file systems, putting multiple file-system types on the same device, or leaving part of the device available for other uses, such as swap space or unformatted (raw) disk space.
- Any entity containing a file system is generally known as a volume. Each volume can be thought of as a virtual disk. Volumes can also store multiple operating systems, allowing a system to boot and run more than one operating system.
- Each volume that contains a file system must also contain information about the files in the system.
  - directory 情報と files 情報を持つ
- Storage Structure
  - Solaris の file systems
    - tmpfs
      - a "temporary" file system that is created in volatile main memory and has its contents erased if the system reboots or crashes
    - objfs
      - a "virtual" file system that gives debuggers access to kernel symbols
    - など
- Directory Overview
  - The directory can be viewed as a symbol table that translates file names into their directory entries.
  - When considering a particular directory structure, we need to keep in mind the operations that are to be performed on a directory:
    - Search for a file.
    - Create a file.
    - Delete a file.
    - List a directory.
    - Rename a file.
    - Traverse the file system.
- Single-Level Directory
  - The simplest directory structure is the single-level directory.
  - All files are contained in the same directory, which is easy to support and understand.
  - A single-level directory has significant limitations, however, when the number of files increases or when the system has more than one user.
    - Since all files are in the same directory, they must have unique names.
- Two-Level Directory
  - master file directory (MFD) があって、その下に user file directory (UFD) がある。
  - When a user job starts or a user logs in, the system's master file directory is searched.
  - When a user refers to a particular file, only his own UFD is searched. To delete a file, the operating system confines its search to the local UFD; thus, it cannot accidentally delete another user's file that has the same name.
  - Although the two-level directory structure solves the name-collision problem, it still has disadvantages.
    - This structure effectively isolates one user from another. Isolation is an advantage when the users are completely independent but is a disadvantage when the users want to cooperate on some task and to access one another's file.
  - Every file in the system has a path name.
  - Additional syntax is needed to specify the volume of a file.
    - 例えば Windows だと A volume is specified by a letter follewed by a colon. `C:\userb\test` のような感じ。
    - VMS だと `u:[sst.jdeck]login.com;1` で u が volume 名、1 が version 名。
    - UNIX・Linux だと `u/pbg/test` となり、u が volume 名。
  - loaders や assemblers、compilers なども file の形式で定義されるが、file の検索は current UFD で実行されてしまう。standard solution はそれらの system files を含んだ special user directory を定義して、operating system がその local UFD も検索するようにする。
- Tree-Structured Directories
  - 任意の高さの tree 構造。
  - A directory (or subdirectory) contains a set of files or subdirectories. One bit in each directory entry defines the entry as a file (0) or as a subdirectory (1).
  - In normal use, each process has a current directory.
  - Path name can be two of types: absolute and relative.
    - An absolute path name begins at the root and follows a path down to the specified file, giving the directory names on the path.
    - A relative path name defines a path from the current directory.
  - An interesting policy decision in a tree-structured directory concerns how to handle the decision of a directory.
    - directory が空の時はいいが、files や subdirectories がある時にどうするか。One of two approaches can be taken.
      - Some systems will not delete a directory unless it is empty.
      - An alternative approach, such as that taken by the UNIX rm command, is to provide an option: when a request is made to delete a directory, all that directory's files and subdirectories are also to be deleted.
- Acyclic-Graph Directories
  - A shared directory or file exist in the file system in two (or more) places at once.
  - A tree structure prohibits the sharing of files or directories.
  - An acyclic graph - that is, a graph with no cycles - allows directories to share subdirectories and files.
  - It is important to note that a shared file (or directory) is not the same as two copies of the file.
  - Shared files and subdirectories can be implemented in several ways.
    - A common way, exemplified by many of the UNIX systems, is to create a new directory entry called a link. We resolve the link by using that path name to locate the real file.
    - Another common approach to implementing shared files is simply to duplicate all information about them in both sharing directories.
      - A major problem with duplicate directory entries is maintaining consistency when a file is modified.
- General Graph Directory
  - A serious problem with using an acyclic-graph structure is ensuring that there are no cycles.
  - If cycles are allowed to exist in the directory, we likewise want to avoid searching any component twice, for reasons of correctness as well as performance.
  - In this case, we generally need to use a garbage collection scheme to determine when the last reference has been deleted and the disk space can be reallocated.
    - Garbage collection involves traversing the entire file system, marking everything that can be accessed. Then a second pass collects everything that is not marked onto a list of free space.

### 10.4: File-System Mounting

- Just as a file must be opened before it is used, a file system must be mounted before it can be available to processes on the system.
- The operating system is given the name of the device and the mount point - the location within the location within the file structure where the file system is to be attached.
- Next, the operating system verifies that the device contains a valid file system.
- Finally, the operating system notes in its directory structure that a file system is mounted at the specified mount point.

### 10.5: File Sharing

- User-oriented operating systems must accommodate the need to share files in spite of the inherent difficulties.
- Multiple Users
  - multiple users を認める時、file sharing / file naming / file protection の問題が顕著になる。
  - Most systems have evolved to use the concepts of file (or directory) owner (or user) and group.
    - The owner is the user who can change attributes and grant access and who has the most control over the file.
    - The group attribute defines a subset of users who can share access to the file.
  - The owner and group IDs of a given file (or directory) are stored with the other file attributes.
    - そのファイルに対して、リクエストが来た時は user ID が比較されて owner かどうかチェックされる。同じようにどの group に該当するかもチェックされる。
- Remote File Systems
  - Through the evolution of network and file technology, remote file-sharing methods have changed.
    - The first implemented method involves manually transferring files between machines via programs like ftp.
    - The second major method uses a distributed file system (DFS) in which remote directories are visible from a local machine.
    - In some ways, the third method, the World Wide Web, is a reversion to the first.
  - The Client-Server Model
    - Remote file systems allow a computer to mount one or more file systems from one or more remote machines.
    - In this case, the machine containing the files in the server, and the machine seeking access to the files is the client.
    - The server usually specifies the available files on a volume or directory level.
    - Client identification is more difficult.
      - A client can be specified by a network name or other identifiers, such as an IP address, but these can be spoofed, or imitated.
    - Once the remote file system is mounted, file operation requests are sent on behalf of the user across the network to the server via the DFS protocol.
  - Distributed Information Systems
    - To make client-server systems easier to manage, distributed information systems, also known as distributed naming services, provide unified access to the information needed for remote computing.
    - The domain name system (DNS) provides host-name-to-network-address translations for the entire Internet.
    - Other distributed information systems provide user name/password/user ID/group ID space for a distributed facility.
  - Failure Modes
    - local file systems では様々な理由でエラーが発生する。その影響でファイルやディレクトリが消える場合は、復帰するための仲介が必要になる。
    - Remote file systems have even more failure modes.
      - Because of complexity of network systems and the required interactions between remote machines, many problems can interfere with the proper operation of remote file systems.
    - To implement this kind of recovery from failure, some kind of state information may be maintained on both the client and the server.
- Consistency Semantics
  - Consistency semantics represent an important criterion for evaluating any file system that supports file sharing.
  - UNIX Semantics
    - Writes to an open file by a user are visible immediately to other users who have this file open.
    - One mode of sharing allows users to share the pointer of current location into the file. Thus, the advantage of the pointer by one user affects all sharing users. Here, a file has a single image that interleaves all accesses, regardless of their origin.
  - Session Semantics
    - Writes to an open file by a user are not visible immediately to other users that have the same file open.
    - Once a file is closed, the changes made to it are visible only in sessions starting later. Already open instances of the file do not reflect these changes.
  - Immutable-Shared-Files Semantics
    - Once a file is declared as shared by its creator, it cannot be modified.

### 10.6: Protection

- When information is stored in a computer system, we want to keep it safe from physical damage (the issue of reliability) and improper access (the issue of protection).
  - Reliability is generally provided by duplicate copies of files.
  - Protection can be provided in many ways. For a single-user laptop system, we might protection by locking the computer in a desk drawer or file cabinet.
    - In a larger multiuser system, however, other mechanisms are needed.
- Types of Access
  - Protection mechanisms provide controlled access by limiting the types of file access that can be made.
  - Several different types of operatios may be controlled:
    - Read.
    - Write.
    - Execute.
    - Append.
      - Write new information at the end of the file.
    - Delete.
    - List.
      - List the name and attributes of the file.
  - Other operations, such as renaming, copying, and editing the file, may also be controlled.
- Access Control
  - The most common approach to the protection problem is to make access dependent on the identity of the user.
  - The most general scheme to implement identity-dependent access is to associate with each file and directory an access-control list (ACL) specifying user names and the types of access allowed for each user.
  - These problems can be resolved by use of a condensed version of the access list. To condense the length of the access-control list, many systems recognize three classifications of users in connection with each file:
    - Owner.
    - Group.
    - Universe.
- Other Protection Approaches
  - Another approach to the protection problem is to associate a password with each file.
  - Disadvantage
    - First, the number of passwords that a user needs to rememer may become large, making the scheme impractical.
    - Second, if only one password is used for all the files, then once it is discovered, all files are accessible; protection is on an all-or-none basis.
  - In a multilevel structure, we need to protect not only individual files but also collections of files in subdirectories; that is, we need to provide a mechanism for directory protection.

### 10.7: Summary

- これまでに書いている内容なので省略

## 11: Implementing File-System

- Chapter Objectives
  - To describe the details of implementing local file systems and directory structures.
  - To describe the implementation of remote file systems.
  - To discuss block allocation and free-block algorithms and trade-offs.

### 11.1: File-System Structure

- Disks provide most of the secondary storage on which file systems are maintained. Two characteristics make them convenient for this purpose:
  - A disk can be rewritten in place; it is possible to read a block from the disk, modify the block, and write it back into the same place.
  - A disk can access directly any block of information it contains.
- To improve I/O efficiency, I/O transfers between memory and disk are performed in units of blocks. Each block has one or more sectors.
- A file system poses two quite different design problems.
  - The first problem is defining how the file system should look to the user.
  - The second problem is creating algorithms and data structures to map the logical file system onto the physical secondary-storage devices.
- Layered file system.
  - application programs
  - logical file system
  - file-organization module
  - basic file system
  - I/O control
  - devices
- The I/O control level consists of device drivers and interrupt handlers to transfer information between the main memory and the disk system.
- The basic file system needs only to issue generic commands to the appropriate device driver to read and write physical blocks on the disk.
- The file-organization module knows knows about files and their logical blocks, as well as physical blocks.
- Finally, the logical file system manages metadata information. Metadata includes all of the file-system structure except the actual data (or contents of the files).
- A file-control block (FCB) contains information about the file, including ownership, permissions, and location of the file contents.

### 11.2: File-System Implementation

- Overview
  - On disk, the file system may contain information about how to boot an operating system stored there, the total number of blocks, the number and location of free blocks, the directory structure, and individual files.
    - A boot control block (per volume) can contain information needed by the system to boot an operating system from that volume.
    - A volume control block (per volume) contains volume (or partition) details, such as the number of blocks in the partition, the size of the blocks, a free-block count and free-block pointers, and a free-FCB count and FCB pointers.
    - A directory structure (per file system) is used to organize the files.
    - A per-file FCB contains many details about the file.
  - The in-memory information is used for both file-system management and performance improvement via caching.
    - An in-memory mount table contains information about each mounted volume.
    - An in-memory directory-structure cache holds the directory information of recently accessed directories.
    - The system-wide open-file table contains a copy of the FCB of each open file, as well as other information.
    - The per-process open-file table contains a pointer to the appropriate entry in the system-wide open-file table, as well as other information.
    - Buffers hold file-system blocks when they are being read from disk or written to disk.
- Partitions and Mounting
  - Each partition can be either "raw", containing no file system, or "cooked", containing a file system.
  - Boot information can be stored in separate partition. It has its own format, because at boot time the system does not have the file-system code loaded and therefore cannot interpret the file-system format.
  - This boot loader in turn knows enough about the file-system structure to be able to find and load the kernel and start it executing.
  - The root partition, which contains the operating-system kernel and sometimes other system files, is mounted at boot time.
- Virtual File Systems
  - An obvious but suboptimal method of implementing multiple types of file systems is to write directory and file routines for each type.
  - Instead, however, most operating systes, including UNIX, use object-oriented techniques to simplify, organize, and modularize the implementation.
  - The VFS (virtual file system) layer serves two important functions:
    - It separates file-system-generic operations from their implementation by defining a clean VFS interface. Several implementations for the VFS interface may coexist on the same machine, allowing transparent access to different types of file systems mounted locally.
    - It provides a mechanism for uniquely representing a file throughout a network.
  - The VFS activates file-system-specific operations to handle local requests according to their file-system types and calls the NFS protocol procedures for remote requests.
  - 参考: [VFSとファイルシステムの基礎技術 (2/2)：Linuxファイルシステム技術解説（1）](https://www.atmarkit.co.jp/ait/articles/0305/20/news002_2.html)

### 11.3: Derectory Implementation

- The selection of directory-allocation and directory-management algorithms significantly affects the efficiency, performance, and reliability of the file system.
- Linear List
  - The simplest method of implementing a directory is to use a linear list of file names with pointers to the data blocks.
  - The real disadvantage of a linear list of directory entries is that finding a file requires a linear search.
- Hash Table
  - The hash table takes a value computed from the file name and returns a pointer to the file name in the linear list. Therefore, it can greatly decrease the directory search time.
  - The major difficulties with a hash table are its generally fixed size and the dependence of the hash function on that size.

### 11.4: Allocation methods

- The main problem is how to allocate space to these files so that disk space is utilized effectively and files can be accessed quickly.
- Contiguous Allocation
  - Contiguous allocation requires that each file occupy a set of contiguous blocks on the disk.
  - Accessing a file that has been allocated contiguously is easy. For sequential access, the file system remembers the disk address of the last block referenced and, when necessary, reads the next block.
  - Both sequential and direct access can be supported by contiguous allocation.
  - Contiguous allocation has some problems, however.
    - One difficulty is finding space for a new file.
    - The contiguous-allocation problem can be seen as a particular application of the general dynamic storage-allocation problem, which involves how to satisfy a request of size n from a list of free holes.
      - first hit または best fit で解くのが一般的。
  - All these algorithms suffer from the problem of external fragmentation.
    - One strategy for preventing loss of significant amounts of disk space to external fragmentation is to copy an entire file system onto another disk. We then copy the files back onto the original disk by allocating contiguous space from this one large hole.
      - The cost of this compaction is time, however, and the cost can be particularly high for large hard disks.
  - Another problem with contiguous allocation is determining how much space is needed for file.
    - When the file is created, the total amount of space it will need must be found and allocated.
    - また事前にわかったとしても、そこまでに長い時間使わないのであれば効率が悪い。
  - To minimize these drawbacks, some operating systems use a modified contiguous-allocation scheme.
- Linked Allocation
  - Linked allocation solves all problems of contiguous allocation.
  - The directory contains a pointer to the first and last blocks of the file.
  - There is no external fragmentation with linked allocation, and any free block on the free-space list can be used to satisfy a request. It is never necessary to compact disk space.
  - Linked allocation does have disadvantages, however.
    - The major problem is that it can be used effectively only for sequential-access files. It is inefficient to support a direct-access capability for linked-allocation files.
    - Another disadvantage is the space required for the pointers.
      - The usual solution to this problem is to collect blocks into multiples, called clusters, and to allocate clusters rather than blocks. Pointers then use a much smaller percentage of the file's disk space.
    - Yet another problem of linked allocation is reliability.
      - Recall that the files are linked together by pointers scattered all over the disk, and consider what would hapen if a pointer were lost or damaged.
      - One partial solution is to use doubly linked lists, and another is to store the file name and relative block number in each block.
  - An important variation on linked allocation is the use of a file-allocation table (FAT).
    - The table entry contains the block naumber of the next block in the file.
    - つなげていくと last block に到達する。
- Indexed Allocation
  - Linked allocation solves the external-fragmentation and size-declaration problems of contiguous allocation. However, in the absence of a FAT, linked allocation cannot support efficient direct access.
  - Indexed allocation solves this problem by bringing all the pointers together into one location: the index block.
  - Indexed allocation supports direct access, without suffering from external fragmentation, because any free block on the disk can satisfy a request for more space.
  - Indexed allocation does suffer from wasted space, however. The pointer overhead of the index block is generally greater than the pointer overhead of linked allocation.
  - pointer が多くなると、index block が小さくて足りなくなる。それを解決するために以下のような方法がある。
    - Linked scheme.
      - To allow for large files, we can link together several index blocks.
    - Multilevel index.
      - first-level index block と second-level index block を作って、first-level index から second-level index block を見つける。
    - Combined scheme.
      - direct blocks と indirect blocks を持つ。
- Performance
  - linked allocation は sequential access には強いが、direct access に弱い。
  - いくつかのシステムは contiguous allocation で direct-access をサポートし、いくつかのシステムは linked allocation で sequential-access をサポートする。
  - The performance of indexed allocation depends on the index structure, on the size of the file, and on the position of the block desired.

### 11.5: Free-Space Management

- Since disk space is limited, we need to reuse the space from deleted files for new files, if possible.
- To keep track of free disk space, the system maintains a free-space list. The free-space list records all free disk blocks - those not allocated to some file or directory.
- Bit Vector
  - Frequently, the free-space list is implemented as a bit map or bit vector.
  - 1 だと free、0 だと allocated を表す。001001011100... みたいな感じ。
  - Unfortunately, bit vectors are inefficient unless the entire vector is kept in main memory.
    - disk size が大きくなって、bit vector も大きくなると問題になる。
- Linked List
  - Another approach to free-space management is to link together all the free disk blocks, keeping a pointer to the first free block in a special location on the disk and caching it in memory.
  - This first block contains a pointer to the next free disk block, and so on.
  - list を横断して探す場合は効率が悪い。だが、大抵の場合は初めの block を割り当ててれば問題ない。
- Grouping
  - linked list のやり方を改善して、最後の block に他の free blocks のアドレス情報を持たせる。これですぐに探せるようにする。
- Counting
  - contiguous allocation を行う。
  - free space list には、first free block のアドレスと the number of free contiguous blocks の情報を持たせる。
- Space Maps
  - Oracle's ZFS file system の話。大量の files や directories、file systems を扱う。この状況で free space をうまく管理するために、いくつかのテクニックを組み合わせて利用している。
  - First, ZFS creates metaslabs to divide the space on the device into chunks of manageable size.
  - Each metaslabs has an associated space map.
  - The space map is a log of all block activity, in time order, in counting format.
  - The in-memory space map is then an accurate representation of the allocated and free space in the metaslab.
  - Finally, the free-space list is updated on disk as part of the transaction-oriented operations of ZFS.

### 11.6: Efficiency and Performance

- Efficiency
  - The efficient use of disk space depends heavily on the disk-allocation and directory algorithms in use.
  - Generally, every data item associated with a file needs to be considered for its effect on efficiency and performance.
  - efficiency は pointers の size にも影響する。
    - Using 32-bit pointers limits the size of a file to 2^32, or 4GB.
- Performance
  - Some systems maintain a separate section of main memory for a buffer cache, where blocks are kept under the assumption that they will be used again shortly.
  - Other systems cache file data using a page cache.
  - Several systems - including Solaris, Linux, and Windows - use page caching to cache both process pages and file data. This is known as unified virtual memory.
    - Some versions of UNIX and Linux provide a unified buffer cache.
  - Regardless of whether we are caching disk blocks or pages (or both), LRU seems a reasonable general-purpose algorithm for block or page replacement.
    - However, the evolution of the Solaris page-caching algorithms reveals the difficulty in choosing an algorithm.
  - Another issue that can affect the performance of I/O is whether writes to the file system occur synchronously or asynchronously.
    - Most writes are asynchronous. However, metadata writes, among others, can be synchronous.
  - Some systems optimize their page cache by using different replacement algorithms, depending on the access type of the file.
    - free-behind や read-ahead を使う場合がある。

### 11.7: Recovery

- Files and directories are kept both in main memory and disk, and care must be taken to ensure that a system failure does not result in loss of data or in inconsistency.
  - We also consider how a system can recover from such a failure.
- Some changes may go directly to disk, while others may be cached. If the cached changes do not reach disk before a crash occurs, more corruption is possible.
- In addition to crashes, bugs in file-system implementation, disk controllers, and even user applications can corrupt a file system.
- Consistency Checking
  - For detection, a scan of all the metadata on each file system can confirm or deny the consistency of the system.
  - Unfortunately, this scan can take minutes or hours and should occur every time the system boots.
  - 代わりに status bit を用意し、file system の状態をそこに記憶している。metadata の変更が始まったら status bit を set し、変更が完了したら clear する。もし set されたままだったら、consistency checker を走らせる。
  - The consistency checker - a systems program such as fsck in UNIX - compares the data in the directory structure with the data blocks on disk and tries to fix any inconsistencies it finds.
  - 例えば linked allocation で link が壊れた場合は、data block から再構築するなど。
- Log-Structured File Systems
  - database の log-based recovery algorithms を consistency checking でも適用する。
  - The resulting implementations are known as log-based transaction-oriented (or journaling) file systems.
  - Fundamentally, all metadata changes are written sequentially to a log.
    - Each set of operations for performing a specific task is a transaction.
  - A side benefit of using logging on disk metadata updates is that those updates proceed much faster than when they are applied directory to the on-disk data structures.
- Other Solutions
  - These systems never overwrite blocks with new data.
  - Rather, a transaction writes all data and metadata changes to new blocks.
- Backup and Restore
  - Magnetic disk sometimes fail, and care must be taken to ensure that the data lost in such a failure are not lost forever.
  - 典型的な backup は incremental backup で差分だけバックアップする。

### 11.8: NFS (Network File System)

- They are typically integrated with the overall directory structure and interface of the client system.
- Overview
  - Subject to access-rights accreditation, any file system, or any directory within a file system, can be mounted remotely on top of any local directory.
  - FIgure 11.14 に mounts の図と cascading mounts の図あり。
  - One of the design goals of NFS was to operate in a heterogeneous environment of different machines, operating systems, and network architectures.
  - The NFS specification distinguish between the services provided by a mount mechanism and the actual remote-file-access services.
    - 前者を提供するのが mount protocol、後者を提供するのが NFS protocol。The protocols are specified as sets of RPCs.
- The Mount Protocol
  - The mount protocol establishes the initial logical connection between a server and a client.
  - The server maintains an export list that specifies local file systems that it exports for mounting, along with names of machines that are permitted to mount them.
  - The server also maintains a list of the client machines and the corresponding currently mounted directories.
- The NFS Protocol
  - The NFS protocol provides a set of RPCs for remote file operations. The procedures support the following operations:
    - Searching for a file within a directory
    - Reading a set of directory entries
    - Manipulating links and directories
    - Accessing file attributes
    - Reading and writing files
  - These procedures can be invoked only after a file handle for the remotely mounted directory has been established.
  - The omission of open and close operations is intentional.
  - A prominent feature of NFS servers is that they are stateless.
  - A further implication of the stateless-server philosophy and a result of the synchrony of an RPC is that modified data must be committed to the server's disk before results are returned to the client.
  - NFS is integrated into the operating system via a VFS.
    - Figure 11.15 がわかりやすい
- Path-Name Translation
  - Path-name translation in NFS involves the parsing of a path name such as `/usr/local/dir1/file.txt` into separate directory, or components: (1) usr, (2) local, and (3) dir1.
  - So that lookup is fast, a directory-name-lookup cache on the client side holds the vnodes for remote directory names.
- Remote Operations
  - With the exception of opening and closing files, there is an almost one-to-one correspondence between the regular UNIX system calls for file operations and the NFS protocol RPCs. Thus, a remote file operation can be translated directly to the corresponding RPC.
  - There are two caches: the file-attribute (inode-information) cache and the file-blocks cache.
    - The cached file blocks are used only if the corresponding cached attributes are up to date.

### 11.9: Example: The WAFL File System

- The write-anywhere file layout (WAFL) from Network Appliance is an example of this sort of optimization. WAFL is a powerful, elegant file system optimized for random writes.
- WAFL は distributed file system として使われ、It can provide files to clients via the NFS, CIFS, ftp, and http protocols, although it was designed just for NFS and CIFS.
- snapshots, clones, replications などの特徴がある。

### 11.10: Summary

- これまでに書いている内容なので省略

## 12: Mass-Storage Structure

- Chapter Objectives
  - To describe the physical structure of secondary storage devices and its effects on the uses of the devices.
  - To explain the performance characteristics of mass-storage devices.
  - To evaluate disk scheduling algorithms.
  - To discuss operating-system services provided for mass storage, including RAID.

### 12.1: Overview of Mass-Storage Structure

- In this section, we present a general overview of the physical structure of secondary and tertiary storage devices.
- Magnetics Disks
  - Magnetic disks provide the bulk of secondary storage for modern computer systems.
  - Each disk platter has a flat circular shape, like a CD.
  - We store information by recording it magnetically on the platters.
  - platter は tracks に分割できて、track は sectors に分割できる。The set of tracks that are at one arm position makes up a cylinder.
- Solid-State Disks (SSDs)
  - Simply described, an SSD is nonvolatile memory that is used like a hard drive.
  - traditional hard disk より省エネで信頼性が高いが、コストも高い。
  - Because SSDs can be much faster than magnetic disk drives, standard bus interfaces can cause a major limit on throughput.
  - disk head がないので、このあと出る disk-scheduling algorithm は当てはまらない。ただし、throughput と formatting は当てはまる。
- Magnetic Tapes
  - Magnetic tape was used as an early secondary-storage medium.
  - Although it is relatively permanent and can hold large quantities of data, its access time is slow compared with that of main memory and magnetic disk.
    - さらに random access になると magnetic disk の 1000 倍程度遅い。
  - Tapes are used mainly for backup, for storage of infrequently used information, and as a medium for transferring information from one system to another.

### 12.2: Disk Structure

- Modern magnetic disk drives are addressed as large one-dimensional arrays of logical blocks. where the logical block is the smallest unit of transfer.
  - size は大抵 512 bytes。
- The one-dimensional array of logical blocks is mapped onto the sectors of the disk sequentially.
- By using this mapping, we can - at least in theory - convert a logical block number into an old-style disk address that consists of a cylinder number, a track number within that cylinder, and a sector number within that track.
  - しかし実際にはこの translation は難しい。

### 12.3: Disk Attachment

- Computers access disk storage in two ways.
  - One way is via I/O ports (or host-attached storage); this is common on small systems.
  - The other way is via a remote host is a distributed file system; this is referred to as network-attached storage.
- Host-Attached Storage
  - Host-attached storage is storage accessed through local I/O ports.
  - A wide variety of storage devices are suitable for use as host-attached storage.
- Network-Attached Storage (NAS)
  - A network-attached storage device is a special-purpose storage system that is accessed remotely over a data network.
  - Network-attached storage provides a convenient way for all the computers on a LAN to share a pool of storage with the same ease of naming and access enjoyed with local host-attached storage.
- Storage-Area Network
  - One drawback of network-attached storage systems is that the storage I/O operations consume bandwidth on the data network, thereby increasing the latency of network communicaion.
  - A storage-area network (SAN) is a private network (using storage protocols rather than networking protocols) connecting servers and storage units.
  - The power of SAN lies in its flexibility. Multiple hosts and multiple storage arrays can attach to the same SAN, and storage can be dynamically alocated to hosts.

### 12.4: Disk Scheduling

- 効率的に disk drive を使うには fast access time と large disk bandwidth が必要。
- For magnetic disks, the access time has two major components.
  - The seek time is the time for the disk arm to move the hands to the cylinder containing the desired sector.
  - The rotational latency is the additional time for the disk to rotate the desired sector to the disk head.
- The disk bandwidth is the total number of bytes transferred, divided by the total time between the first request for service and the completion of the last transfer.
- If the drive or controller is busy, any new requests for service will be placed in the queue of pending requests for that drive.
  - pending request の中からどのように次に実行するものを決めるかが disk-scheduling algorithm。
- FCFS Scheduling
  - The simplest form of disk scheduling is, of course, the first-come, first-served (FCFS) algorithm.
  - しかしこれだと disk head の移動が多くなり、一般的には fastest service を提供できない。
- SSTF Scheduling
  - It seems reasonable to service all the requests close to the current head position before moving the head far away to service other requests.
  - This assumption is the basis for the shortest-seek-time-first (SSTF) algorithm.
  - The SSTF algorithm selects the request with the least seek time from the current head position.
  - It may cause starvation of some requests.
  - Although the SSTF algorithm is a substantial improvement over the FCFS algorithm, it is not optimal.
- SCAN Scheduling
  - In the SCAN algorithm, the disk arm starts at one end of the disk and moves toward the other end, servicing requests as it reaches each cylinder, until it gets to the other end of the disk. At the other end, the direction of head movement is reversed, and servicing continues.
  - The SCAN algorithm is sometimes called the elevator algorithm.
- C-SCAN Scheduling
  - Circular SCAN (C-SCAN) scheduling is a variant of SCAN designed to provide a more uniform wait time.
  - C-SCAN は SCAN と同様、まず端まで head を動かす。When the head reaches the other end, however, it immediately returns to the beginning of the disk without servicing any requests on the return trip.
- LOOK Scheduling
  - More commonly, the arm goes only as far as the final request in each direction.
  - LOOK scheduling と C-LOOK scheduling がある。
- Selection of a Disk-Scheduling Algorithm
  - With any scheduling algorithm, however, performance depends heavily on the number and types of requests.
  - 他にも file-allocation method や location of directories and index blocks に影響される。
  - Because of these complexities, the disk-scheduling algorithm should be written as a separate module of the operating system, so that it can be replaced with a different algorithm if necessary.
  - It is difficult for the operating system to schedule for improved rotational latency, though, because modern disks do not disclose the physical location of logical blocks.

### 12.5: Disk Management

- Disk Formatting
  - A new magnetic disk is a blank slate: it is just a platter of a magnetic recording material. Before a disk can store data, it must be divided into sectors that the disk controller can read and write.
    - This process is called low-level formatting or physical formatting.
  - The data structure for a sector typically consists of a header, a data area, and a trailer.
  - Before it can use a disk to hold files, the operating system still needs to record its own data structures on the disk. It does so in two steps.
    - The first step is to partition the disk into one or more groups of cylinders.
    - The second step is logical formatting, or creation of a file system. In this step, the operating system stores the initial file-system data structures onto the disk.
  - To increase efficiency, most file systems group blocks together into larger chunks, frequently called clusters.
- Boot Block
  - Bootstrap program initializes all aspects of the system, from CPU registers to device controllers and the contents of main memory, and then starts the operating system.
  - To do its job, the bootstrap program finds the operating-system kernel on disk, loads that kernel into memory, and jumps to an initial address to begin the operating -system execution.
  - For most computers, the bootstrap is stored in read-only memory (ROM).
  - bootstrap code を変更するには ROM hardware chips を変更しなければならないが、それはコストが高い。なのでほとんどのシステムでは、boot ROM に bootstrap loader program を格納し、それは disk から bootstrap program を取得することだけを行う。これにより、bootstrap code 変更のコストが下がる。
  - The full bootstrap program is stored in the "boot blocks" at a fixed location on the disk. A disk that has a boot partition is called a boot disk or system disk.
  - THe code in the boot ROM instructs the disk controller to read the boot blocks into memory and then starts executing that code.
- Bad Blocks
  - Most disks even come from the factory with bad blocks.
  - One strategy is to scan the disk to find bad blocks while the disk is being formatted. Any bad blocks that are discovered are flagged as unusable so that the file system does not allocate them.
  - More sophisticated disks are smarter about bad-block recovery.
    - The controller maintains a list of bad blocks on the disk.
    - spare sectors を用意しておいて、The controller can be told to replace each bad sector logically with one of the spare sectors.
    - This scheme is known as sector sparing or forwarding.
  - As an alternative by sector sparing, some controllers can be instructed to replace a bad block by sector slipping.
  - The replacement of a bad block generally is not totally automatic, because the data in the bad block are usually lost.

### 12.6: Swap-Space Management

- In fact, some systems now use the terms "swapping" ad "paging" interchangeably, reflecting the merging of these two concepts.
- Swap-space management is another low-level task of main memory.
  - The main goal for the design and implementation of swap space is to provide the best throughput for the virtual memory system.
- Swap-Space Use
  - swap space の量は多く見積もっておいたほうが安全。
- Swap-Space Location
  - A swap space can reside in one of two places: it can be carved out of the normal file system, or it can be in a separate disk partition.
  - If the swap space is simply a large file within the file system, normal file-system routines can be used to create it, name it, and allocate its space.
    - だがこのやり方は効率が悪い。
  - Alternatively, swap space can be created in a separate raw partition. No file system or directory structure is placed in this space.
    - This manager uses algorithms optimized for speed rather than for storage efficiency, because swap space is accessed much more frequently than file systems.
  - 両方を使えるような operating system もある。
- Swap-Space Management: An Example
  - The traditional UNIX kernel started with an implementation of swapping that copied entire processes between contiguous disk regions and memory.

### 12.7: RAID Structure

- Disk drives have continued to get smaller and cheaper, so it is now economically feasible to attach many disks to a computer system.
- A variety of disk-organization techniques, collectively called redundant arrays of independent disks (RAID), are commonly used to address the performance and reliability issues.
- Today, RAIDs are used for their higher reliability and higher data-transfer rate, rather than for economic reasons.
- Improvement of Reliability via Redundancy
  - We store extra information that is not normally needed but that can be used in the event of failure of a disk to rebuild the lost information.
  - The simplest (but most expensive) approach to introducing redundancy is to duplicate every disk. This technique is called mirroring.
- Improvement in Performance via Parallelism
  - With multiple disks, we can improve the transfer rate as well (or instead) by striping data across the disks.
  - In its simplest form, data striping consists of splitting the bits of each byte across multiple disks; such striping is called bit-level striping.
  - Parallelism in a disk system, as achieved through striping, has two main goals:
    - Increase the throughput of multiple small accesses by load balancing.
    - Reduce the response time of large access.
- RAID Levels
  - Mirroring provides high reliability, but it is expensive. Striping provides high data-transfer rates, but it does not improve reliability.
  - These schemes have different cost-performance trade-offs and are classified according to levels called RAID levels.
  - RAID level 0
    - non-redundant striping.
  - RAID level 1
    - mirrored disks.
  - RAID level 2
    - RAID level 2 is also known as memory-style error-correcting-code (ECC) organization.
    - 3 台の ECC 用のディスクが必要になる。
  - RAID level 3
    - bit-interleaved parity.
    - parity disk を 1 つ用意すれば良い。RAID 2 の 3 台と比べるとコストが低い。
    - Since reads and writes of a byte are spread out over multiple disks with N-way striping of data, the transfer rate for reading or writing a single block is N times as fast as with RAID level 1.
  - RAID level 4
    - block-interlieved parity.
    - The data-transfer rate for each access is slower, but multiple read accesses can proceed in parallel, leading to higher overall I/O rate.
    - large writes や large reads は速い。
  - RAID level 5
    - block-interleaved distributed parity.
    - RAID level 5 differs from level 4 in that it spreads data and parity among all N+1 disks, rather than storing data in N disks and parity in one disk.
    - RAID system の中で最も一般的。
  - RAID level 6
    - P + Q redundancy.
    - RAID level 6 is much like RAID level 5 but stores extra redundant information to guard against multiple disk failures.
    - Instead of parity, error-correcting codes such as the Read-Solomon codes are used.
  - RAID levels 0 + 1 and 1 + 0
    - RAID 0 provides the performance, while RAID 1 provides the reliability.
    - Generally, this level provides better performance than RAID 5.
    - In RAID 0 + 1, a set of disks are striped, and then the stripe is mirrored to another, equivalent stripe.
    - Another RAID option that is becoming available commercially is RAID level 1 + 0, in which disks are mirrored in pairs and then the resulting mirrored pairs are striped.
      - こちらはグループをまたがった障害が起きても大丈夫。0 + 1 だとグループをまたがった障害が起きると動作不能になる。
  - The implementation of RAID is another area of variation.
    - Volume-management software / host bus-adapter (HBA) / hardware of the storage array / SAN interconnect layer
  - Other features, such as snapshots and replication, can be implemented at each of these levels as well.
    - A snapshot is a view of the file system before the last update took place.
    - Replication involves the automatic duplication of writes between separate sites for redundancy and disaster recovery.
  - One other aspect of most RAID implementation is a hot spare disk or disks.
    - A hot spare is not used for data but is configured to be used as a replacement in case of disk failure.
- Selecting a RAID Level
  - One consideration is rebuild performance.
  - Rebuilding is easiest for RAID level 1, since data can be copied from another disk.
  - RAID level 0 is used in high-performance applications where data loss is not critical.
  - RAID level 1 is popular for applications that require high reliability.
  - RAID 0 + 1 and 1 + 0 are used where both performance and reliability are important.
  - Level 6 is not supported currently by many RAID implementations, but it should offer better reliability than level 5.
- Extensions
  - The concepts of RAID have been generalized to other storage devices, including arrays of tapes, and even to the broadcast of data over wireless systems.
- Problems with RAID
  - Unfortunately, RAID does not always assure that data are available for the operating system and its users.
  - The Solaris ZFS file system takes an innovative approach to solving these problems through the use of checksums - a technique used to verify the integrity of data.
    - If there is a problem with the data, the checksum will be incorrect, and the file system will know about it.
    - If the data are mirrored, and thre is a block with a correct checksum and one with an incorrect checksum, ZFS will automatically update the bad block with the good one.
  - Another issue with most RAID implementations is lack of flexibility.
  - Disks, or partitions of disks, are gathered together via RAID sets into pools of storage. A pool can hold one or more ZFS file systems.

### 12.8: Stable-Storage Implementations

- A disk write results in one of three outcomes:
  - Successful completion.
  - Partial failure.
  - Total failure.
- An output operation is executed as follows:
  - Write the information onto the first physical block.
  - When the first write completes successfully, write the same information onto the second physical block.
  - Declare the operation complete only after the second write completes successfully.
- During recovery from a failure, each pair of physical block is examined.
- We can extend this procedure easily to allow the use of an arbitrarily large number of copies of each block of stable storage.

### 12.9: Summary

- これまでに書いている内容なので省略

## 13: I/O Systems

- Chapter Objectives
  - To explore the structure of an operating system's I/O subsystem.
  - To discuss the principles and complexities of I/O hardware.
  - To explain the performance aspects of I/O hardware and software.

### 13.1: Overview

- The control of devices connected to the computer is a major concern of operating-system designers.
- I/O-device technology exhibits two conflicting trends.
  - On the one hand, we see increasing standardization of software and hardware interfaces.
  - On the other hand, we see increasingly broad variety of I/O devices.
- The device drivers present a uniform device-access interface to the I/O subsystem, much as system calls provide a standard interface between the application and the operating system.

### 13.2: Hardware

- Most fit into the general categories of storage devices (disks, tapes), transmission devices (network connections, Bluetooth), and human-interface devices (screen, keyboard, mouse, audio in and out).
- Despite the incredible variety of I/O devices, though, we need only a few concepts to understand how the devices are attached and how the software can control the hardware.
- A device communicates with a computer system by sending signals over a cable or even through the air.
- A bus is a set of wires and a rigidly defined protocol that specifies a set of messages that can be sent on the wires.
- Buses are used widely in computer architecture and vary in their signaling methods, speed, throughput, and connection methods.
- PCI bus (the common PC system bus) / expansion bus (keyboard や USB ports など比較的遅い devices をつなぐ) / Small Computer System Interface (SCSI) bus / PCI Express (PCIe) / HyperTransport などがある。
- A controller is a collection of electronics that can operate a port, a bus, or a device.
  - A serial-port controller is a simple device controller. It is a single chip (or portion of a chip) in the computer that controls the signals on the wires of a serial port.
- How can the processor give commands and data to a controller to accomplish an I/O transfer?
  - The short answer is that the controller has one or more registers for data and control signals. The processor communicates with the controller by reading and writing bit patterns in these registers.
- Alternatively, the device controller can support memory-mapped I/O.
  - In this case, the device-control registers are mapped into the address space of the processor.
  - The CPU executes I/O requests using the standard data-transfer instructions to read and write the device-control registers at their mapped locations in physical memory.
- Some systems use both techniques.
- An I/O port typically consists of four registers, called the status, control, data-in, and data-out registers.
  - The data-in register is read by the host to get input.
  - The data-out register is written by the host to send output.
  - The status register contains bits that can be read by the host. current command の実行が完了したかなどの情報を持つ。
  - The control register can be written by the host to start a command or to change the mode of a device.
- Polling
  - controller と host の間で producer-consumer relationship を作っていく例をもとに考える。
  - The host writes output through a port, coordinating with the controller by handshaking as follows:
    - The host repeatedly reads the busy bit until that bit becomes clear. (busy の間、controller は status register の busy bit を 1 に set する)
    - The host sets the write bit in the command register and writes a byte into the data-out register.
    - The host sets the command-ready bit.
    - When the controller notices that the command-ready bit is set, it sets the busy bit.
    - The controller reads the command register and sees the write command. It reads the data-out register to get the byte and does the I/O to the device.
    - The controller clears the command-ready bit, clears the error bit in the status register to indicate that the device I/O succeeded, and clears the busy bit to indicate that it is finished.
- Interrupts
  - The basic interrupt mechanism works as follows.
    - The CPU hardware has a wire called the interrupt-request line that the CPU senses after executing every instruction.
    - When the CPU detects that a contrller has asserted a signal on the interrupt-request line, the CPU performs a state save and jumps to the interrupt-handler routine at a fixed address in memory.
    - The interrupt handler determines the cause of the interrupt, performs the necessary processing, performs a state restore, and executes a return from interrupt instruction to return the CPU to the execution state prior to the interrupt.
  - In a modern operating system, however, we need more sophisticated interrupt-handling features.
    - We need the ability to defer interrupt handling during critical processing.
    - We need an efficient way to dispatch to the proper interrupt handler for a device without first polling all the devices to see which one raised the interrupt.
    - We need multilevel interrupts, so that the operating system can distinguish between high- and low-priority interrupts and can respond with the appropriate degree of urgency.
    - 上記 3 つの機能は CPU と interrupt-controller hardware によって提供される。
  - Most CPUs have two interrupt request lines.
    - One is the nonmaskable interrupt, which is reserved for events such as unrecoverable memory errors.
    - The second interrupt line is maskable: it can be turned off by the CPU before the execution of critical instruction sequences that must not be interrupted.
  - The interrupt mechanism accepts an address - a number that selects a specific interrupt-handling routine from a small set.
    - In most architectures, this address is an offset in a table called the interrupt vector.
  - The interrupt mechanism also implements a system of interrupt priority levels.
    - These levels enable the CPU to defer the handling of lower-priority interrupts without masking all interrupts and makes it possible for a high-priority interrupt to preempt the execution of a lower-priority interrupt.
  - ハードウェア割り込みは CPU の外部からの要求であるのに対して、ソフトウェア割り込みは CPU 内部の要因で発生するもの。
  - In summary, interrupts are used throughout modern operating systems to handle asynchronous events and to trap to supervisor-mode routines in the kernel.
- Direct Memory Access
  - programmed I/O (PIO): CPU を介して周辺機器とメインメモリの間のデータ転送を行う方式のこと。
  - Many computers avoid burdening the main CPU with PIO by offloading some of this work to a a special-purpose processor called a direct-memory-access (DMA) controller.
  - Handshaking between the DMA controller and the device controller is performed via a pair of wires called DMA-request and DMA-acknowledge.
  - Some computer architectures use physical memory address for DMA, but others perform direct virtual memory access (DVMA), using virtual addresses that undergo translation to physical addresses.
- I/O Hardware Summary
  - Main concepts
    - A bus
    - A controller
    - An I/O port and its registers
    - The handshaking relationship between the host and a device controller
    - The execution of this handshaking in a polling loop or via interrupts
    - The offloading of this work to a DMA controller for large transfer

### 13.3: Application I/O Interface

- In this section, we discuss structuring techniques and interfaces for the operating system that enable I/O devices to be treated in a standard, uniform way.
- A kernel I/O structure
  - software では kernel -> kernel I/O subsystem -> 各 device driver で、hardware では各 device controller -> 各 device となっている。
  - The purpose of the device-driver layer is to hide the difference among device controllers from the I/O subsystem of the kernel, much as the I/O system calls encapsulate the behavior of devices in a few generic classes that hide hardware differences from applications.
- Most operating systems also have an escape (or back door) that transparently passes arbitrary commands from an application to a device driver.
- Block and Character Devices
  - The block-device interface captures all the aspects necessary for accessing disk drives and other block-oriented devices.
  - Memory-mapped file access can be layered on top of block-device drivers.
  - A keyboard is an example of a device that is accessed through a character-stream interface.
    - input devices や output devices ではこの形式が便利。
- Network Devices
  - network I/O は disk I/O と異なる部分が多いので、disk で利用している interface とは別の interface を用意している operating systems が多い。
  - One interface available in many operating systems, including UNIX and Windows, is the network socket interface.
  - By analogy, the system calles in the socket interface enable an application to create a socket, to connect a local socket to a remote address, to listen for any remote application to plug into a socket created by another application, and to send and receive packets over the connection.
- Clocks and Timers
  - Most computers have hardware clocks and timers that provide three basic functions:
    - Give the current time.
    - Give the elapsed time.
    - Set a timer to trigger operation X at time T.
- Nonblocking and Asynchronous I/O
  - Another aspect of the system-call interface relates to the choice between blocking I/O and nonblocking I/O.
    - When an application issues a blocking system call, the execution of the application is suspended.
    - nonblocking I/O の場合は同時に実行できる。
  - An alternative to a nonblocking system cal is an asynchronous system call.
    - An asynchronous call returns immediately, without waiting for the I/O to complete.
    - The difference between nonblocking nad asynchronous system calls is that a nonblocking read() returns immediately with whatever data are available - the full number of bytes requested, fewer, or none at all. An asynchronous read() call requests a transfer that will be performed in its entirety but will complete at some future time.
      - 参考: [ノンブロッキングI/Oと非同期I/Oの違いを理解する](https://blog.takanabe.tokyo/2015/03/%E3%83%8E%E3%83%B3%E3%83%96%E3%83%AD%E3%83%83%E3%82%AD%E3%83%B3%E3%82%B0i/o%E3%81%A8%E9%9D%9E%E5%90%8C%E6%9C%9Fi/o%E3%81%AE%E9%81%95%E3%81%84%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B/)
- Vectored I/O
  - Vectored I/O allows one system call to perform multiple I/O operations involving multiple locations.
  - Programmers make use of scatter-gather I/O features to increase throughput and decrease system overhead.
  - 参考: [Linux におけるファイル I/O の基礎](https://itkq.jp/blog/2017/05/10/linux-file-and-io/)

### 13.4: Kernel I/O Subsystem

- Kernel provide many services related to I/O. Several services - scheduling, buffering, caching, spooling, device reservation, and error handling - are provided by the kernel's I/O subsystem and build on the hardware and device-driver infrastructure.
- I/O scheduling
  - To schedule a set of I/O requests means to determine a good order in which to execute them.
  - When a kernel supports asynchronous I/O, it must be able to keep track of many I/O requests at the same time. For this purpose, the operating system might attach the wait queue to a device-status table.
- Buffering
  - A buffer, of course, is a memory area that stores data being transferred between two devices or between a device and an application.
  - Buffering is done for three reasons.
    - One reason is to cope with a speed mismatch between the producer and consumer of a data stream.
    - A second use of buffering is to provide adaptations for devices that have different data-transfer sizes.
    - A third use of buffering is to support copy semantics for application I/O.
- Caching
  - A cache is a region of fast memory that holds copies of data. Access to the cached copy is more efficient than access to the original.
  - The difference between a buffer and a cache is that a bufffer may hold the only existing copy of a data item, whereas a cache, by definition, holds a copy on faster storage of an item that resides elseware.
- Spooling and Device Reservation
  - A spool is a buffer that holds output for a device, such as a printer, that cannot accept interleaved data streams.
  - 他にも Some operating systems provide support for exclusive device access by enabling a process to allocate an idle device and to deallocate that device when it is no longer needed.
- Error Handling
  - As a general rule, an I/O system call will return one bit of information about the status of the call, signifying either success or failure.
  - In the UNIX operating system, an additional integer variable named errno is used to return an error code - one of about a hundred values - indicating the general nature of the failure.
- I/O Protection
  - To prevent users from performing illegal I/O, we define all I/O instructions to be privileged instructions.
  - In addition, any memory-mapped and I/O port memory locations must be protected from user access by the memory-protection system.
    - user の direct access が必要な場合もあるので、そういう時のために例えば graphics memory へのアクセスを認めるなら The kernel might provide a locking mechanism to allow a section of graphics memory to be allocated to one process at a time.
- Kernel Data Structures
  - The kernel needs to keep state information about the use of I/O components.
  - UNIX provides file-system access to a variety of entities, such as user files, raw devices, and the address spaces of processes. Although each of these entities supports a read() operation, the semantics differ.
- Kernel I/O Subsystem Summary

### 13.5: Transforming I/O Requests to Hardware Operations

- The application refers to the data by a file name.
- The lifecycle of an I/O request (Figure 13.13)

### 13.6: STREAMS

- UNIX System V has an interesting mechanism, called STREAMS, that enables an application to assemble pipelines of driver code dynamically.
- It consists of a stream head that interfaces with the user process, a driver end that controls the device, and zero or more stream modules between the stream head and the driver end.
  - Each of these components conttains a pair of queues - a read queue and a write queue.
- The benefit of using STREAMS is that it provides a framework for a modular and incremental approach to writing device drivers and network protocols.

### 13.7: Performance

- I/O is a major factor in system performance. It places heavy demands on the CPU to execute device-driver code and to schedule processes fairly and efficiently as they block and unblock.
- Although modern computers can handle many thousands of interrupts per second, interrupt handling is a relatively expensive task.
- Network traffic can also cause a high context-switch.
- We can employ several principles to improve the efficiency of I/O:
  - Reduce the number of context switches.
  - Reduce the number of times that data must be copied in memory while passing between device and application.
  - Reduce the frequency of interrupts by using large transfers, smart controllers, and polling.
  - Increase concurrency by using DMA-knowledgeable controllers or channels to offload simple data copying from the CPU.
  - Move processing primitives into hardware, to allow their operation in device controllers to be concurrent with CPU and bus operation.
  - Balance CPU, memory subsystem, bus, and I/O performance, because an overload in any one area will cause idleness in others.

### 13.8: Summary

- これまでに書いている内容なので省略

## 14: System Protection

- Chapter Objectives
  - To discuss the goals and principles of protection in a modern computer system.
  - To explain how protection domains, combined with an access matrix, are used to specify the resources a process may access.
  - To examine capability- and language-based protection systems.

### 14.1: Goals of Protection

- Modern protection concepts have evolved to increase the reliability of any complex system that makes use of shared resources.
- We need to protection for several reasons.
  - The most obvious is the need to prevent the mischievous, intentional violation of an access restriction by a user.
  - Of more general importance, however, is the need to ensure that each program component active in a system uses system resources only in ways consistent with stated policies.
- Protection can improve reliability by detecting latent errors at the interfaces between component subsystems.
- The role of protection in a computer system is to provide a mechanism for the enforcement of the policies governing resource use.
- Note that mechanisms are distinct from policies.
  - Mechanisms determine how something will be done; policies decide what will be done.

### 14.2: Principles of Protection

- A key, time-tested guiding principle for protection is the principle of least privilege.
- An operating system following the principle of least privilege implements its features, programs, system cals, and data structures so that failure or compromise of a component does the minimum damage and allows the minimum damage to be done.
- Managing users with the principle of least privilege entails creating a separate account for each user, with just the privileges that the user needs.

### 14.3: Domain of Protection

- A computer system is a collection of processes and objects.
  - objects には hardware objects (such as the CPU, memory segments, printers, disks, and tape drives) と software objects (such as files, programs, and semaphores).
- A process should be allowed to access only those resources for which it has authorization.
- Futhermore, at any time, a process should be able to access only those resources that it currently requires to complete its task.
  - This second requirement, commonly referred to as the need-to-know principle, is useful in limiting the amount of damage a faulty process can cause in the system.
- Domain Structure
  - To facilitate the scheme just described, a process operates within a protection domain, which specifies the resources that the process may access.
  - Each domain defines a set of objects and the types of operations that may be invoked on each object. The ability to execute an operation on an object is an access right.
    - A domain is a collection of access rights, each of which is an ordered pair `<object-name, rights-set>`. 例えば `<file F, {read, write}>` など。
  - Domain may share access rights.
  - The association between a process and a domain may be either static, or dynamic.
  - A domain can be realized in a variety of ways:
    - Each user may be a domain.
    - Each process may be a domain.
    - Each procedure may be a domain.
- An Example: UNIX
  - In the UNIX operating system, a domain is associated with the user.
- An Example: MULTICS
  - In the MULTICS system, the protection domains are organaized hierarchically into a ring structure.
  - Each ring corresponds to a single domain.
  - The main disadvantage of the ring (or hierarchical) structure is that it does not allow us to enforce the need-to-know principle.

### 14.4: Access Matrix

- One general model of protection can be viewed abstractly as a matrix, called an access matrix.
  - The rows of the access matrix represent domains, and the columns represent objects.
- The user normally decide the contents of the access-matrix entries.
- Processes should be able to switch from one domain to another.
- Allowing controlled change in the contents of the access-matrix entries requires three additional operations: copy, owner, and control.

### 14.5: Implementation of the Access Matrix

- Global Table
  - The simplest implementation of the access matrix is a global table consisting of a set of ordered triples `<domain, object, rights-set>`.
  - 弱点もある。
    - The table is usually large and thus cannot be kept in main memory, so additional I/O is needed.
    - Virtual memory techniques are often used for managing this table.
    - In addition, it is difficult to take advantage of special grouping of objects or domains.
- Access Lists for Objects
  - The resulting list for each object consists of ordered pairs `<domain, rights-set>`, which define all domains with a nonempty set of access rights for that object.
  - This approach can be extended easily to define a list plus a default set of access rights.
- Capability Lists for Domains
  - A capability list for a domain is a list of objects together with the operations allowed on those objects.
  - The capability list is associated with a domain, but it is never directly accessible to a process executing in that domain.
- A Lock-Key Mechanism
  - The lock-key scheme is a compromise between access lists and capability lists.
  - Each object has a list of unique bit patterns, called locks. Similarly, each domain has a list of unique bit patterns, called keys.
  - A process executing in a domain can access an object only if that domain has a key that matches one of the locks of the object.
- Comparison
  - どの実装にするか選ぶには様々な trade-off がある。
    - Using a global table is simple; however, the table can be quite large and often cannot take advantage of special groupings of objects or domains.
    - Access lists correspond directly to the needs of users.
      - However, because access-right information for a particular domain is not localized, determining the set of access rights for each domain is difficult.
    - Capability lists do not correspond directly to the needs of users, but they are useful for localizing information for a given process.
    - The lock-key mechanism is a compromise between access lists and capability lists.
  - Most systems use a combination of access lists and capabilities.
  - The right to access must still be checked on each access, and the file-table entry has a capability only for the allowed operations.

### 14.6: Access Control

- Solaris 10 advances the protection available in the operating system by explicitly adding the principle of least privilege via role-based access control (RBAC).
- Privileges and programs can also be assigned to roles.
- Users are assigned roles or can take roles based on passwords to the roles.

### 14.7: Revocation of Access Rights

- In a dynamic protection system, we may sometimes need to revoke access rights to objects shared by different users. Various questions about revocation may arise:
  - Immediate versus delayed.
  - Selective versus general.
    - 全 user か一部の user か。
  - Partial versus total.
  - Temporary versus permanent.
- With an access-list scheme, revocation is easy. 削除対象となるものを見つけて消せば良い。
  - Revocation is immediate and can be general or selective, total or partial, and permanent or temporary.
- Schemes that implement revocation for capabilities include the following:
  - Reacquisition.
  - Back-pointers.
  - Indirection.
  - Keys.

### 14.8: Capability-Based System

- An Example: Hydra
  - Hydra is a capability-based protection system that provides considerable flexibility.
- An Example: Cambridge CAP System
  - CAP's capability system is simpler and superficially less power than that of Hydra.
  - CAP has two kinds of capabilities.
    - data capability と software capability。

### 14.9: Language-Based Protection

- Compiler-Based Enforcement
  - What, then, are the relative merits of enforcement based solely on a kernel, as opposed to enforcement provided largely by a compiler?
    - Security.
    - Flexibility.
    - Efficiency.
- Protection in Java
  - Because Java was designed to run in a distributed environment, the Java virtual machine - or JVM - has many built in protection mechanisms.

### 14.10: Summary

- これまでに書いている内容なので省略

## 15: System Security

- Protection is strictly an internal problem. Security, on the other hand, requires not only an adequate protection system but also consideration of the external environment within which system operates.
- Chapter Objectives
  - To discuss security threats and attacks.
  - To explain the fundamentals of encryption, authentication, and hashing.
  - To examine the use of cryptography in computing.
  - To describe various countermeasures to security attacks.

### 15.1: The Security Problem

- intruder と cracker はセキュリティを破ろうとする人を指す言葉として使う。
- 様々な violation
  - Breach of confidentiality.
    - This violation involves unauthorized reading of data.
  - Breach of integrity.
    - This violation involves unauthorized modification of data.
  - Breach of availability.
    - This violation involves unauthorized destruction of data.
  - Theft of service.
    - This violation involves unauthorized use of resources.
  - Denial of service.
    - This violation involves preventing legitimate use of the system.
- The most common attempt is masquerading, in which one participant in a communication pretends to be someone else (another host or another person).
  - By masquerading, attackers breach authentication, the correctness of identification.
- Another common attack is to replay a captured exchange of data.
  - データのやり取りを盗聴して、それを再現することでなりすます。
- 他にも man-in-the-middle attack (中間者攻撃)などがある
- To protect a system, we must take security measures at four levels:
  - Physical.
  - Human.
    - 人が情報を渡すのに加えて social engineering によるものも含まれる。phishing や dumpster diving。
  - Operating system.
  - Network.
- In the remainder of this chapter, we address security at the network and operating-system levels.

### 15.2: Program Threats

- Processes, along with the kernel, are the only means of accomplishing work on a computer. Therefore, writing a program that creates a breach of security, or causing a normal process to change its behavior and create a breach, is a common goal of crackers.
- In this section, we describe common methods by which programs cause security breaches.
- Trojan Horse
  - A code segment that misuses its environment is called a Trojan horse.
  - A variation of the Trojan horse is a program that emulates a login program.
  - Another variation on the Trojan horse is spyware.
  - 参考: [トロイの木馬とは? | ウイルスとの違いや感染被害例について](https://japan.norton.com/trojan-virus-3868)
- Trap Door
  - The designer of a program or system might leave a hole in the software that only she is capable of using. This type of security breach (or trap door) was shown in the movie War Games.
  - Trap doors pose a difficult problem because, to detect them, we have to analyze all the source code for all components of a system.
- Logic Bomb
  - ある特定の状況下で security incident が発生するようにされたものを logic bomb という。
- Stack and Buffer Overflow
  - By using trial and error, or by examining the source code of the attacked program if it is available, the attacker determines the vulnerability and writes a program to do the following:
    - Overflow an input field, command-line argument, or input buffer - for example, on a network daemon - until it writes into the stack.
    - Overwrite the current return address on the stack with the address of the exploit code loaded in next step.
    - Write a simple set of code for the next space in the stack that includes the commands that the attacker wishes to execute - for instance, spawn a shell.
  - The result of this attack program's execution will be a root shell or other privileged command execution.
  - Given the standard memory layout, a cracker could execute a buffer-overflow attack. Her goal is to replace the return address in the stack frame so that it now points to the code segment containing the attacking program.
  - One solution of this problem is for the CPU to have a feature that disallows execution of code in a stack section of memory.
- Viruses
  - A virus is a fragment of code embeded in a legitimate program.
  - Viruses are self-replicating and are designed to "infect" other programs.
  - Viruses are usually borne via e-mail, with spam the most common vector.
  - Another common form of virus transmission uses Microsoft Office files, such as Microsoft Word documents.
  - Note that many viruses belong to more than one category.
    - File.
    - Boot.
    - Macro.
    - Source code.
    - Polymorphic.
    - Encrypted.
    - Stealth.
    - Tunneling.
    - Multipartite.
    - Armored.

### 15.3: System and Network Threats

- In remainder of this section, we discuss some examples of system and network threats, including worms, port scanning, and denial-of-service attacks.
- Worms
  - A worm is a process that uses the spawn mechanism to duplicate itself.
  - worm は grappling hook (also called a bootstrap or vector) program と main program から成る。まず target system に grappling hook をしかけ、そこから infected system に request を送らせる。request に対して、worm を送ると target system が感染する。
  - The other two methods involved operating-system bugs in the UNIX finger and sendmail programs.
- Port Scanning
  - Port scanning is not an attack but rather a means for a cracker to detect a system's vulnerabilities to attack.
  - Port scanning typically is automated, involving a tool that attempts to create a TCP/IP connection to a specific port or a range of ports.
- Denial of Service
  - As mentioned earlier, denial-of-service attacks are aimed not at gaining information or stealing resources but rather at disrupting legitimate use of a system or facility.
  - Denial-of-service attacks are generally network based. They fall into two categories.
    - Attacks in the first category use so many facility resources that, in essence, no useful work can be done.
    - The second category involves disrupting the network of the facility. These atacks result from abuse of some of the fundamental functionality of TCP/IP.
  - Generally, it is impossible to prevent denial-of-service attacks. The attacks use the same mechanism as normal operation.

### 15.4: Cryptography as a Security Tool

- Abstractly, cryptography is used to constrain the potential senders and / or receivers of a message.
- Encryption
  - In this section, we describe important modern encryption principles and algorithms.
  - An encryption algorithm consists of the following components:
    - A set K of keys.
    - A set M of messages.
    - A set C of ciphertexts.
    - An encrypting function E: K → (M → C).
    - A decrypting function D: K  → (C → M).
  - There are two main types of encryption algorithms: symmetric and asymmetric.
  - Symmetric Encryption
    - In a symmetric encryption algorithm, the same key is used to encrypt and to decrypt. Therefore, the secrecy of k must be protected.
    - この symmetric encryption を使った data-encryption standard (DES) がかつてアメリカで最も一般的だった。
    - 2001 年に DES を置き換えるものとして登場したのが advanced encryption standard (AES)。
  - Asymmetric Encryption
    - In an asymmetric encryption algorithm, there are different encryption and decryption keys.
    - An entity preparing to receive encrypted communicaion creates two keys and makes one of them (called the public key).
      - これにより、key を秘密にしなくて良くなった。
      - この public-key encryption を使ったアルゴリズムを RSA という (開発者 3 人の頭文字から名付けられてる)。
    - Asymmetric cryptography is much more computationally expensive to execute.
      - In truth, asymmetric cryptography algorithms are not used for general-purpose encryption of large amounts of data.
      - However, they are used not only for encryption of small amounts of data but also for authentication, confidentiality, and key distribution, as we show in the following sections.
  - Authentication
    - Constraining the set of potential senders of a message is called authentication.
    - An authentication algorithm using symmetric keys consists of the following components:
      - A set K of keys.
      - A set M of messages.
      - A set A of authenticators.
      - A function S: K → (M → A).
      - A function V: K → (M × A → {true, false}).
    - Just as there are two types of encryption algorithms, there are two main varieties of authentication algorithms.
      - The first main type of authentication algorithm uses symmetric encryption.
        - In a message-authentication code (MAC), a cryptographic checksum is generated from the message using a secret key.
      - The second main type of authentication algorithm is a digital-signature algorithm, and the authenticators thus produced are called digital signatures.
        - Digital signatures are very useful in that they enable anyone to verify the authenticity of the message.
        - In a digital-signature algorithm, `kv` is the public key, and `ks` is the private key.
  - Key Distribution
    - The delivery of the symmetric key is a huge challenge.
    - public keys の送付についても注意する必要がある。man-in-the-middle attack の可能性があるため。
    - A digital certificate is a public key digitally signed by a trusted party.
- Implementation of Cryptography
  - Cryptography can be inserted at almost any layer in the OSI model.
    - トランスポート層の SSL や、ネットワーク層の IPSec。
  - Where is cryptographic protection best placed in a protocol stack?
    - In general, there is no devinitive answer.
    - On the one hand, more protocols benefit from protections placed lower in the stack.
    - On the other hand, protection at lower layers in the protocol stack may give insufficient protection to higher-layer protocols.
- An Example: SSL
  - SSL3.0 is a cryptographic protocol that enables two computers to communicate securely - that is, so that each can limit the sender and receiver of messages to the other.
  - 参考: [SSLって何？意味や仕組みをわかりやすく解説！ | さくらのSSL](https://ssl.sakura.ad.jp/column/ssl/)

### 15.5: User Authentication

- Generally, user authentication is based on one or more of three things:
  - the user's possession of something (a key or card)
  - the user's knowledge of something (a user identifier and password)
  - an attribute of the user (fingerprint, retina pattern, or signature)
- Passwords
  - The most common approach to authenticating a user identity is the use of passwords.
  - In practice, most systems require only one password for a user to gain full rights.
- Password Vulnerabilities
  - Passwords are extremely common because they are easy to understand and use. Unfortunately, passwords can often be guessed, accidentally exposed, sniffed, or illegally transferred from an authorized user to an unauthorized one, as we show next.
  - There are two common ways to guess a password.
    - One way is for the intruder (either human or program) to know the user or to have information about the user.
    - The other way is to use brute force, trying enumeration - or all possible combinations of valid password characters - until the password is found.
  - In addition to being guessed, passwords can be exposed as a result of visual or electronic monitoring.
  - The final type of password compromise, illegal transfer, is the result of human nature.
- Securing Passwords
  - One problem with all these approaches is the difficulty of keeping the password secret within the computer.
  - The UNIX system uses secure hashing to avoid the necessity of keeping its password list secret.
    - Because the list is hashed rather than encrypted, it is impossible for the system to decrypt the stored value and determine the original password.
  - The flaw in this method is that the system no longer has control over the passwords.
    - "salt" を password に加えて計算することで、同じ plaintext でも異なる hash values になるようにしている。
  - Another weakness in the UNIX password methods in that many UNIX systems treat only the first eight characters as significant.
- One-Time Passwords
  - To avoid the problems of password sniffing and shoulder surfing, a system can use a set of paired passwords.
    - system がランダムに 1 つを選び、それに答える。
  - One-time password system is one of only a few ways to prevent improper authentication due to password exposure.
- Biometrics
  - Palm- or hand-readers are commonly used to secure physical access - for example, access to a data center.

### 15.6: Implementing Security Defenses

- Most security professionals subscribe to the theory of defense in depth, which states that more layers of defense are better than fewer layers.
- Security Policy
  - まずポリシーから決める。
  - Once the security policy is in place, the people it affects should know it well.
  - The policy should also be a living document that is reviewed and updated periodically to ensure that it is still pertinent and still forward.
- Vulnerability Assessment
  - Such assessments can cover broad ground, from social engineering through risk assessment to port scans.
  - Risk assessment, for example, attempts to value the assets of the entity in question (a program, a management team, a system, or a facility) and determine the odds that a security incident will affect the entity and decrease its value.
  - Networked computers are much more susceptible to security attacks than are standalone systems.
- Intrusion Detection
  - Intrusion detection, as its name suggests, strives to detect attempted or successful intrusions into computer systems and to initiate appropriate responses to the intrusions.
  - These degrees of freedom in the design space for detecting intrusionshave yielded a wide range of solutions, known as intrusion-detection systems (IDSs) and intrusion-prevention systems (IDPs).
  - In the first, called signature-based detection, system input or network traffic is examined for specific behavior patterns (or signatures) known to indicate attacks.
  - The second approach, typically called anomaly detection, attempts through various techniques to detect anomalous behavior within computer systems.
  - anomaly detection は未知の攻撃も特定できるが、signature-based detection は既知のものしか対応できない。ただし、anomaly detection が常に signature-based detection より優れているわけではない。
  - false-alarm rate (間違ったアラームが起きてしまう確率) を下げることは usability を考えるととても大事。
- Virus Protection
  - The best protection against computer viruses is prevention, or the practice of safe computing.
  - Another defense is to avoid opening any e-mail attachments from unknown uers.
- Auditing, Accounting, and Logging
  - Auditing, accounting, and logging can decrease system performance, but they are useful in several areas, including security.

### 15.7: Firewalling to Protect Systems and Networks

- We turn next to the question of how a trusted computer can be conneted safely to an untrustworthy network.
- One solution is the use of a firewall to separate trusted and untrusted network.
  - A firewall is a computer, appliance, or router that sits between the trusted and the untrusted.
  - A network firewall limits network access between the two security domains and monitors and logs all connections.
  - It can also limit connections based on source or destination address, source or destination port, or direction of the connection.
- A personal firewall is a software layer either included with the operating system or added as an application.
  - Rather than limiting communicaion between security domains, it limits communicaion to (and possibly from) a given host.
- An application proxy firewall understands the protocols that application speak across the network.
- 他にも XML firewall や system-call firewall がある。

### 15.8 Computer-Security Classifications

- The U.S. Department of Defense Trusted Computer System Evaluation Criteria specify four security classifications in systems: A, B, C, and D.
  - TCSEC でググると関連記事が出てくる。
  - D が最も低くて、A が最も高い。C は C1 と C2、B は B1 - B3 まである。A は A1 がある。
  - 参考: [TCSEC.pdf](http://www.jssm.net/jssm/secure-os/TCSEC.pdf)

### 15.9: An Example: Windows7

- The Windows 7 security model is based on the notion of user accounts.
- Windows7 uses the idea of a subject to ensure that programs run by a user do not get greater access to the system than the user is authorized to have.
- Windows7 classifies objects as either container objects or noncontainer objects.
  - Container objects, such as directories, can logically contain the new object inherits permission from the parent object.
  - Noncontiner objects inherit no other permissions.

### 15.10: Summary

- これまでに書いている内容なので省略

## 16: The Linux System

- Chapter Objectives
  - To explore the history of the UNIX operating system from which Linux is derived and the principles upon which Linux's design is based.
  - To examine the Linux process model and illustrate how Linux schedules processes and provides interprocess communication.
  - To look at memory management in Linux.
  - To explore how Linux implements file systems and manages I/O devices.

### 16.1: Linux History

- Its development began 1991, when a Finnish university student, Linus Torvalds, began developing a small but self-contained kernel for the 80386 processor, the first true 32-bit processor in Intel's range of PC-compatible CPUs.
- As Linux has matured, a need has arisen for another layer of functionality on top of the Linux system. This need has been met by various Linux distributions.
- The Linux Kernel
  - The first Linux kernel released to the public was version 0.01, dated May 14, 1991.
  - The next milestone, Linux 1.0, was released on March 14, 1994.
    - Perhaps the biggest new feature was networking: 1.0 included support for UNIX's standard TCP/IP networking protocols, as well as a BSD-compatible socket interface for networking programming.
  - In March 1995, the 1.2 kernel was released. This release did not offer nearly the same improvement in functionality as the 1.0 release, but it did support a much wider variety of hardware, including the new PCI hardware bus architecture.
  - 1996 年の 6 月に 2.0 がリリース。
    - This release was given a major version-number increment because of two major new capabilities: support for multiple architectures, including a 64-bit native Alpha port, and symmetric multiprocessing (SMP) support.
  - Linux kernel version 3.0 was released in July 2011. 20 周年を記念してメジャーバージョンが 3 になった。
- The Linux System
  - As we noted earlier, the Linux kernel forms the core of the Linux project, but other components make up a complete Linux operating system.
  - Whereas the Linux kernel is composed entirely of code written from scratch specifically for the Linux project, much of the supporting software that make up the Linux system is not exclusive to Linux but is common to a number of UNIX-like operating systems.
  - The File System Hierarchy Standard document is also maintained by the Linux community as a means of ensuring compatibility across the various system components.
- Linux Distributions
  - These collections, or distributions, include much more than just the basic Linux system.
  - The first distributions managed these packages by simply providing a means of unpacking all the files into the appropriate places.
    - Today's Linux distributions include a package-tracking database that allows packages to be installed, upgraded, or removed painlessly.
  - The SLS distribution, dating back to the early days of Linux, was the first collection of Linux packages that was recognizable as a complete distribution.
  - The Slackware distribution represented a great improvement in overall quality, even though it also had poor package management.
- Linux Licensing
  - Linux is not public-domain software. Public domain implies that the authors have waived copyright rights in the software, but copyright rights in Linux code are still held by the code's various authors.
  - Linux is free software, however, in the sense that people can copy it, modify it, use it in any manner they want, and give away (or sell) their own copies.
  - If you release software that includes any components covered by the GPL (GNU General Public License), you must make source code available alongside any binary distributions.

### 16.2: Design Principles

- In its overall design, Linux resembles other traditional, nonmicrokernel UNIX implementations.
- Although Linux runs on a wide variety of platforms, it was originally developed exclusively on PC architecture.
- Speed and efficiency are still important design goals, but much recent and current work on Linux has concentrated on a third major design goal: standardization.
- Components of a Linux System
  - The Linux system is composed of three main bodies of code, in line with most traditional UNIX implementations:
    - Kernel.
      - The kernel is responsible for maintaining all the important abstractions of the operating system, including such thing as virtual memory and processes.
    - System libraries.
      - The system libraries define a standard set of functions through which applications can interact with the kernel.
      - The system utilities are programs that perform individual, specialized management tasks.
    - System utilities.
      - system utilities と user utilities がある。
  - kernel mode と user mode がある。
    - Unlike kernel mode, user mode has access only to a controlled subset of the system's resources.
  - One of the most important user utilities is the shell, the standard command-line interface on UNIX systems.
    - Linux supports many shells; the most common is the bourne-Again shell (bash).

### 16.3: Kernel Modules

- Kernel modules are convenient for several reasons.
  - 例えば kernel になにか修正を加えた時、kernel 全体を recompile して relinking して reloading してというのはとても大変。
  - If you use kernel modules, you do not have to make a new kernel to test a new driver - the driver can be compiled on its own and loaded into the already running kernel.
  - Kernel modules allow a Linux system to be set up with a standard minimal kernel, without any extra device drivers built in. Any device drivers that the user needs can be either loaded explicitly by the system at startup or loaded automatically by the system on demand and unloaded when not in use.
  - The module support under Linux has four components:
    - The module-management system allows modules to be loaded into memory and to communicate with the rest of the kernel.
    - The module loader and unloader, which are user-mode utilities, work with the module-management system to load a module into memory.
    - The driver-registration system allows modules to tell the rest of the kernel that a new driver has become available.
    - A conflict-resolution mechanism allows different device drivers to reserve hardware resources and to protect those resources from accidental use by another driver.
  - Module Management
    - Loading a module requires more than just loading its binary contents into kernel memory.
      - The system must also make sure that any references the module makes to kernel symbols or entry points are updated to point to the correct locations in the kernel's address space.
    - The loading of the module is performed in two stages.
      - First, the module-loader utility asks the kernel to reserve a continuous area of virtual kernel memory for the module.
      - A second system call then passes the module, plus any symbol table that the new module wants to export, to the kernel.
    - The final module-management component si the module requester.
  - Driver Registration
    - A module may register many types of functionality. Registration tables include, among others, the following items:
      - Device drivers.
      - File systems.
      - Network protocols.
      - Binary format.
  - Conflict Resolution
    - Linux provides a central conflict-resolution mechanism to help arbitrate access to certain hardware resources.
      - To prevent modules from clashing over access to hardware resources
      - To prevent autoprobes - device-driver probes that auto-detect device configuration - from interfering with existing device drivers
      - To resolve conflicts among multiple drivers typing to access the same hardware

### 16.4: Process Management

- In this section, we review the traditional UNIX process model and introduce Linux's threading model.
- The fork() and exec() Process Model
  - The basic principle of UNIX process management is to separate into two steps two operations that are usually combined into one: the creation of a new process and the running of a new program.
    - A new process is created by the fork() system call, and a new program is run after a call to exec().
  - This model has the advantage of great simplicity.
  - Process identity
    - A process identity consists mainly of the following items:
      - Process ID (PID).
      - Credentials.
      - Personality.
        - Personalities are primarily used by emulation libraries to request that system calls be compatible with certain varieties of UNIX.
      - Namespace.
        - Each process is associated with a specific view of the file-system hierarchy, called its namespace.
  - Process Environment
    - A process's environment is inherited from its parent and is composed of two null-terminated vectors: the argument vector and the environment vector.
      - The argument vector simply lists the command-line arguments used to invoke the running program; it conventionaly starts with the name of the program itself.
      - The environment vector is a list of "NAME=VALUE" pairs that associates named environment variables with arbitrary textual values.
        - The environment is not held in kernel memory but is stored in the process's own user-mode address space as the first datum at the top of the process's stack.
    - The argument and environment vectors are not altered when a new process is created. The new child process will inherit the environment of its parent.
  - Process Context
    - The process identity and environment properties are usually set up when a process is created and not changed until that process exits.
    - In contrast, process context is the state of the running program at any one time; it changes constantly.
    - Process context includes the following parts:
      - Scheduling context.
        - The most important part of the process context is its scheduling context - the information that the scheduler needs to suspend and restart the process.
        - This information includes saved copies of all the process's registers.
      - Accounting.
      - File table.
        - The file table is an array of pointers to kernel file structures representing open files.
      - File-system context.
      - Signal-handler table.
        - The signal-handler table defines the action to take in response to a specific signal.
      - Virtual memory context.
        - The virtual memory context describes the full contents of a process's private address space.
  - Processes and Threads
    - Linux では process を複製する fork() の他に thread を作成する clone() を提供している。
    - The clone() system call behaves identically to fork(), except that it accepts as arguments a set of flags that dictate what resources are shared between the parent and child.
    - The arguments to the clone() system call tell it which subcontexts to copy and which to share.

### 16.5: Scheduling

- Scheduling is the job of allocating CPU time to different tasks within an operating system.
- Linux, like all UNIX systems, supports preemptive multitasking.
- Kernel tasks encompass both tasks that are requested by a running process and tasks that execute internally on behalf of the kernel itself, such as tasks spawned by Linux's I/O subsystem.
- Process Scheduling
  - Linux has two separate process-scheduling algorithms.
    - One is a time-sharing algorithm for fair, preemptive scheduling among multiple processes.
    - The other is designed for real-time tasks, where absolute priorities are more important than fairness.
  - The Linux scheduler is a preemptive, priority-based algorithm with two separate priority ranges: a real-time from 0 to 99 and a nice value ranging from -20 to 19.
    - Smaller nice values indicate higher priorities.
  - CFS (Completely Fair Scheduler) is a significant departure from the traditional UNIX process scheduler.
    - In the latter, the core variables in the scheduling algorithm are priority and time slice.
      - The time slice is the length of time - the slice of the processor - that a process is afforded.
      - CFS introduced a new scheduling algorithm caled fair scheduling that eliminates time slices in the traditional sense.
      - In stead of time slices, all processes are allotted a proportion of the processor's time.
    - To calculate the actual length of time a process runs, CFS relies on a configurable variable called target latency, which is the interval of time during which every runnable task should run at least once.
    - CFS consequently relies on a second configurable variable, the minimum granularity, which is a minimum length of time any process is allotted the processor.
- Real-Time Scheduling
  - Linux implements the two real-time scheduling classes required by POSIX.1b: first-come, first-served (FCFS) and round-robin.
  - Linux's real-time scheduling is soft - rather than hard - real time.
- Kernel Synchronization
  - The way the kernel schedules its own operations is fundamentally different from the way it schedules processes.
  - A request for kernel-mode execution can occur in two ways.
    - A running program may request an operating-system service, either explicitly via a system call or implicitly.
    - Alternatively, a device controller may deliver a hardware interrupt that causes the CPU to start executing a kernel-defined handler for that interrupt.
  - The Linux kernel provides spinlocks and semaphores for locking in the kernel.
  - Linux uses an interesting approach to disable and enable kernel preemption. It provides two simple kernel interfaces - preempt_disable() and preempt_enable().
  - However, there is a penalty for disabling interrupts.
    - More importantly, as long as interrupts remain disabled, all I/O is suspended, and any device waiting for servicing will have to wait until interrupts are reenabled.
  - Linux implements this architecture by separating interrupt service routines into two sections: the top half and the bottom half.
  - This separation means that the kernel can complete any complex processing being interrupted itself.
- Symmetric Multiprogramming
  - The Linux 2.0 was the first stable Linux kernel to support symmetric multiprocessor (SMP) hardware, allowing separate processes to execute in parallel on separate processors.

### 16.6: Memory Management

- Memory management under Linux has two components.
  - The first deals with allocating and freeing physical memory - pages, groups of pages, and small blocks of RAM.
  - The second handles virtual memory, which is memory-mapped into the address space of running processes.
- Management of Physical Memory
  - Due to specific hardware constraints, Linux separates physical memory into four different zones, or regions:
    - ZONE_DMA
    - ZONE_DMA32
    - ZONE_NORMAL
    - ZONE_HIGHMEM
  - The primary physical-memory manager in the Linux kernel is the page allocator.
    - 各 zone はそれぞれ allocator を持つ。
  - Ultimately、all memory allocations in the Linux kernel are made either statically, by drivers that reserve a contiguous area of memory during system boot time, or dynamically, by the page allocator.
  - The slab-allocation algorithm uses caches to store kernel objects.
  - In Linux, a slab may be in one of three possible states:
    - Full.
    - Empty.
    - Partial.
  - The slab allocator first attempts to satisfy the request with a free object in a partial slab. If none exists, a free object is assigned from an empty slab. If no empty slabs are available, a new slab is allocated from contiguous physical pages and assigned to a cache.
- Virtual Memory
  - The Linux virtual memory system is responsible for maintaining the address space accessible to each process. It creates pages of virtual memory on demand and manages loading those pages from disk and swapping them back out to disk as required.
  - Under Linux, the virtual memory manager maintains two separate views of a process's address space: as a set of separate regions and as a set of pages.
  - Virtual Memory Regions
    - Linux implements several types of virtual memory regions.
      - backed by nothing のものと backed by file のものがある。
    - The mapping of a region into the process's address space can be either private or shared.
- Lifetime of a Virtual Address Space
  - The kernel creates a new virtual address space in two situations: when a process runs a new program with the exec() system call and when a new process is created by the fork() system call.
- Swapping and Paging
  - The paging system can be divided into two sections.
    - First, the policy algorithm decides which pages to write out to disk and when to wirte them.
    - Second, the paging mechanism carries out the transfer and pages data back into physical memory when they are needed again.
- Kernel Virtual Memory
  - Linux reserves for its own internal use a constant, architecture-dependent region of the virtual address space of every process.
    - The page-table entries that map to these kernel pages are marked as protected, so that the pages are not visible or modifiable when the processor is running in user mode.
  - This kernel virtual memory area contains two regions.
    - The first is a static area that contains page-table references to every available physical page of memory in the system, so that a simple translation from physical to virtual addresses occurs when kernel code is run.
    - The remainder of the kernel's reserved section of address space is not reserved for any specific purpose.
- Execution and Loading of User Programs
  - The Linux kernel's execution of user programs is triggered by a call to the exec() system call.
  - This exec() call commands the kernel to run a new program within the current process, completely overwriting the current execution context with the initial context of the new program.
  - Mapping of Programs into Memory
    - Under Linux, the binary loader does not load a binary file into physical memory. Rather, the pages of the binary file are mapped into regions of virtual memory.
    - Once these mappings have been set up, the loader initializes the process's program-counter register with the starting point recorded in the ELF header, and the process can be scheduled.
  - Static and Dynamic Linking
    - Once the program has been loaded and has started running, all the necessary contents of the binary file have been loaded into the process's virtual address space.
    - However, most programs also need to run functions from the system libraries, and these library functions must also be loaded.
    - Such a program is statically linked to its libraries, and statically linked executable can commence running as soon as they are loaded.
    - Linux implements dynamic linking in user mode through a special linker library.

### 16.7: File Systems

- The Virtual File System
  - The Linux VFS designed around object-oriented principles. It has two components: a set of definitions that specify what file-system objects are allowed to look like and a layer of software to manipulate the objects.
  - The VFS defines four main object types:
    - An inode object represents an individual file.
    - A file object represents an open file.
    - A superblock object represents an entire file system.
    - A dentry object represents an individual directory entry.
- The Linux ext3 File System
  - The standard on-disk file system used by Linux is called ext3, for historical reasons.
    - もともと Linux は MINIX と互換性があるように作られていて、その時の file system が extended file system (extfs)。それが改良されて third extended file system で ext3。
  - The ext3 allocation policy works as follows:
    - As in FES, an ext3 file system is partitioned into multiple segments. In ext3, these are called block groups.
    - FES uses the similar concept of cylinder groups, where each group corresponds to a single cylinder of a physical disk.
- Journaling
  - The ext3 file system supports a popular feature called journaling, whereby modifications to the file system are written sequentially to a journal.
    - A set of operations that performs a specific task is a transaction.
    - Once a transaction is written to the journal, it is considered to be committed.
  - If the system crashes, some transaction may remain in the journal. Those transactions were never completed to the file system even though they were committed by the operating system, so they must be completed once the system recovers.
  - Journaling file systems may perform some operations faster than nonjournaling systems, as updates proceed much faster when they are applied to the in-memory journal rather than directory to the on-disk data structures.
- The Linux Process File System
  - The flexibility of the Linux VFS enables us to implement a file system that does not store data persistently at all but rather provides an interface to some other functionality. The Linux process file system, known as the /proc file system, is an example of a file system whose contents are not actually stored anywhere but are computed on demand according to user file I/O requests.
  - The /proc file system must implement two things: a directory structure and the file contents within.
    - The /proc file system must define a unique and persistent inode number for each directory and the associated files.

### 16.8: Input and Output

- To the user, the I/O system in Linux looks much like that ina any UNIX system. That is, to the extent possible, all device drivers appear as normal files.
- Linux splits all devices into three classes: block devices, character devices, and network devices.
  - Block devices include all devices that allow random access to completely independent, fixed-sized blocks of data, including hard disks and flopy disks, CD-ROMs and Blu-ray disks, and flash memory.
    - Block devices are typically used to store file systems, but direct access to a block device is also allowed so that programs can create and repair the file system that the device contains.
  - Character devices include most other devices, such as mice and keyboards.
    - The fundamental difference between block and character devices is random access - block devices are accessed randomly, while character devices are accessed serially.
  - Network devices are dealt with differently from block and character devices.
    - Users cannot directly transfer data to network devices.
    - Instead, they must communicate indirectly by opening a connection to the kernel's networking subsystem.

### 16.9: Interprocess Communication

- Synchronization and Signals
  - The standard Linux mechanism for informing a process that an event has occured is the signal.
    - Signals can be sent from any process to any other process, with restrictions on signals sent to processes owned by another user.
    - However, a limited number of signals are available, and they cannot carry information.
  - Internally, the Linux kernel does not use signals to communicate with processes running in kernel mode.
    - Rather, communication about incoming asynchronous events within the kernel takes place through the use of scheduling states and wait_queue structures.
  - Although signals have always been the main mechanism for communicating asynchronous events among processes, Linux also implements the semaphore mechanism of System V UNIX.
- Passing of Data among Processes
  - The standard UNIX pipe mechanism allows a child process to inherit a communication channel from its parent; data written to one end of the pipe can be read at the other.
  - Another process communication method, shared memory, offers an extremely fast way to communicate large or small amounts of data.
  - A shared-memory region in Linux is persistent object that can be created or deleted by processes.

### 16.10: Network Structure

- Not only does Linux support the standard Internet protocols used for most UNIX-to-UNIX communications, but it also implements a number of protocols native to other, non-UNIX operating systems.
- Internally, networking in the Linux kernel is implemented by three layers of software:
  - The socket interface
  - Protocol drivers
  - Network-device drivers
- User applications perform all networking requests through the socket interface.
- Whenever any networking data arrive at this layer, either from an application's socket or from a network-device driver, the data are expected to have been tagged with an identifier specifying which network protocol they contain.
- All communication between the layers of the networking stack is performed by passing single skbuff (socket buffer) structures.
- The most important set of protocols in the Linux networking system is the TCP/IP protocol suite.

### 16.11: Security

- The security concerns can be classified in two groups:
  - Authentication.
    - Making sure that nobody can access the system without first providing that she has entry rights.
  - Access control.
    - Providing a mechanism for checking whether a user has the right to access a certain object and preventing access to objects as required.
- Authentication
  - Authentication in UNIX has typically been performed through the use of a publicly readable password file.
  - A user's password is combined with a random "salt" value, and the result is encoded with a one-way transformation function and stored in the password file.
  - A new security mechanism has been developed by UNIX vendors to address authentication problems. The pluggable authentication modules (PAM) system is based on a shared libary that can be used by any system component that needs to authenticate users.
- Access Control
  - Access contorl under UNIX systems, including Linux, is performed through the use of unique numeric identifiers.
    - A user identifier (UID) identifies a single user or a single set of access rights.
    - A group identifier (GID) is extra identifier that can be used to identify rights belonging to more than one user.
  - Every object in a UNIX system under user and group access control has a single UID and a single GID associated with it. User object also have a single UID, but they may have more than one GID.
  - Linux performs access control by assigning objects a protection mask that specifies which access modes - read, write, or execute - are to be granted to processes with owner, group, or world access.
  - The only exception is the privileged root UID.

### 16.12: Summary

- これまでに書いている内容なので省略

## 17: Windows7

- Chapter Objectives
  - To explore the principles underlying Windows7's design and the specific components of the system.
  - To provide a detailed discussion of the Windows7 file system.
  - To illustrate the networking protocols supported in Windows7.
  - To describe the interface available in Windows7 to system and application programmers.
  - To describe the important algorithm implemented with Windows7.

### 17.1: History

- In the mid-1980s, Microsoft and IBM cooperated to develop the OS/2 operating system, which was written in assembly language for single-processor Intel 80286 systems.
- In October 2001, Windows XP was released as both an update to the Windows 2000 desktop operating system and a replacement for Windows 95/98.
  - Windows XP was the first version of Windows to ship a 64-bit version.
- Windows Vista への批判を受けて改善させたのが Windows7。2009 年 10 月に server editions と共にリリース。
  - Windows7 is a multiuser operating system, supporting simultaneous access through distributed services or through multiple instances of the GUI via the Windows terminal services.

### 17.2: Design Principles

- Microsoft's design goals for Windows included security, reliability, Windows and POSIX application compatibility, high performance, extensibility, portability, and international support.
- Windows7 を例に見ていく。
- Security
  - Windows7 security goals required more than just adherence to the design standards that had enabled Windows NT 4.0 to receive a C2 security classification from the U.S. government.
  - Windows bases security on discretionary access controls. System objects, including files, registry settings, and kernel objects, are protected by access-control lists (ACLs).
  - Other security measures include address-space layout randomization (ASLR), nonexecutable stacks and heaps, and encryption and digital signature facilities.
- Reliability
  - Its reliability increased due to such factors as maturity in the source code, extensive stress testing of thy system, improved CPU architectures, and automatic detection of many serious errors in drivers from both Microsoft and third parties.
  - One of the most significant improvements in the Windows experience came from adding memory diagnostics as an option at boot time.
  - Windows7 introduced a fault-tolerant memory heap.
  - Achieving high reliability in Windows is particularly challenging because almost one billion computers run Windows.
  - To cope with these challenges, Microsoft is increasingly relying on communications from customer machines to collect large amounts of data from the ecosystem.
- Windows and POSIX Application Compatibility
  - Application compatibility is difficult to achieve because many applications check for a particular version of Windows, may depend to some extent on the quirks of the implementation of APIs, may have latent application bugs that were masked in the previous system, and so forth.
  - Like Windows XP, Windows7 has a compatibility layer that sits between applications and the Win32 APIs.
  - As a final compatibility measure, several editions of Windows7 provide a virtual machine that runs Windows XP inside Windows7.
- High Performance
  - To satisfy performance requirements, NT used a variety of techniques, such as asynchronous I/O, optimized protocols for networks, kernel-based graphics rendering, and sophisticated caching of file-system data.
  - Windows XP further improved performance by reducing the code-path length in critical functions, using better algorithms and per-processor data structures, using memory coloring for non-uniform memory access (NUMA) machines, and implementing more scalable locking protocols, such as queued spinlocks.
  - By the time Windows7 was developed, several major changes had come to computing.
    - client/server computing や advanced local procedure call (ALPC)、CPU 数の増加など。
  - Other changes are due to the increasing importance of support for parallel computing.
    - To support task-based parallelism, Windows7 provides a new form of user-mode scheduling (UMS).
- Extensibility
  - Extensibility refers to the capacity of an operating system to keep up with advances in computing technology.
- Portability
  - An operating system is portable if it can be moved from one CPU architecture to another with relatively few changes.
    - Windows was designed to be portable.
  - Like the UNIX operating system, Windows is written primarily in C and C++.
  - The architecture-specific source code is relatively small, and there is very little use of assembly code.
  - Operating systems are sensitive not only to CPU architecture but also to CPU support chips and hardware boot programs.
    - The CPU and support chips are known as a chipset.
- International Support
  - Windows was designed for international and multinational use.
  - It provides support for different locales via the national-language-support (NLS) API.
- Energy Efficiency
  - Increasing energy efficiency for computers causes batteries to last longer for laptops and netbooks, saves significant operating costs for power and cooling of data centers, and contributions to green initiatives aimed at lowering energy comsumption by businesses and consumers.
  - Windows7 added some new strategies for saving energy.
    - The longer a CPU can stay unused, the more energy can be saved.
- Dynamic Device Support
  - Early in the history of the PC industry, computer configurations were fairly static.
  - In a contemporary PC, the situation has completely changed. PCs are designed to enable users to plug and unplug a huge host of peripherals all the time; external disks, thumb drivers, cameras, and the like are constantly coming and going.

### 17.3: System Components

- The main layers are the HAL, the kernel, and the executive, all of which run in kernel mode, and a collection of subsystems and services that run in user mode.
- The user mode subsystems fail into two categories: the environmental subsystems, which emulate different operating systems, and the protection subsystems, which provide security functions.
- One of the chief advantages of this type of architecture is that interactions between modules are kept simple.
- Hardware-Abstraction Layer (HAL)
  - The HAL is the layer of software that hides hardware chipset differences from upper levels of the operating system.
  - The HAL exports a virtual hardware interface that is used by the kernel dispatcher, the executive, and the device drivers.
- Kernel
  - The kernel layer of Windows has four main responsibilities:
    - thread scheduling
    - low-level processor synchronization
    - interrupt and exception handling
    - switching between user mode and kernel mode
  - The kernel is organaized according to object-oriented design principles.
  - Kernel Dispatcher
    - The kernel dispatcher provides the foundation for the executive and the subsystems.
    - Its main responsibilities are thread scheduling and context switching, implementation of synchronization primitives, timer management, software interrupts, and exception dispatching.
  - Threads and Scheduling
    - Like many other modern operating systems, Windows uses processes and threads for executable code.
    - There are six possible thread states: ready, standby, running, waiting, transition, and terminated.
    - The dispatcher uses a 32-level priority scheme to determine the order of thread execution.
      - 1 - 15 が variable class、16 - 31 が real-time class となり、数字が大きいものほど優先順位が高い。
    - When a thread's time quantum runs out, the clock interrupt queues a quantum-end deferred procedure call (DPC) to the processor.
    - Scheduling occurs when a thread enters the ready or wait state, when a thread terminates, or when an application changes a thread's priority or processor affinity.
    - Traditionally, operating-system schedulers used sampling to measure CPU utilization by threads.
  - Implementation of Synchronization Primitives
    - Key operating-system data structures are managed as objects using common facilities for allocation, reference counting, and security.
    - Dispatcher objects control dispatching and synchronization in the system.
      - The event object is used to record an event occurrence and to synchronize this occurrence with some action.
      - The mutant provides kernel-mode or user-mode mutual exclusion associated with the notion of ownership.
      - The mutex, available only in kernel mode, provides deadlock-free mutual exclusion.
      - The semaphore object acts as a counter or gate to control the number of threads that access a resource.
      - The timer object is used to keep track of time and to signal timeouts when operations take too long and need to be interrupted or when a periodic activity needs to be scheduled.
  - Software Interrupts: Asynchronous and Deferred Procedure Calls
    - The dispatcher implements two types of software interrupts: asynchronous procedure calls (APCs) and deferred procedure calls (DPCs).
      - An APC call breaks into an executing thread and calls a procedure.
      - DPCs are used to postpone interrupt processing.
  - Exceptions and Interrupts
    - The kernel dispatcher also provides trap handling for exceptions and interrupts generated by hardware or software.
    - When an exception occurs in kernel mode, the exception dispatcher simply calls a routine to locate the exception handler. If no handler is found, a fatal system error occurs, and the user is left with the infamous "blue screen of death" that signifies system failure.
    - When Windows fails to handle an exception, it may construct a description of the error that occurred and request permission from the user to send the information back to Microsoft for further analysis.
    - There are 32 interrupt request levels in Windows.
    - The kernel uses an interrupt-dispatch table to bind each interrupt level to a service routine.
  - Switching between User-Mode and Kernel-Mode Threads
    - What the programmer thinks of as a thread in traditional Windows is actually two threads: a use-mode thread (UT) and a kernel-mode thread (KT).
    - Windows7 modifies the behavior of the kernel layer to support user-mode scheduling of the UTs.
- Executive
  - The Windows executive provides a set of services that all environmental subsystems use. The services are grouped as follows:
    - object manager
    - virtual memory manager
    - process manager
    - advanced local procedure call facility
    - power managers
    - registry
    - booting
  - Object Manager
    - For managing kernel-mode entities, Windows uses a generic set of interfaces that are manipulated by user-mode programs. Windows call these entities objects, and the executive component that manipulates them is the object manager.
    - User-mode code accesses these objects using an opaque value called a handle, which is returned by manay APIs.
    - The object manager keeps track of two counts for each object: the number of handles for the object and the number referenced pointers.
    - The object manager maintains the Windows internal name space.
  - Virtual Memory Management
    - The executive component that manages the virtual address space, physical memory allocation, and paging is the virtual memory (VM) manager.
    - The design of the VM manager assumes that the underlying hardware supports virtual-to-physical mapping, a paging mechanism, and transparent cache coherence on multiprocessor systems, as well as allowing multiple page-table entries to map to the same physical page frame.
    - Key areas of the kernel-mode region that are not identical for all processes are the self-map, hyperspace, and session space.
    - The Windows VM manager uses a two-step process to allocate virtual memory.
      - The first step reserves one or more pages of virtual addresses in the process's virtual address space.
      - The second step commits the allocation by assigning virtual memory space.
    - Windows implements shared memory by defining a section object.
    - Virtual-to-physical address translation on IA-32
      - 最初の 2 bit が pointer、9 bit の PDEs (page-derectory entries) があって次の 9 bit が PTEs (page-table entries)。最後の 12 bit が page offset。
    - To improve performance, the VM manager maps the page-directory and PTE-table pages into the same contiguous region of virtual addresses in every process.
    - Windows manages physical memory by associating each physical page with one of seven states.
      - A free page is a page that has no particular content.
      - A zeroed page is a free page that has been zeroed out and is ready for immediate use to satisfy zero-on-demand faults.
      - A modified page has been written by a process and must be sent to the disk before it is allocated for another process.
      - A standby page is a copy of information already stored on disk.
      - A bad page is unusable because a hardware error has been detected.
      - A transition page is on its way in from disk to a page frame allocated in physical memory.
      - A valid page is part of the working set of one or more processes and is contained within these processes' page table.
  - Process Manager
    - The Windows process manager provides services for creating, deleting, and using processes, threads, and jobs.
    - Each process contains one or more threads. Processes themselves can be collected into larger units called job objects.
    - The process manager relies on the asynchronous procedure calls (APCs) implemented by the kernel layer.
    - The process manager also supports impersonation.
      - By default, all threads within a process share a common token, representing the user and the application that started the process.
  - Facilities for Client-Server Computing
    - The implementation of Windows uses a client-server model throughout.
    - The environmental subsystems are servers that implement particular operating system personalities.
    - The normal implementation paradigm for client-server computing is to use RPCs to communicate requests. The Win32 API supports a standard RPC protocol.
    - ALPC (advanced-local-procedure-call) is a message-passing mechanism.
      - 3 種類の message-pasing techniques がある。
  - I/O Manager
    - The I/O manager is responsible for managing file systems, device drivers, and network drivers.
    - It keeps track of which device drivers, filter drivers, and file systems are loaded, and it also manages buffers for I/O requests.
    - Filter drivers can examine and potentially modify each I/O operation.
      - mount managenent / partition management / disk striping / mirroring などは filter drivers を利用して実装される。
    - Device drivers for Windows are written to the Windows Driver Model (WDM) specification.
  - Cache Manager
    - In many operating systems, caching is done by the file system. Instead, Windows provides a centralized caching facility.
    - The cache manager works closely with the VM manager to provide cache services for all components under the control of the I/O manager.
    - To improve performance, the cache management keeps a small history of read requests and from this history attempts to predict future requests.
    - The cache manager is also responsible for telling the VM manager to flush the contents of the cache.
  - Security Reference Monitor
    - Whenever a process opens a handle to an object, the security reference monitor (SRM) checks the process's security token and the object's access-control list to see whether the process has the necessary access rights.
    - The SRM is also responsible for manipulating the privileges in security tokens.
    - Another responsibility of the SRM is logging security audit events.
  - Plug-and-Play Manager
    - The operating system uses the plug-and-play (PnP) manager to recognize and adapt to changes in the hardware configuration.
    - Many programs in the system are interested in the addition or removal of devices, so the PnP manager supports notification.
  - Power Manager
    - The power manager detects current system conditions, such as the load on CPUs or I/O devices, and improves energy efficiency by reducing the performance and responsiveness of the system when need is low.
    - The power manager can also put the entire system into a very efficient sleep mode and can even write all the contents of memory to disk and turn off the power to allow the system to go into hibernation.
    - Hibernation takes considerably longer because the entire contents of memory must be transferred to disk before the system is turned off.
    - Like the PnP manager, the power provides notifications to the rest of the system about changes in the power state.
  - Registry
    - Windows keeps much of its configuration information in internal databases, called hives, that are managed by the Windows configuration manager, which is commonly known as the registry.
    - Because the information in the system hive is required to boot the system, the registry manager is implemented as a component of the executive.
  - Booting
    - The booting of a Windows PC begins when the hardware powers on and firmware begins executing from ROM.
      - In older machines, this firmware was known as the BIOS, but more modern systems use UEFI (the Unified Extensible Firmware Interface), which is faster and more general and makes better use of the facilities in contemporary processors.
    - The firmware runs power-on self-test (POST) diagnostics; identifies many of the devices attached to the system and initializes them to a clean, power-up state; and then builds the description used by the advanced configuration and power interface (ACPL).
    - Next, the firmware finds the system disk, loads the Windows bootmgr program, and begins executing it.
    - The kernel initializes itself and creates two processes.
      - The system process contains all the internal kernel worker threads and never executes in user mode.
      - The first user-mode process created is SMSS, for session manager subsystem, which is similar to the INIT process in UNIX.

### 17.4: Terminal Services and Fast User Switching

- Windows supports a GUI-based console that interfaces with the user via keyboard, mouse, and display. Most systems also support audio and video.
- Windows7 added support for multi-touch hardware, allowing users to input data by touching the screen and making gestures with one or more fingers.
- Modern Windows, however, supports the sharing of a PC among multiple users.
  - Each user that is logged on using the GUI has a session created to represent the GUI environment he well be using and to contain all the processes created to run his applications.
- The terminal server (TS) connects one of the GUI windows in a user's local session to the new or existing session, called a remote desktop, on the remote computer.
  - The most common use of remote desktops is for users to connect to a session on their work PC from their home PC.

### 17.5: File System

- The native file system in Windows is NTFS (NT File System). It is used for all local volumes.
- NTFS Internal Layout
  - The fundamental entity in NTFS is a volume. A volume is created by the Windows logical disk management utility and is based on a logical disk partition.
  - NTFS does not deal with individual sectors of a disk but instead uses clusters as the units of disk allocation.
    - A cluster is a number of disk sectors that is a power of 2.
  - NTFS uses logical cluster numbers (LCNs) as disk addresses.
  - A file in NTFS is not a simple byte stream as it is in UNIX; rather, it is a structured object consisting of typed attributes.
  - Each file in an NTFS volume has a unique ID called a file reference.
  - NTFS B+ Tree
    - As in UNIX, the NTFS namespace is organaized as a hierarchy of directories. Each directory uses a data structure caled a B+ tree to store an index of the file names in that directory.
  - NTFS Metadata
    - The NTFS volume's metadata are all stored in files. The first file is the MFT (master file table). The second file, which is used during recovery if the MFT is damaged, contains a copy of the first 16 entries of the MFT.
    - The next few files are also special in purpose.
      - log file
      - volume file
      - attribute-definition table
      - root directory
      - bitmap file
      - boot file
      - bad-cluster file
- Recovery
  - In many simple file systems, a power failure at the wrong time can damage the file-system data structures so severely that the entire volume is scrambled.
  - In NTFS, all file-system data-structure updates are performed inside transactions.
  - After a crash, the system can restore the file-system data structures to a consistent state by processing the log records, first redoing the operations for committed transactions and then undoing the operations for transactions that did not commit successfully before the crash.
  - The logging functionality is provided by the log-file service.
- Security
  - Each NTFS file references a security descriptor, which specifies the owner of the file, and an access-control list, which contains the access permissions granted or denied to each user or group listed.
- Volume Management and Fault Tolerance
  - Volume Sets and RAID Sets
  - Sector Sparing and Cluster Remapping
    - Sector sparing is a hardware capability provided by many disk drives.
    - Cluster remapping is a software technique performed by the file system.
- Compression
  - To compress a file, NTFS divides the file's data into compression units, which are blocks of 16 contiguous clusters.
  - For sparse files or files that contain mostly zeros, NTFS uses another technique to save space.
- Mount Points, Symbolic Links, and Hard Links
  - Mount points are a form of symbolic link specific to directories on NTFS that were introduced in Windows 2000.
  - They provide a mechanism for organizing disk volumes that is more flexible than the use of global names.
- Change Journal
  - NTFS keeps a journal describing all changes that have been made to the file system.
- Volume Shadow Copies
  - Windows implements the capability of bringing a volume to a known state and then creating a shadow copy that can be used to back up a consistent view of the volume.
    - The technique is known as snapshots in some other file systems.

### 17.6: Networking

- Windows supports both peer-to-peer and client-server networking.
- The networking components in Windows provide data transport, interprocess communication, file sharing across a network, and the ability to send print jobs to remote printers.
- Network Interfaces
  - To describe networking in Windows, we must first mention two of the internal networking interfaces: the network device interface specification (NDIS) and the transport driver interface (TDI).
    - NDIS resides at the interface between data-link and network layers in the ISO model and enables many protocols to operate over many different network adapters.
    - The TDI is the interface between the transport layer (layer 4) and the session layer (layer 5). This interface enables any session-layer component to use any available transport mechanism.
- Protocols
  - Windows implements transport protocols as drivers. These drivers can be loaded and unloaded from the system dynamically, although in practice the system typically has to be rebooted after a change.
  - Windows come with several networking protocols. Next, we discuss a number of these protocols.
  - Server-Message Block
    - The server-message-block (SMB) protocol was first introduced in MS-DOS 3.1.
    - The SMB protocol has four message types.
      - Session control messages.
      - File messages.
      - Printer messages.
      - Message messages.
    - A version of the SMB control was published as the common Internet file system (CIFS) and is supported on a number of operating systems.
  - Transmisson Control Protocol/Internet Protocol (TCP/IP)
    - The TCP/IP suite that is used on the Internet has become the de fact standard networking infrastructure.
    - Windows provides a software firewall that limits the TCP ports that can be used by programs for network communication.
  - Point-to-Point Tunneling Protocol (PPTP)
    - The PPTP is a protocol provided by Windows to communicate between remote-access server modules running on Windows server machines and other client systems that are connected over the Internet.
    - The remote-access servers can encrypt data sent over the connection, and they support multiprotocol virtual private networks (VPNs) over the Internet.
  - HTTP Protocol
    - The HTTP protocol is used to get/put information using the World Wide Web.
  - Web-Distributed Authoring and Versioning Protocol (WebDAV)
    - WebDAV is an HTTP-based protocol for collaborative authoring across a network.
  - Named Pipes
    - Named pipes are a connection-oriented messaging mechanism.
    - A process can use named pipes to communicate with other processes on the same machine.
  - Remote Procedure Calls
    - A remote procedure call (RPC) is a client-server mechanism that enables an application on one machine to make a procedure call to code on another machine.
    - The Windows RPC mechanism follows the widely used distributed-computing-environment standard for RPC messages, so programs written to use Windows RPCs are highly portable.
  - Component Object Model
    - The component object model (COM) is a mechanism for interprocess communication that was developed for Windows.
- Redirectors and Servers
  - In Windows, an application can use the Windows I/O API to access files from a remote computer as though they were local, provided that the remote computer is running a CIFS server such as those provided by Windows.
  - A redirector is the client-side object that forwards I/O requests to a remote system, where they are satisfied by a server.
  - For performance and security, the redirectors and servers run in kernel mode.
  - Distributed File System
    - Windows supports a distributed file system (DFS) protocol that allows a network administrator to serve up files from multiple servers using a single distributed name space.
  - Folder Redirection and Client-Side Caching
    - To improve the PC experience for users who frequently switch among computers, Windows allow administrators to give users roaming profiles, which keep user's preferences and other settings on servers.
    - Folder redirection is then used to automatically store a user's documents and other files on a server.
  - Domains
    - To manage the global access rights within such groups, Windows uses the concept of a domain.
    - Specifically, a Windows domain is a group of Windows workstations and servers that share a common security policy and user database.
  - Active Directory
    - Active Directory is the Windows implementation of lightweight directory-access protocol (LDAP) services.

### 17.7: Programmer Interface

- The Win32 API is th fundamental interface to the capabilities of Windows.This section describes five main aspects of the Win32 API.
- Access to Kernel Objects
  - The Windows kernel provides many services that application programs can use.
- Sharing Objects between Processe
  - Windows provides three ways to share objects between processes.
    - The first way is for a child process to inherit a handle of the object.
    - The second way to share objects is for one process to give the object a name when the object is created and for the second process to open the name.
    - The third way to share objects is via the DuplicateHandle() function.
- Process Management
  - In Windows, a process is a loaded instance of an application and a thread is an executable unit of code that can be scheduled by the kernel dispatcher.
  - Scheduling Rule
    - Priorities in the Win32 environment are based on the native kernel (KT) scheduling model, but not all priority values may be chosen. The Win32 API uses four priority classes.
      - IDLE_PRIORITY_CLASS (NT priority level 4)
      - NORMAL_PRIORITY_CLASS (NT priority level 8)
      - HIGH_PRIORITY_CLASS (NT priority level 13)
      - REAL_TIME_PRIORITY_CLASS (NT priority level 24)
  - Thread Priorities
    - A thread starts with an initial priority determined by its class. The priority can be altered by the SetThreadPriority() function.
      - -2〜+2 の範囲で base priority に加算
  - Thread Suspend and Resume
    - A thread can be created in a suspended state or can be placed in a suspended state later by use of the SuspendThread() function.
  - Thread Synchronization
    - To synchronize concurrent access to shared objects by threads, the kernel provides synchronization objects, such as semaphores and mutexes.
  - Thread Pools
    - Repeatedly creating and deleting threads can be expensive for applications and services that perform small amounts of work in each instantiation.
    - The Win32 thread pool provides user-mode programs with three services.
    - The goal of using a thread pool is to increase performance and reduce memory footprint.
  - Fibers
    - A fiber is user-mode code that is scheduled according to a user-defined scheduling algorithm.
    - fiber は協調的マルチタスク
  - User-Mode Scheduling (UMS) and ConcRT
    - A new mechanism in Windows7, user-mode scheduling (UMS), addresses several limitations of fibers.
    - Unlike fibers, UMS is not intended to be used directly by the programmer. The details of writing user-mode schedulers can be very challenging, and UMS does not include such a scheduler.
    - Microsoft Visual Studio 2010 shipped with Concurrency Runtime (ConcRT), a concurrent programming framework for C++. ConcRT provides a user-mode scheduler together with facilities for decomposing programs into tasks, which can then be scheduled on the available CPUs.
  - Winsock
    - Winsock is the Windows socket API.
    - Winsock is a session-layer interface that is largely compatible with UNIX sockets but has some added Windows extensions.
    - Winsock follows the Windows Open System Architecture (WOSA) model, which provides a standard service provider interface (SPI) between applications and networking protocol layers.
- Interprocess Communication Using Windows Messaging
  - Win32 applications handle interprocess communication in several ways.
    - One way is by using shared kernel objects.
    - Another is by using the Windows messaging facility, an approach that is particularly popular for Win32 GUI applications.
- Memory Management
  - The Win32 API provides several ways for an application to use memory: virtual memory, memory-mapped files, heaps, and thread-local storage (TLS).

### 17.8: Summary

- これまでに書いている内容なので省略

## 18: Influential Operating Systems

- Chapter Objectives
  - To explain how operating-system features migrate over time from large computer systems to smaller ones.
  - To discuss the features of several historical important operating systems.

### 18.1: Feature Migration

- One reason to study early architectures and operating systems is that a feature that once ran only on huge systems may eventually make its way into very small systems.
  - Figure 18.1 がわかりやすい。

### 18.2: Early Systems

- Before the 1940s, computing devices were designed and implemented to perform specific, fixed tasks.
- All that changed in the 1940s when Alan Turing and John von Neumann, both separately and together, worked on the idea of a more general-purpose stored program computer.
- It is likely that the first working stored-program general-purpose computer was the Manchester Mark 1, which run successfully in 1949.
- Early computers were physically enormous machines run from consoles.
- Dedicated Computer Systems
  - Libraries of common functions were crated. Common functions could then be copied into a new program without having to be written again, providing software reusability.
  - A significant amount of setup time could be involved in the running of a job.
- Shared Computer Systems
  - The solution was twofold.
    - First, a professional computer operator wa hired.
      - operator は setup できるが debug はできない、programmer はその逆のイメージ。
    - Second, jobs with similar needs were batched together and run through the computer as a group to reduce setup time.
  - To overcome this idle time, people developed automatic job sequencing.
  - The problem is the speed of the mechanical I/O devices, which are intrinsically slower than electronic devices.
- Overlapped I/O
  - One common solution to the I/O problem was to replace slow card readers (input devices) and line printers (output devices) with magnetic-tape units.
  - Although off-line preparation of jobs continued for some time, it was quickly replaced in most systems.
  - In a disk system, cards are read directly from the card reader onto the disk.
  - spooling は Simultaneous Peripheral Operation On-Line の頭文字から来ている。
  - Spooling overlaps the I/O of one job with the computation of other jobs.

### 18.3:Atlas

- The Atlas operating system was designed at the University of Manchester in England in the late 1950s and early 1960s.
- The most remarkable feature of Atlas, however, was its memory management.
  - Core memory was new and expensive at the time.
- The Atlas system used a drum for its main memory, but it had a small amount of core memory that was used as a cache for the drum.

### 18.4: XDS-940

- The XDS-940 operating system was designed at the University of California at Berkeley in the early 1960's.
  - Like the Atlas system, it was used paging for memory management.
  - Unlike the Atlas system, it was a time-shared system.
- A system-call instruction was added to the user-mode instruction set.

### 18.5: THE

- The THE operating system was designed at the Technische Hogeschool in Eindhoven in the Netherlands in the mid-1960's.
- The mainly noted for its clean design, particularly its layer structure, and its use of a set of concurrent processes employing semaphores for synchronization.
- Unlike the processes in the XDS-940 system, the set of processes in THE system was static.
- A priority CPU-scheduling algorithm was used.

### 18.6: RC 4000

- The RC 4000 system, like the THE system, was notable primarily for its design concepts.
- The goal was to create an operating-system nucleus, or kernel, on which a complete operating system could be built.
- The kernel supported a collection of concurrent processes.
- A round-robin CPU scheduler was used.
- Although processes could share memory, the primary communication and synchronization mechanism was the message system provided by the kernel.
- A message queue was associated with each process.
- I/O devices were also threated as processes.

### 18.7: CTSS

- The Compatible Time-Sharing System (CTSS) was designed at MIT as an experimental time-sharing system and first appeared in 1961.
- CTSS was extremely successful and was in use as late as 1972.
- Although it was limited, it succeeded in demonstrating that time sharing was a convenient and practical mode of computing.

### 18.8: MULTICS

- The MULTICS operating system was designed from 1965 to 1970 at MIT as a natural extension of CTSS.
- It was extended to a multiprocessor system, allowing a CPU to be taken out of service for maintenance while the system countinued running.

### 18.9: IBM OS/360

- The longest line of operating-system development is undoubtedly that of IBM computers.
- These systems were developed separately, often at independent sites. As a result, IBM was faced with many different computers, with different languages and different system software.
- The IBM/360 - which first appeared in the mid 1960's - was designed to alter this situation.
- The IBM/360 was designed as a family of computers spanning the complete range from small business machines to large scientific machines.

### 18.10: TOPS-20

- Perhaps the most influential of DEC's operating systems was TOPS-20.
- TOP-20 had an advanced command-line interpreter that provided help as needed to users.

### 18.11: CP/M and MS/DOS

- An early "standard" operating system for these computers of the 1970s was CP/M, short for Control Program/Monitor, written by Gary Kindall of Digital Research, Inc.
- CP/M ran primarily on the first "personal computer" CPU, the 8-bit Intel 8080.
- When IBM entered the personal computer business, it decided to have Bill Gates and company write a new operating system for its 16-bit CPU of choice - the Intel 8086.
  - This operating system, MS-DOS, was similar to CP/M but had a richer set of built-in commands, again mostly modeled after TOPS-10.

### 18.12: Macintosh Operating System and Windows

- The Apple Macintosh computer was arguably the first computer with a GUI designed for home users.
- It was certainly the most successful for the while, starting at its launch in 1984.
- The desktop rivalry between Apple and Microsoft continue today, with new versions of Windows and Mac OS trying to outdo each other in features, usability, and application functionality.

### 18.13: Mach

- The Mach operating system traces its ancestry to the Accent operating system developed at Carnegie Mellon University (CMU).
- Mach's development followed an evolutionary path from BSD UNIX systems.
- Unlike UNIX, which was developed without regard for multiprocessing, Mach incorporates multiprocessing support throughout.

### 18.14: Other Systems

- History is littered with operating systems that suited a purpose for a time and then, when faded, were replaced by operating systems that had more features, supported newer hardware, were easier to use, or were better marketed.
- We are sure this trend will continue in the future.
