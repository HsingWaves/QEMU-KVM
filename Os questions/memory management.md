# Memory Management Summary (TLB, Swapping, Allocation, Address Translation)

## 1. Address Translation with TLB (Translation Lookaside Buffer)

### Without TLB
Logical Address → Page Table → Physical Address  
Each memory access requires:
- 1 access to page table
- 1 access to physical memory  
Total = 2 memory accesses

### With TLB
Logical Address → TLB lookup → (Hit) Physical Address  
- TLB Hit: 1 memory access  
- TLB Miss: Page Table lookup + memory access  

### Average Access Time (Example)
Assume:
- TLB access = 1 μs  
- Memory access = 100 μs  
- TLB hit rate = 90%

Average time:  
(1 + 100) × 0.9 + (1 + 100 + 100) × 0.1 = **111 μs**

Without TLB:  
100 + 100 = **200 μs**

 TLB significantly improves address translation performance.

---

## 2. Swapping vs Overlay

### Swapping
- Performed between **different processes**
- Entire process may be swapped in/out of memory

### Overlay
- Used **within the same process**
- Only part of a program is loaded at a time
- Programmer or loader decides which part stays in memory

---

## 3. Dynamic Partition Allocation Algorithms

### (1) First Fit
- Pick the first free block that is large enough  
- Fast and simple  
- Tends to create small fragments at low addresses

### (2) Best Fit
- Pick the smallest free block that fits  
- Leaves many tiny fragments  
- High fragmentation

### (3) Worst Fit (Largest Fit)
- Pick the largest free block  
- Reduces tiny fragments  
- Large blocks may be wasted; big requests may fail later

### (4) Next Fit
- Like First Fit, but search continues from last allocated position  
- Reduces repeated scanning of small low-address blocks  
- Large blocks at high addresses may be consumed early

### Comparison

| Algorithm | Idea                            | Pros                       | Cons                         |
| --------- | ------------------------------- | -------------------------- | ---------------------------- |
| First Fit | First suitable block            | Simple, fast, low overhead | Fragmentation at low address |
| Best Fit  | Smallest suitable block         | Saves large blocks         | Many tiny fragments          |
| Worst Fit | Largest free block              | Reduces tiny fragments     | Large blocks wasted          |
| Next Fit  | Continue search from last point | Less scanning overhead     | High-address fragmentation   |

**Conclusion:**  
In practice, **First Fit usually performs best overall**.

---

## 4. Logical → Physical Address Translation (Paging)

Steps:
1. Logical address A → Page number P + Offset W  
2. Check if P < page table length  
3. Find frame number b from page table  
4. Physical address E = b × PageSize + W  
5. Access physical memory

Example:
- Page size = 1KB  
- Logical address = 2500  
- P = 2500 / 1024 = 2  
- W = 2500 % 1024 = 452  
- Frame b = 8  
- Physical address = 8 × 1024 + 452 = **8644**