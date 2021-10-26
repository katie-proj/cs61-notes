# Lecture 14

## File I/O
C++ standnard defines library functions for file access.
- ```open(filename, mode)```: opens a file and returns a file descriptor
- ```read(fd, buf, sz)```: read up to ```sz``` bytes from ```fd``` into ```buf```, return number of bytes read
- ```write(fd, buf, sz)```: write up to ```sz``` bytes from ```buf``` into ```fd```, return number of bytes written  

1. Write data to a file one byte at a time using library functions (stdiobyte):
```
size_t n = 0;
while (n < size) {
  int ch = fputc(buf[0], f);
  if (ch == EOF) {
    perror("write");
    exit(1);
  }
  ++n;
}
```
2. Write data to a file one byte at a time using system calls (osbyte):
```
size_t n = 0;
while (n < size) {
  ssize_t r = write(fd, buf, 1);
  if (r != 1) {
    perror("write");
    exit(1);
  }
  ++n;
}
```

3. Write data to a file one byte at a time directly to memory (membyte)  

System calls are slower than writing directly to memory because they require transferring permission, saving registers, etc. Bytes written into files
are stored onto a flash drive, rather than on the hardware disk. The functions using library/system calls are fundamentally altering more hardware than
the function that writes to memory. stdiobyte writes to a temporary buffer.

## Storage Hierarchy
Top -> Bottom:
- registers (fastest to access, closest to processor, smallest size)
- L1 cache
- L2 cache
- L3 cache
- L4 cache
- main memory
- solid state (flash) drive
- disk drive  

Caches are smaller in size, physically closer to processor, and faster to access than primary size, though they are similar physically. Sequential access to
primary memory uses caches efficiently. 

### Expense of storage
Expense is a major factor in the construction of computer systems because new technologies are often more expensive.

## Caching
**Cache**: small amount of fast storage used to speed up access to slower storage.  
- computer systems are covered with caches; registers act aas a cache for primary memory, and memory acts as a cache for flash/hard disk
- ```strace```: powerful Linux program that snoops on another program's system calls and prints a human-readable summary of those system calls to a file
(```strace -o strace.out PROGRAM ARGUMENTS)```)

### Stdio Process
**stdio cache**: user-level memory  
**buffer cache**: kernel-level memory  
