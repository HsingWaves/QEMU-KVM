## Inter-Process Communication (IPC) in Linux

**Common IPC mechanisms:**

1. **Pipes**
   - **Anonymous pipes**
     - Half-duplex (one-way communication)
     - Only usable between related processes (e.g., parent–child)
   - **Named pipes (FIFO)**
     - Half-duplex
     - Can be used between unrelated processes
     - FIFO order (first-in, first-out)
2. **Shared Memory**
   - Maps a memory region that multiple processes can access
   - Created by one process, accessed by many
   - Fastest IPC method
   - Usually combined with **semaphores or mutexes** for synchronization
3. **Message Queues**
   - Kernel-managed linked list of messages
   - Identified by a queue ID
   - Overcomes limitations of signals (too little data) and pipes (byte stream, buffer size limits)
4. **Sockets**
   - Used for communication between processes on different machines
   - Can also be used for local IPC
5. **Signals**
   - Used to notify a process that an event occurred
   - Example: `Ctrl + C` sends a signal (`SIGINT`)
6. **Semaphores**
   - A counter used to control access to shared resources
   - 

## Synchronization Mechanisms in Linux

1. **POSIX Semaphores**
   - Can be used for **process synchronization**
   - Can also be used for **thread synchronization**
2. **POSIX Mutex + Condition Variables**
   - Used for **thread synchronization only**
   - Typical pattern: mutex protects shared data, condition variable handles waiting/signaling

### 1. FCFS (First-Come, First-Served)

- Non-preemptive
- Processes are scheduled in arrival order
- **Pros:** Simple, good for long jobs
- **Cons:** Short jobs may wait a long time (convoy effect)

------

### 2. SJF (Shortest Job First)

- Non-preemptive
- The job with the shortest estimated run time runs first
- **Pros:** Minimizes average waiting time
- **Cons:** Long jobs may starve if short jobs keep arriving
- Requires knowing/estimating job length

------

### 3. SRTN (Shortest Remaining Time Next)

- Preemptive version of SJF
- A newly arrived job can preempt the current one if its remaining time is shorter
- **Pros:** Better responsiveness
- **Cons:** More context switches, starvation risk

------

### 4. Round Robin (RR)

- Preemptive
- Each process gets a fixed **time slice (quantum)**
- When the time slice expires, the process is moved to the end of the ready queue
- **Trade-off:**
  - Small time slice → high overhead (too many context switches)
  - Large time slice → poor real-time responsiveness

------

### 5. Priority Scheduling

- Each process has a priority
- Higher priority processes run first
- **Problem:** Low-priority processes may starve
- **Solution:** Aging (gradually increase priority of waiting processes)

------

## 6. Multilevel Feedback Queue (MLFQ)

- Designed for processes that need **multiple time slices**
- Multiple ready queues with different:
  - Priorities
  - Time slice lengths (e.g., 1, 2, 4, 8, …)
- New processes start in the highest-priority queue
- If a process uses up its time slice, it is moved to a lower-priority queue
- CPU always schedules from the highest non-empty queue

**Key idea:**

> Combines **Round Robin + Priority Scheduling**
>  Reduces context switches for long-running jobs
>  Improves responsiveness for interactive/short jobs