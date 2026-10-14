---
title: "System Calls and Support"
tags: [syscalls, kernel, glibc, strace, user-space]
related: ["[[levels-of-abstraction]]", "[[kernel-overview]]", "[[user-space-vs-kernel-space]]", "[[unix-history]]"]
---

# System Calls and Support

Every time a program opens a file, allocates memory, forks a child process, or sends a packet - it's making a system call. There's no other way for user-space code to get the kernel to do anything. Syscalls are the entire interface between the two worlds.

## What actually happens during a syscall

The program puts the syscall number in `rax`, arguments in other registers (`rdi`, `rsi`, `rdx`...), and executes the `syscall` instruction. That instruction causes a CPU trap: execution switches to ring 0, the user-space registers are saved, and the kernel's syscall dispatcher looks up the handler by number and runs it. On return, the result goes into `rax`, the registers are restored, and the program continues from the next instruction. The whole thing takes microseconds.

```mermaid
sequenceDiagram
    participant App as User Program
    participant C as glibc
    participant CPU as CPU
    participant K as Kernel

    App->>C: open("file", O_RDONLY)
    C->>CPU: rax=257 (openat), rdi=AT_FDCWD,<br/>rsi=path, rdx=flags<br/>syscall instruction
    CPU->>K: ring 3 → ring 0<br/>save user registers
    K->>K: validate, do the work
    K->>CPU: result in rax, sysret
    CPU->>C: ring 0 → ring 3<br/>restore registers
    C->>App: return fd (or -1, set errno)
```

Every syscall has a number. On x86-64: `read`=0, `write`=1, `clone`=56, `execve`=59, `openat`=257. The kernel has a table. The number is an index into that table.

A lot of material still says `open`=2 and `fork`=57. Both syscalls exist, but modern glibc doesn't use them: `open()` calls `openat` with `AT_FDCWD` (meaning "relative to the current directory"), `fork()` calls `clone`, `wait()` calls `wait4`, and `exit()` ends in `exit_group`. Grep an strace of anything current for `open(` and you won't find it.

## The syscalls worth knowing

| Syscall | What it does |
|---|---|
| `openat(dirfd, path, flags)` | Opens a file, returns a file descriptor. What glibc's `open()` calls |
| `read(fd, buf, n)` | Reads up to n bytes from fd |
| `write(fd, buf, n)` | Writes n bytes to fd |
| `close(fd)` | Releases the file descriptor |
| `clone(flags, ...)` | Creates a child process (glibc's `fork()` is a `clone`). Returns 0 in child, child's PID in parent |
| `execve(path, argv, envp)` | Replaces the process image with a new program |
| `mmap(...)` | Maps memory into address space - this is what `malloc` calls eventually |
| `wait4(pid, status, ...)` | Waits for a child to finish. What `wait()` and `waitpid()` call |
| `exit_group(status)` | Terminates the process, every thread of it. What `exit()` ends in |

The fork+exec pattern is how every new program gets launched. The shell forks itself, gets a child process, and calls `execve` in the child to replace it with whatever you typed. The original shell sits there waiting with `wait()`. In an strace that's `clone`, then `execve` in the child, then `wait4` in the parent.

## glibc

You almost never call syscalls directly. glibc (GNU's C library, see [[unix-history]] for where GNU comes in) wraps all of them - proper C functions, error handling, sets `errno` on failure, returns something sensible. `fopen()` calls `openat`. `malloc()` eventually calls `mmap` or `brk`. Python, Ruby, Go - their runtimes are all sitting on top of the same foundation.

You *can* call syscalls directly using the `syscall()` wrapper in `<unistd.h>` or with inline assembly. It works. It's just non-portable and annoying, and you're giving up everything the library does for you.

## strace

`strace` shows you every syscall a process makes, with arguments and return values. It's one of the better debugging tools on Linux because it's honest - it shows you what the program is actually doing, not what the source code claims it's doing.

```bash
strace ls /tmp

# only care about file opens
strace -e trace=openat ls /tmp

# attach to something already running
strace -p 1234

# get a summary with counts and time
strace -c ls /tmp
```

Run `strace` on something simple like `ls` and actually read the output. Before `main()` even runs, you'll see the dynamic linker opening `/etc/ld.so.cache`, loading shared libraries, glibc initializing. It changes how you think about what a "running program" actually is.

## exam-note

> [!exam] LFCA
> Syscalls are the user→kernel interface. Know the common ones: `open`, `read`, `write`, `fork`, `execve`, and that a modern strace shows `open` as `openat` and `fork` as `clone`. glibc wraps them. `strace` traces live syscall activity on a process.

## Related

- [[levels-of-abstraction]]
- [[kernel-overview]]
- [[user-space-vs-kernel-space]]
- [[unix-history]]
