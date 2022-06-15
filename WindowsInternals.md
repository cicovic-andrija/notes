# Notes on: Windows Internals

# Windows Internals Seventh Edition Part 1
*by Pavel Yosifovich, Alex Ionescu, Mark Russinovich,
David Solomon*

## Chapter 1. Concepts and Tools

### Introduction

- **OneCore** - Converged kernel, drivers and other base binaries for all hardware platforms (PC, Xbox, IoT devices...);
    since Windows 10

### APIs

- Native API (Native system services / system calls) - undocumented, underlying services in the OS that
    are callable from user-mode
- Windows API - user-mode programming interface for Windows OS family (32-bit and 64-bit)
- Win32 API - Older name, for the 32-bit Windows API
- Component Object Model (COM) - a newer, more modern API for inter/in-binary component communication, in which
    COM clients communicate with *objects* (aka COM server objects) through interfaces, and interface
    implementations (COM servers) are loaded dynamically (DLL) rather than being statically linked to the client code
- The Windows Runtime (WinRT) - new API introduced with Windows 8 for developing *Windows Apps* (aka *Metro Apps*),
    and is built on top of COM
- .NET Framework - part of windows that consists of
    - The Common Language Runtime (CLR) - runtime engine for .NET which includes a JIT compiler that translates
        Common Intermediate Language (CIL) instructions to the underlying CPU machine language; implemented
        as a COM in-process server (DLL)
    - The .NET Framework Class Library (FCL) - large collection of types that implement common functions needed
        by client and server applications

### Processes

- Program / process
    - Program - static sequence of instructions
    - Process - container for a set of resources needed to execute an instance of the program
        (virtual address space (VAS), executable binary (image), list of open handles, security context, process 
        ID, thread(s)...)
- Each process is named by its executable image and has a unique process ID
- Each process points to its parent process (may be, but is not always, its creator process)
    - Parent process may no longer exist, and that's fine. Windows does not keep track of higher ancestors of the
        process, it can only trace back up the process tree if the parent process is still running
- Process ID is part of an internal structure called client ID
- Security context is stored in an object called an *access token*
- Virtual address descriptor (VAD) - used by the memory manager to keep track of the VAS

### Threads

- Thread - an entity within a process that Windows schedules for execution
- Essential components of a thread:
    - Contents of a set of CPU registers representing the state of the processor
    - Two stacks - one for user-mode and one for kernel-mode
    - Private storage area - *thread-local storage (TLS)* for use by subsystems, run-time libs and DLLs
    - Thread ID (never overlaps any process ID, they are generated from the same set)
    - Sometimes, thread's own security context
- Thread ID is part of an internal structure called client ID
- Threads share the processes's virtual address space
- Threads don't have an access token by default, but can obtain one
- Registers and TLS constitute thread's context, which structure is architecture-specific
- Fibers (lightweight threads), application can do the scheduling, implemented in user mode in kernel32.dll
    and **invisible to the kernel** (causing perf and sharing problems), can't run concurrently on more than
    one processor
- User-mode scheduling (UMS threads) - only on 64-bit Win, visible to the kernel, can issue blocking syscalls,
    share and contend for resources, and switch context in user-mode without involving the kernel scheduler
    (because from kernel perspective, it's all still running on the same kernel thread). For kernel-mode operations
    (like syscalls) UMS thread switches to its dedicated kernel thread (*directed context switch*)

### Jobs

- Job (or a *job object*) - Windows's extension the the process model that allows managing and manipulation of 
    groups of processes as a unit, and offers control of certain attributes and limits for the processes
    associated with the job

### Virtual memory

- Flat (linear) address space, that provides a logical view of a private memory space for processes
- Pages of virtual address space are mapped by the OS's memory manager and hardware to the physical space
- Default page size 4KB
- On 32-bit x86 systems (4GB VAS), lower half of addresses are private to process, and upper half are OS's
    virtual addresses, although this can be changed up to 3GB for the process and 1GB for the OS. Mappings
    for the upper half are mostly the same for all processes because they map to the kernel-owned memory
- On 32-bit x86 systems, AWE (Address Windowing Extensions) allow 32-bit programs to allocate up to 64GB
    of physical memory and map "windows" of it into its 2GB address space
- On 64-bit systems, theoretical limit of the VAS is is 16EB, but 64-bit hardware limits VAS to smaller
    sizes. On Windows 8.1 and newer, 128TB is reserved for process's private use, and 128TB for OS use

### Kernel-mode and user-mode

- Processors provide multiple execution modes (*ring levels* and similiar terminology)
- Windows uses two modes, and in Windows terminology, those are called user-mode and kernel-mode,
    (usually they map to the highest and lowest privilege level modes available on the processor)
- In kernel-mode, OS runs its system services, drivers etc. and has access to all of memory and all
    processor instructions
- Each page in virtual memory is tagged to indicate what mode the processor must be in to read/write
    the page. Since the code running in kernel-mode is running in the highest priviledge mode
    on the processor, every memory page is accessible
- There are very strict security procedures (e.g. code signing) that drivers must go through, in order
    to be installed and loaded in the OS
- When user-mode code executes a system call, a special processor instruction triggers the switch to
    kernel mode and calls a kernel-mode function to handle the call
- Note: mode transition from user/kernel to kernel/user is not a thread context switch, it's a mode switch

### Hypervisor

- Hypervisor - a specialized, highly priviledged component that allows for the virtualization and isolation
    of all resources on the machine (virtual/physical memory, device interrupts, PCI/USB devices etc.)
- Hyper-V - Windows's hypervisor solution
- Hypervisor has greater access privileges than the kernel, and can provide a new set of security services
    for monitoring its instance(s), known as virtualization-based security (VBS)
- Virtual trust levels (VTLs) - Similar to processor priviledge levels; kernel and user mode exists within
    each VTL, but the hypervisor managed priviledges across VTLs
- In hypervisor, kernel code is running in a less priviledged VTL than the VBS and other
    services the hypervisor is running, so even a kernel crash cannot affect the hypervisor

## Environment Variables

| Variable | Purpose |
| -------- | ------- |
| _NT_SYMBOL_PATH | List of symbol paths that various tools (Process Explorer, Debugging Tools...) automatically look for |

## Graphical Tools

| Tool | Description |
| ---- | ----------- |
| winver  | Display current Windows version info |
| taskmgr | Windows Task Manager |
| perfmon | Performance Monitor |
| procexp / procexp64 | Process Explorer by Sysinternals |

## Command-line Tools

| Tool | Description |
| ---- | ----------- |
| ver  | Print current Windows version |
