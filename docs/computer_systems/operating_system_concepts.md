# 概要

## 本

- [Operating System Concepts | Abraham Silberschatz, Peter B. Galvin, Greg Gagne](https://amzn.to/2CN0bst)

## かかった時間

- x 時間

## 読む前の状態

- [作って理解するOS](create_and_understand_os.md) だけ読んだが、まだ理解の怪しい部分があったので、理解を深めようと読むことにした

## 読む前後の変化

- xxx

# 読書メモ

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

- In this section, we explore the mechanisms involved in creating processes and illustrate process creation on UNIX and Windowws systems.
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
