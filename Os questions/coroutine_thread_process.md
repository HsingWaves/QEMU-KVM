## 1. Differences and Relationships Between **Process**, **Thread**, and **Coroutine**

| Aspect                | Process                                                      | Thread                                                       | Coroutine                                                    |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Definition**        | The basic unit of **resource allocation and ownership**      | The basic unit of **program execution**                      | A **user-mode lightweight thread**, the basic unit of **intra-thread scheduling** |
| **Context Switching** | Saves and restores CPU context (stack, registers), page tables, file descriptors, etc.; sets up a new process CPU environment | Saves and restores program counter, a small set of registers, and stack contents | Saves register context and stack first, and restores them when resumed |
| **Who Switches**      | Operating system                                             | Operating system                                             | User (application/runtime)                                   |
| **Switch Path**       | User mode → Kernel mode → User mode                          | User mode → Kernel mode → User mode                          | User mode only (no kernel trap)                              |
| **Call Stack**        | Kernel stack                                                 | Kernel stack                                                 | User stack                                                   |
| **Owned Resources**   | CPU resources, memory resources, files, handles, etc.        | Program counter, registers, stack, and status                | Own register context and stack                               |
| **Concurrency Model** | Different processes switch to achieve concurrency; each process may run in parallel on different CPUs | Multiple threads within one process execute concurrently     | At a given time only **one coroutine executes**; others are suspended—suited for splitting tasks |
| **System Overhead**   | High: virtual address space switch, kernel stack & hardware context switch, CPU cache invalidation, page table switch | Lower: only small register state needs saving/restoring      | Very low: no kernel context switch; often lock-free access to shared data → very fast switching |
| **Communication**     | Requires OS support (e.g., IPC)                              | Threads can directly access process data (e.g., globals)     | Shared memory, message queues                                |

------

### Summary Notes

1. **Process**
   - The basic unit of **resource allocation**.
   - Running an executable program creates one or more processes.
   - A process is essentially a *running instance* of a program.
2. **Thread**
   - The basic unit of **CPU scheduling** and **program execution**.
   - A lightweight process.
   - Each process has exactly **one main thread**.
   - Threads depend on the process: when the main thread exits, the process ends.
3. **Coroutine**
   - A **user-mode lightweight thread**.
   - The basic unit of **intra-thread scheduling** (cooperative scheduling).