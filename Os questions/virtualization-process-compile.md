# Virtualization, Process State, and C/C++ Build Pipeline Summary

## 1. Virtualization

Virtualization maps **one physical resource into multiple logical resources**.

### Two Main Types
- **Time-sharing virtualization**  
  Multiple processes/threads share CPU time (time slicing)

- **Space-sharing virtualization**  
  Memory virtualization:  
  - Each process has its own virtual address space  
  - Pages are mapped to physical memory  
  - Pages not in memory are swapped in on demand

---

## 2. Process States and Transitions

### Main States
- **Ready** – waiting for CPU  
- **Running** – currently executing  
- **Waiting (Blocked)** – waiting for I/O or event  

### Transitions
- Ready → Running (scheduler dispatch)
- Running → Ready (time slice expires / preemption)
- Running → Waiting (I/O request)
- Waiting → Ready (I/O complete)
- Running → Exit

### Key Points
- Only **Ready ↔ Running** are mutual transitions  
- Blocked state is due to missing resources (not CPU)

---

## 3. From C/C++ Source Code to Executable

### Step 1: Preprocessing
Handles:
- `#include`  
- `#define` (macro expansion)  
- Conditional compilation (`#if`, `#ifdef`, `#else`)  
- Remove comments  
- `#pragma once`  
- Insert line/file info for debugging

Output: preprocessed source file

---

### Step 2: Compilation
Includes:
- Lexical analysis (tokens)
- Syntax analysis (AST)
- Semantic analysis
- Optimization
- Code generation (target assembly / object code)

Output: `.o` (Linux) or `.obj` (Windows)

---

### Step 3: Assembling
- Converts assembly into machine code
- Produces object files

---

### Step 4: Linking

#### Static Linking
- All libraries copied into executable  
- Pros: fast startup, no runtime dependency  
- Cons: large binary, hard to update, memory waste

#### Dynamic Linking
- Libraries loaded at runtime  
- Pros: smaller binaries, easy update, shared memory  
- Cons: runtime overhead, dependency issues

---

## 4. Summary (Interview Version)

- Virtualization enables time-sharing and space-sharing of resources  
- Process states: Ready, Running, Waiting  
- Program build pipeline:  
  **Preprocess → Compile → Assemble → Link**  
- Static linking: fast but large  
- Dynamic linking: flexible but slower