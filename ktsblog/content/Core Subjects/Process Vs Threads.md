+++
title = 'Process vs Threads'
date = 2024-08-20T23:57:15+05:30
draft = false
series = 'Operating System'
tags =['process','threads','os','interview']
toc = false
+++

## Process Vs Thread [Popular Interview Question]

A process is an independent unit of execution with its own memory space, while a thread is a lightweight unit of execution that shares memory space with other threads within the same process.

### Process

A process is an independent, self-contained unit of execution that has its own memory space, including the code segment, data segment, heap, and stack.

1. **Address Space**: Each process operates in its own virtual address space, meaning it has its own separate memory layout and cannot directly access the memory of another process without inter-process communication (IPC).

2. **Resource Allocation**: Processes are heavyweight entities as they require a significant amount of overhead to create, manage, and destroy. Each process has its own set of resources like file descriptors, memory pages, and other system resources.

3. **Isolation**: Processes are isolated from each other. This isolation is enforced by the operating system, which prevents one process from corrupting another process’s memory or resources.

4. **Context Switching**: The switch between processes, known as context switching, involves saving and loading the state of the process, including its CPU registers, memory map, and other resources, which makes context switching between processes relatively expensive.

### Thread

A thread, also known as a lightweight process (LWP), is the smallest execution unit within a process. A single process can have multiple threads, all of which share the same memory space but execute independently.

1. **Shared Memory**: Threads within the same process share the same address space, which includes the code segment, data segment, and heap. However, each thread has its stack and registers.

2. **Resource Sharing**: Threads are more lightweight than processes because they share most of their resources with other threads within the same process. This includes open files, signal handlers, and memory.

3. **Efficiency**: Since threads within the same process share memory and resources, **context switching** between threads is faster and more efficient compared to processes. The overhead is lower as there’s no need to switch the memory map or reallocate resources.

4. **Communication**: Threads can communicate with each other more efficiently through shared memory because they reside in the same address space. However, this also means that threads can potentially corrupt each other’s memory if not managed properly.

### Key Differences

1. **Isolation**:

   - Processes are _isolated_, while threads share the same memory space within a process.
   - **Resource Usage**: Processes are _heavyweight_ and require more resources to manage, whereas threads are _lightweight_ and share most resources.

2. **Context Switching**:
   - Switching between processes is more _costly_ than switching between threads due to the need to manage separate memory spaces and resources.
