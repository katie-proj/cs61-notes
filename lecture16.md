# Lecture 16
## Process Control
- process control allows you to coordinate multiple processes to accomplish complex tasks
- shell is program that prints a prompt, waits for input, parses command line to determine which program to invoke, creates new processes to invoke program, and waits
for process to complete before waiting for another prompt

### Process Control System Calls
- create a process: ```fork```
- run a program: ```exec``` family
- exit a process: ```_exit```
- stop a process: ```kill```
- await completion: ```waitpid```

### Process Componenets
**Process = program image + identity + environment**  
**Program image**: uprivileged, contents of memory
- code, data, stack, and heap
- command line arguments (```argc```, ```argv```)

**Identity**: kernel, managed by kernel
- process ID = returned by ```getpid``` system call
- process relationships (parent process ID)
- ownership, timing, etc.

**Environemnt**: kernel, managed by system calls
- file descriptor table (open file descriptors, file positions)
- file descriptor table is an array (0 = standard input, 1 = standard output, 2 = standard error ==> all connected to terminal)
- a new system call, opens new slot in file descriptor table  

Piping: Shell is changing a process's intitial environment, so that standard input is not pointing to terminal but to a file.

## Process Hierarchy
Every process has a parent process. 
- ```getpid```: return current process ID
- ```getppid```: return parent process ID
- ```fork```: creates new child process  
Processes form a tree, where the root of the process hierarchy is process with ID 1 (init, which starts up shell). Random chance of whether parent or child
process runs first after forking.

### ```fork``` vs. ```exec```
```fork```:
- cloned program image
- new identity
- cloned environment

```exec```:
- new program image
- unchanged identity
- unchanged environment

Executing new processes: First create a new process using fork, then replace that process's program image using exec. exec only returns if it fails (return -1).
