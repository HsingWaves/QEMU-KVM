## 3. How many threads can a process create, and what does it depend on?

This depends on the operating system and system architecture.

- **On a 32-bit system**:
   The user-mode virtual address space is typically about **3 GB**.
   If each thread allocates **10 MB of stack space**, then a single process can create at most **around 300 threads**.
- **On a 64-bit system**:
   The user-mode virtual address space can be extremely large (e.g., **128 TB**).
   In theory, the number of threads is **not limited by virtual address space**, but instead by **system parameters** (such as OS limits) or **performance constraints**.

Additionally, creating too many threads wastes a large amount of time on **context switching**, which negatively affects program performance.
 Creating unnecessary threads is usually counterproductive and should be avoided.

------

## 4. What is the difference between interrupts and exceptions?

- **Interrupts** are caused by **events external to the CPU instruction stream**.
   Examples include:

  - I/O completion interrupts (indicating that a device has finished input/output processing)
  - Timer interrupts
  - Control panel or hardware interrupts

  Interrupts notify the CPU that an external event has occurred and allow it to handle the event accordingly.

- **Exceptions** are caused by **internal events during instruction execution**.
   Examples include:

  - Illegal instructions
  - Address out-of-bounds (segmentation faults)
  - Arithmetic overflow

  Exceptions occur as a direct result of executing specific CPU instructions.



## 5. How much do you know about process–thread models?

Understanding processes and threads reflects a system’s design philosophy and control capability.
 Their meaning goes far beyond “threads are scheduling units” and “processes are resource containers”.

------

## Multithreading (within a process)

- A **process may contain multiple threads**
- All threads in the same process:
  - Share the **same address space**
  - Share **global variables and heap**
  - Have **independent stacks** and **register contexts**

Example:

```
int i = 10;
i++;   // NOT atomic
```

This operation involves:

1. Load from memory
2. Modify
3. Store back

Because threads can be preempted at any step, **race conditions** may occur.

### Key multithreading problems

- **No guaranteed execution order** between threads
- **Concurrent access to shared variables**
- Requires synchronization (mutexes, atomics, barriers)

------

## Why still use threads?

Despite complexity, threads offer:

- Lower creation and context-switch cost than processes
- Better resource sharing
- Natural modeling of concurrent tasks

Example:

- A QQ/IM client:
  - One thread for receiving messages
  - One thread for sending messages
  - One thread for UI

------

## Thread properties and lifecycle (POSIX threads)

### Thread creation

```
pthread_create(pthread_t *tid,
               const pthread_attr_t *attr,
               void *(*start_routine)(void *),
               void *arg);
```

- `tid`: thread identifier
- `attr`: thread attributes (stack size, detach state, etc.)
- `start_routine`: entry function
- `arg`: parameter passed to the thread

### Thread termination

- `pthread_exit(void *retval)`
- `pthread_join(pthread_t tid, void **retval)`
- `pthread_detach(pthread_t tid)`

Detached threads reclaim resources automatically when they exit.

------

## Thread attributes (`pthread_attr_t`)

Common attributes include:

- Detach state
- Scheduling policy
- Stack address
- Stack size
- Guard size

Attributes can be queried or set via:

```
pthread_attr_get*
pthread_attr_set*
```

------

## Processes (multiprocessing model)

### Process as a resource container

A process includes:

- Code segment (text)
- Data segment
- Heap
- Stack
- File descriptors
- Virtual address space

Processes **do not share memory by default**.

------

## Process creation

```
pid_t fork(void);
```

- Parent receives child PID (> 0)
- Child receives 0

After `fork()`:

- Parent and child have **separate address spaces**
- Uses **copy-on-write** optimization

To run a new program:

```
execve(...)
```

Shell behavior:

```
shell → fork → exec
```

------

## Process termination

Normal exit:

- `exit(status)`
- `_exit(status)`
- `return` from `main`

Abnormal exit:

- `abort()`
- fatal signals

Key difference:

- `exit()` flushes stdio buffers
- `_exit()` does not

------

## Process control APIs

- Get PID:

  ```
  getpid()
  getppid()
  ```

- Wait for child:

  ```
  wait()
  waitpid()
  ```

------

## Linux process internals

### Virtual address space

- Each process has a **private virtual address space**
- 32-bit systems typically allow ~3 GB user space
- Kernel space is protected and inaccessible from user mode

------

### Process Control Block (PCB)

Kernel maintains a PCB containing:

- Process state
- Registers
- Scheduling info
- Memory mappings
- Open files

------

### Context switching

A context switch saves and restores:

- CPU registers
- Program counter
- Stack pointer
- Memory management state

Excessive context switching reduces performance.