# Lecture 12

## WeensyOS
- boot files: ```boot*```
- kernel files: ```kernel.cc, kernel.hh, k-*``` (files that run on machine with full privileges)
- user-mode files: ```u-*, p-*``` (files that do not have full privileges)
- library files (shared): ```lib*, x86-64.h, types.h``` (e.g. impelmentation of memcpy, strcpy)

### Commands
```make run``` boots the OS  
```make kill``` kills all Docker containers  
```make run-gdb + gdb -ix build/weensyos.gdb``` 

### Emulation
WeensyOS runs inside a machine emulator called ```QEMU```, which interprets instructions, translates input/output

## How does a computer start up
1. computer turns on
2. built-in hardware initializes the system
3. buil-in hardware loads a small program called **boot loader** (software) from a fixed location on attached storage
4. boot loader initializes the processor and loads the kernel

## Kernel Isolation
- kernel always retains full privilege over all machine operations
- unprivileged processes cannot stop the kernel from running or corrupt the kernel

Lower 2 bits of ```$cs``` means privilege mode
