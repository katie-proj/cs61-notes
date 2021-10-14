# Lecture 13

## Virtual Memory
- no virtual memory will be on midterm, only process isolation for kernels would be on midterm

### Isolation can benefit from hardware assistance
Process isolation says every computer resource is portected. Some resources are protected by **hiding** them, and others are protected by **adding privilege checks to hardware**.    
Hiding:
- unprivileged processes cannot access them directly
- kernel provides abstractions of these resources
- kernel gets direct access to input/output devices (e.g. disk drives, keyboards) and processes get access to a file abstraction
- access to resources require system calls
- these **resources are relatively slow**

Privilege Checks:
- unprivileged processes do access the processor (CPU instructions) and primary memory (addresses) directly
- these **resources are fast and constantly accessed**
- requiring explicity kernel intervention would slow the computer
- process must not access memory outside its isolated domain and muust not monopolize processor indefinitley --> VIRTUAL MEMORY
- **virtual memory**: layer of indirection by which processor accesses memory, maps VA (virtual address) -> PA (physical address)
- when an instruction accesses virtual address ```x```, the processor access physical address ```P(x)```

### Faults
Address access can **fault** (a processor exception)  
- fault causes processor to protected control transfer to the kernel, allowing the kernel to kill the process

### Virtual Memory for Kernel Isolation
PHYSICAL MEMORY: |--------| kernel code, kernel data, kernel stack |---------| processor code, processor data, processor stack |  
Q: How can virtual memory protect kernel data from processes?  
A: Define mapping function P such that virtual address directly map to physical addresses and all other addresses map to some garbage address in physical memory (not in kernel)
A: If a process tries to access kernel addres, the process will fault  
- you need privilege to change the virtual memory mapping function P, otherwise it would not provide isolation

### Virtual Memory for Process Isolation
Q: How can virtual memory protect one process's data from another process?  
A: Each process gets its own virtual mapping function  
**Process isolations says that different processes have different views of memory.**

### x86-64 virtual memory: Addresses
- virtual address range from [0, 2^64)
- physical addresses range from [0, # of bytes of memory you bought on computer)
- many more virtual addresses than physical addresses

### x86-64 virtual memory: Pages
- memory is organized in aligned contiguous ranges called **pages**
- page starts at an address that is a multiple of 2^12
- any virtual page (block) can map onto any physical page, but within a page the addresses stay in the same order
- pages simplify and speed up hardware because nearby addresses are translated the same way

### x86-64 virtual memory: Permissions and modes
- virtual memory mappings are sensitive to type of access and access privileged, as specified with **permission flags**
- ```PTE_P```: mapping is present
- ```PTE_W```: mapping allows writing (if absent, all access must be read)
- ```PTE_U```: mapping allows uprivilege/user access (if absent, only kernel can access)

### Page table / implementation of mapping function
- 512-ary tree indexed by parts of the virtual address
- ```%cr3``` register holds address of current page table, holds a physical address  

EXAMPLE:  
V = 0x0000'0000'0800'1000  
V_offset = last 12 bits, 0  
V_level1 = next 9 bits, 1  
v_level2 = next 9 bits, 64  
V_level3 = next 9 bits, 0  
V_level4 = next 9 bits, 0  
Use the level to index into ```%cr3``` and get virtual memory address. For instances, ```%cr3[0]``` will provide virtual memory for level 4 index.

### Virtual memory and Kernel execution
Kernel needs access to all memory. It also must query and modify the memory mapping seen by processes.

