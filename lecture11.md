# Lecture 11
## Registers
- each function has its own, possibly-infinite set of local variables
- processor: one set of registers (per core), indefinite local variables are emulated by the compiler by saving and restoring registers to memory
- **program**: set of instructions and data corresponding to a complete set of C++ source code; passive instructions
- **process**: running program in execution

## Process Isolation
Processes interact only in restricted ways as permitted by a global operating system policy 
- every process has its own sandbox, isolated from other sandboxes
- independent sandboxes have independent views of memory
- specific limited process can interact with each other (e.g. reading/writing files, output to terminal)  

### Consequence on Software
Process control over computer resources must be limited in order to provide process isolation. If a process had full control it could 1) run forever, 2) monopolize
hardware, 3) destroy other processes' memory. Policy is too fluid to be written into silicon hardware, but still must be restricted, leading to the **kernel**.

### Consequence on Hardware
- kernel has full control over computer resources; processes are uprivileged/user processes because they do not have full control
- restrictions on processes cannot be enforced by software alone, so the processor and other computer hardware must have a notion of privilege

## Kernel
- **kernel**: software running on the computer must have full control over computer resources
- kernel is the software running on a mcahine that has *full machine privilege*
- processes running on the same operating system interact with the same kernel

### Kernel Responsibilities
- safely (each process is isolated) and fairly (roughly equal amount of memory given to each process) sharing machien resources among processes
- provide convenient abstractions for machine resources (e.g. files, stdout is access to the disk)
- ensure robustness and high performance

## Processor Features
- **system calls**: special instructions for transferring control to the operating system, involves kernel
- processor privilege modes
- hardware features that prevent monopolization of resources
- protection of memory  

- system call number is stored in ```%eax``` 

## WeensyOS
- files that start with ```k``` are kernel source files, files that start with ```p``` process files
- limited library functions
- ```make run``` = command to run WeensyOS
- kernel.cc is where to write code for pset 3
- ```sys_yield``` is needed to isolate processes and give time to kernel to process / have control
- **timer interrupt**: feature of processes, defined to protect against infinite loop attacks; timer will go off and each time it goes off the kernel gets power
