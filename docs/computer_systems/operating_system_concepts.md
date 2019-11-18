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
  - Each process is represented in the operating systems by a process control block - also caled a task control block.
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
