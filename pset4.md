# Pset 4 Overview

## Step 1
Don't read one character at a time.

## Step 2 - Read
Store extra data in single-slot cache buffer as preparation because you anticipate the user will request more data. This reduces the amount of calls to the kernel.
Don't read an entire file into the cache.  
- ```fd```: file descriptor tag is constantly changing to keep up with the next byte

## Step 3 - Write

## Step 4
- currently ```lseek``` only finds a space in memory
- need to specify reading and writing after ```lseek``` 
- implemente ````lseek````: pass in pointer to file, change tags to change position of file descriptor?

## Step 5
- ```lseek```: create cache based on alignment to increase probability of hitting a seek that is backward or forward

## Access Patterns
- sequential pattern does not use seeks in strace

## Single-Slot Cache
- ```off_t tag``` = ```off_t pos_tag``` at beginning of cache
- ```pos_tag``` moves as you read, incremented by one if you read one byte
- ```end_tag``` = end of cache (NOT NECESSARILY SIZE OF CACHE)
- ```tag``` = start of cache

io61_fill performs the read system call, so that io61_read and io61_readc do not make system calls, but instead call io61_fill
```
void io61_fill(io61_file* f) {
    // Check invariants.
    assert(f->tag <= f->pos_tag && f->pos_tag <= f->end_tag);
    assert(f->end_tag - f->pos_tag <= f->bufsize);

    // Reset the cache to empty.
    f->tag = 0; // may not want to have tag always be 0 in pset
    f->tag = f->pos_tag = f->end_tag;
    
    // Read data.
    ssize_t n = read(f->fd, f->cbuf, f->bufsize);
    if (n >= 0) {
        f->end_tag = f->tag + n;
    }
}
```
code needs to be made faster
