# Potentially Lecture 20

## Wakeing up
- ```std::mutex```: gives mutual exclusion and blocking; two threads try to access the same data at the same data, only one can access (lock allows one thread to acquire lock/block
by every other thread)
- thread blocks until th emutex is unlocked
- ```std::mutexxx``` does not supply a wakeup primitive analogous to interruption ==> lost wakeups

## Data types that I probably should have known
- ```std::thread```
- ```std::atomic<bool>```: helps with data races? due to synchornized access
- ```std::mutex```

Better to use mutex over atomic variable, unless that variable is very small and self-contained.

## Deadlock
Deadlock occurs when there are multiple threads that are waiting for each other. Conditions for deadlock are:
1. mutual exlcusion - each resource is held by at most one thread
2. hold and wait - a thread holds one resource while attempting to acquire another
3. blocking wait (no preemption) - a thread attmempting to acquire a resource
4. circular wait

## Condition Variables
Conditon variable is like a ```wakeup_point```, but it avoids lost wakeups.
- ```std::condition_variable_any``` has two important methods: 1) ```wait(mutex)``` blocks until another thread calls, 2) ```notify_all()``` will unblock any threads that are waiting on the condition variable
- atomic unlock and block

## Minipipe
Minipipe is a pipe for communication between threads.
- ```write```: write to pipe, block until there's room to write or read end closed
- ```read```: read from pipe
