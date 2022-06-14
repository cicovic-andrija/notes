# Notes on: Windows Internals

# Windows Internals Seventh Edition Part 1
*by Pavel Yosifovich, Alex Ionescu, Mark Russinovich,
David Solomon*

## Chapter 1. Concepts and Tools

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
        (virtual address space, executable binary (image), list of open handles, security context, process 
        ID, thread(s)...)
- Each process is named by its executable image and has a unique process ID
- Each process points to its parent process (may be, but is not always, its creator process)
    - Parent process may no longer exist, and that's fine. Windows does not keep track of higher ancestors of the
        process, it can only trace back up the process tree if the parent process is still running
- Process ID is part of an internal structure called client ID

### Threads

- Thread - an entity within a process that Windows schedules for execution
- Essential components of a thread:
    - Contents of a set of CPU registers representing the state of the processor
    - Two stacks - one for user-mode and one for kernel-mode
    - Private storage area - *thread-local storage (TLS)* for use by subsystems, run-time libs and DLLs
    - Thread ID (never overlaps any process ID, they are generated from the same set)
    - Sometimes, thread's own security context
- Thread ID is part of an internal structure called client ID
- Registers and TLS constitute thread's context, which structure is architecture-specific
- Fibers and user-mode scheduling (UMS) TBC

## Environment Variables

| Variable | Purpose |
| -------- | ------- |
| _NT_SYMBOL_PATH | List of symbol paths that various tools (Process Explorer, Debugging Tools...) automatically look for |

## Graphical Tools

| Tool | Description |
| ---- | ----------- |
| winver  | Display current Windows version info |
| taskmgr | Windows Task Manager |
| procexp / procexp64 | Process Explorer by Sysinternals |

## Command-line Tools

| Tool | Description |
| ---- | ----------- |
| ver  | Print current Windows version |
| tlist | 

## Code names

- Windows CE - Microsoft's real-time OS
- OneCore - Converged kernel, driver and other base
    binaries for all hardware platforms (PC, Xbox,
    IoT devices...); since Windows 10
