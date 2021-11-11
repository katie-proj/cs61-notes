# Lecture 17
## Metrics for Communication
**preemption (voluntary vs. involuntary communication)**
- voluntary (non-preemptive): process controls when it receives communication
- involuntary (preemptive): process can be interrupted for communication

**blocking**
- blocking: process waits for communication
- nonblocking: proccess does not wait for communication (e.g. short read)

### Voluntary Termination: exit
```exit``` (= return from main)
- ```exit(STATUS)``` library function performs cleanup actions, such as flusing stdio files
- ```_exit(STATUS)``` system call exits without cleanup

```STATUS``` is remembered by kernel and can be collected by the process's parent. Only ```STATUS = 0 = EXIT_SUCCESS``` means a successful exit.

### Involuntary Termination
- termination by signal
- exit status indicates reason for termination

```waitpid(pid, &status, options)```:
- ```options == 0``` means blocking, ```options == WNOHANG``` means nonblocking
- ```pid == -1``` means wait for any child process (if process has many children), ```pid > 0``` means wait for that specfiic process (specific child)
- returns the termianted process's ID (-1 if there are no children/bad pid supplied, 0 if you choose WNOHANG if there is no child)
- on success, ```int status``` is filled in with the status; ```WIFEXITED(status) != 0``` iff proccess terminated by exit and ```WIFEXITED``` is that exit status


#### ZOmBIes 👻
Only the parent can collect a termination notification (cannot wait for a grandchild, cannot wait for parent, only parent can wait for child). If a parent process
does not wait for a child process, the child becomes a zombie process.

**zombie process** = process that has terminated, but the parent has not collected status
- pid (process ID) should not be reused until the termination notification is collected, otherwise that pid still belongs to the parent

## Piping
```pipe(int pfd[2])``` system call
- argument is a pointer to a two-element array
- creates a pipe, which is a high-throughput communication stream
- pipe associated with two descriptors, ```pfd[0]``` and ```pfd[1]```
- data written to ```pfd[1]``` (write end, left side) may be read from ```pfd[0]``` (read end, right side)
- returns 0 on success, -1 on failure (current process has too many file descriptors open already, not enough memory)

### About Pipes
- pipes are private, meaning they are only accessbile to processes that inherit one or more ends from a parent
- once afile descriptor is closed, it cannot be reopened

### dup
```dup2(oldfd, newfd)```: make ```newfd``` point to the same file description as ```oldfd```  
1. pipe -- parent; because the only way for a child to have access to a pipe is to inherit from it
2. fork -- parent; child process file descriptor looks like the parent's
3. dup2(4,1) -- write end is in file descriptor index 4, want the write end to point to stdout (index 1)
4. close(3) -- child 
5. close(4) -- child
6. execvp("echo") -- child
7. close(4) -- parent
8. fork -- parent; creates a second child
9. dup2(3,0) -- second child
10. close(3) -- second child
11. exec("wc") -- second child
12. close(3) -- parent
