# Memory Management and Process Execution

## 30. Why does memory swapping occur?

**Reasons for Swapping**
- Physical memory (RAM) is limited.
- Too many processes or memory-hungry applications.
- Inactive pages are swapped out to free memory for active ones.

**Typical Triggers**
- High memory pressure
- Many concurrent processes
- Large working sets

**Downside**
- Disk is much slower than RAM.
- Heavy swapping leads to thrashing and poor system performance.

---

## 31. How does a terminal start a process? How does the process run?

**Step 1: User runs a command in the terminal**
- Example: `./app`

**Step 2: Shell creates a new process**
- Uses `fork()` to create a child process.

**Step 3: Load program into memory**
- The child process calls `execve()` to replace its memory image with the new program.

**Step 4: OS prepares the process**
- Loads executable and shared libraries.
- Sets up stack, heap, and virtual memory mappings.
- Initializes file descriptors and environment variables.

**Step 5: Process starts execution**
- The CPU begins executing at the program entry point (`_start`).
- Runtime initializes and then calls `main()`.

**Step 6: Process runs and exits**
- The process runs user code.
- On exit, the OS reclaims memory and resources.