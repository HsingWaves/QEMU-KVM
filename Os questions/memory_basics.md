# Memory Basics (Virtual Memory, Cache, and Physical Memory)

## 25. What is virtual address vs physical address?

**Virtual Address**
- The address used by a process.
- Provided by the operating system as an abstraction of memory.
- Each process has its own virtual address space.
- Translated to physical addresses by the MMU (Memory Management Unit).

**Physical Address**
- The real address in physical RAM.
- Shared among all processes.
- Not directly visible to user programs.

**Key Difference**
- Virtual addresses provide isolation and protection.
- Physical addresses represent actual hardware memory locations.

---

## 26. What is cache coherence? What problems does it solve?

**Cache Coherence**
- Ensures that multiple CPU cores see a consistent view of memory.
- If one core modifies a memory location, other cores must see the updated value.

**Problems Solved**
- Prevents stale data being read by other cores.
- Avoids inconsistent views of shared memory.
- Enables correct behavior of multi-threaded programs.

**Common Protocols**
- MESI (Modified, Exclusive, Shared, Invalid)
- MOESI

---

## 27. What is a memory barrier (memory fence)? What does it do?

**Memory Barrier / Fence**
- A CPU instruction that enforces ordering of memory operations.
- Prevents the CPU or compiler from reordering loads and stores across the barrier.

**Why It Is Needed**
- Modern CPUs and compilers reorder instructions for performance.
- Barriers ensure correctness in concurrent programs.

**Types**
- Load barrier
- Store barrier
- Full barrier

---

## 28. What is memory swapping? What are its characteristics?

**Memory Swapping**
- The OS moves inactive memory pages from RAM to disk (swap space).
- Frees RAM for active processes.

**Characteristics**
- Extends apparent memory capacity.
- Much slower than RAM.
- Excessive swapping causes performance degradation (thrashing).

---

## 29. What is memory paging? What are its characteristics?

**Memory Paging**
- Memory is divided into fixed-size pages.
- Virtual pages are mapped to physical frames.

**Characteristics**
- Enables virtual memory.
- Supports demand paging (load pages only when needed).
- Reduces external fragmentation.
- Requires page tables and TLB for fast translation.