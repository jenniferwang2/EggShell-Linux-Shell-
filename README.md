<div align="center">
  <img src="EGGSHELL.png" alt="EggShell Logo" width="300">
  
  # EggShell Linux Shell
  
  *A custom Unix shell implementation with support for built-in commands, process management, I/O redirection, and piping.*
</div>

---

## Overview

EggShell is a Unix shell implementation that provides essential shell functionality including command execution, background process management, signal handling, and I/O operations. The shell supports both built-in and external commands while maintaining proper process control and resource management.

## Features

- ✅ Built-in commands (`cd`, `pwd`, `exit`)
- ✅ External command execution with `fork()` and `execve()`
- ✅ Background process management with `&`
- ✅ Signal handling (Ctrl+C, Ctrl+Z)
- ✅ I/O redirection (`>`, `<`)
- ✅ Command piping (`|`)
- ✅ Process resource tracking and cleanup

## Design Choices

### Built-in Commands
Built-in commands (`cd`, `pwd`, `exit`) execute directly within the shell process rather than forking. This design choice ensures that:
- `cd` can properly change the working directory of the shell itself
- `pwd` reflects the current shell's working directory
- `exit` can cleanly terminate background processes and display execution statistics

### External Command Execution
External commands utilize `fork()` and `execve()` for process creation and execution:
- **Foreground processes**: Shell waits for completion using `wait4()`
- **Background processes**: Marked with `&`, execute asynchronously without blocking the shell
- Output from background processes is preserved (not redirected to `/dev/null`) as per assignment specifications

### Signal Handling
Robust signal management ensures proper shell behavior:

| Signal | Behavior |
|--------|----------|
| **SIGINT (Ctrl+C)** | Terminates foreground process only; shell remains active |
| **SIGTSTP (Ctrl+Z)** | Suspends foreground process; displays suspension message |
| **SIGCHLD** | Handles background process termination and resource collection |

### I/O Operations
- **Redirection**: Implemented using `dup2()` for file descriptor manipulation
  - Input: `command < input.txt`
  - Output: `command > output.txt`
- **Piping**: Single-level pipes supported using `pipe()` and `fork()`
  - Example: `echo hello | grep h`

### Process Management
- Background processes tracked in process ID array
- Automatic cleanup on shell exit using `kill()`
- Resource usage statistics collected via `wait4()`
- Terminal control maintained through `tcgetpgrp()` and `tcsetpgrp()`

## System Calls Reference

| System Call | Purpose |
|-------------|---------|
| `fork()` | Create child processes for external commands |
| `execve()` | Replace child process with external program |
| `wait4()` | Wait for process completion and collect resource usage |
| `dup2()` | Redirect file descriptors for I/O operations |
| `pipe()` | Create communication channel between processes |
| `kill()` | Terminate background processes during cleanup |
| `tcgetpgrp()`/`tcsetpgrp()` | Manage terminal foreground process group |
| `signal()` | Register signal handlers for process control |

## Testing Strategy

The implementation was validated through comprehensive testing including:
- Built-in command functionality verification
- External command execution with various arguments
- Background process management and termination
- Signal handling behavior (interruption and suspension)
- I/O redirection with different file types
- Pipe functionality with multiple command combinations
- Edge cases identified through course discussions and forums

## Resources

- Linux manual pages for system call documentation
- [Advanced Bash-Scripting Guide - I/O Redirection](https://tldp.org/LDP/abs/html/io-redirection.html)
- Course discussion forums for edge case handling
- Office hours and laboratory sessions for implementation guidance
- In-class discussions on signal handling and terminal control

---

*Built as part of systems programming coursework - Fall 2025**