# Weenix Kernel

> ⚠️ **Placeholder Repository**
> The Weenix kernel is an academic project developed at Brown University. The full source code is **proprietary** and cannot be shared publicly. This README serves as a portfolio reference describing the work completed.

---

## Overview

Weenix is a monolithic operating system kernel modeled after **6th Edition UNIX (V6)**, developed from scratch as part of a systems programming curriculum. The project covers the full lifecycle of a functioning kernel — from bootstrapping bare hardware to supporting a multitasking, virtual-memory-capable OS with a polymorphic virtual filesystem layer.

---

## Subsystems Implemented

### Process & Thread Management
- Built the **process and kernel-thread subsystems** from scratch, including the full lifecycle: creation, scheduling, context switching, waiting, and cleanup.
- Implemented **`fork()`**, **`exec()`**, and **`exit()`** semantics consistent with UNIX process model.
- Developed **bootstrapping** logic to bring the kernel from early initialization through the first user-space process.
- Implemented **signal handling**, including signal delivery, masking, and default/custom disposition — faithfully modeled after UNIX signal semantics.

### Virtual Memory
- Designed and implemented the **virtual memory subsystem** from the ground up.
- Implemented **Copy-On-Write (COW) forking** using a **shadow object** and **shadow walking** architecture: on fork, pages are not copied eagerly; instead, shadow objects chain together parent and child address spaces and copy individual pages only on write faults.
- Managed **virtual memory areas (VMAs)** and page fault handling, including demand paging and anonymous mappings.

### Virtual Filesystem (VFS)
- Implemented the **Virtual File System (VFS)** layer, providing a uniform interface over heterogeneous underlying filesystems.
- Supported **polymorphic dispatch** via function pointer tables (analogous to `file_operations` and `inode_operations` in Linux), allowing VFS to front any compatible concrete filesystem.
- Implemented **reference counting** for both `file` objects and **virtual inodes (vnodes)**, ensuring correct lifecycle management across concurrent users without leaking or prematurely freeing resources.
- Supported standard POSIX file operations: `open`, `close`, `read`, `write`, `lseek`, `dup`, `dup2`, and directory traversal.

---

## Architecture

```
┌─────────────────────────────────────────────┐
│               User Space                    │
├─────────────────────────────────────────────┤
│         System Call Interface               │
├───────────────┬─────────────────────────────┤
│  Process /    │     Virtual Memory          │
│  Thread Mgmt  │  (COW · Shadow Objects)     │
├───────────────┴─────────────────────────────┤
│         Virtual File System (VFS)           │
│   (vnodes · ref-counting · polymorphic      │
│    dispatch · file descriptor table)        │
├─────────────────────────────────────────────┤
│        Concrete Filesystem Drivers          │
│              (S5FS, ramfs, …)               │
├─────────────────────────────────────────────┤
│         Hardware Abstraction / x86          │
└─────────────────────────────────────────────┘
```

---

## Key Technical Highlights

| Feature | Details |
|---|---|
| Kernel model | Monolithic, V6 UNIX-inspired |
| COW strategy | Shadow object chaining + page-fault-driven copy |
| VFS dispatch | Function-pointer vtables per inode/file type |
| Resource management | Reference counting on vnodes and file objects |
| Signal model | UNIX-compatible delivery, masking, and disposition |
| Bootstrap | Full kernel init sequence through first `init` process |

---

## Academic Context

Weenix is a well-known systems project originating from **Brown University's CS 1690 (Operating Systems)** course. It is intentionally challenging and spans multiple months of development. Completing all subsystems — processes/threads, virtual memory, and VFS — represents a full, bootable, self-consistent kernel.

> **Note:** Per course policy and Brown University's academic integrity guidelines, source code is not publicly available. This repository exists solely as a record of completed work for portfolio purposes.

---

## Skills Demonstrated

- Low-level systems programming in **C**
- Deep understanding of **OS internals**: scheduling, virtual memory, filesystems
- Debugging at the kernel level (QEMU, GDB, kernel panics)
- Designing layered, modular systems with clean abstraction boundaries
- Memory safety discipline in an environment with no runtime safety net
