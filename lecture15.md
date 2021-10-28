# Lecture 15
## Cache abstraction
1. user
2. cache
3. underlying storage
- user generates a stream of access requests (e.g. read, write)
- requests can be answered by either cache or underlying storage
- cache is divided into slots, which can hold a block or nothing, and absorbs requests from user to generate a more efficient sequence of requests to storage

## Storage access costs
Caches optimize access to storage. Storage access are reads and writes.
- read: copy N units of data from storage
- write: copy N units of data to storage  

### Cost Model
- per-request cost R (measured in units of time) ==> making a system call involves saving registers, calling kernel
- per-unit cost U (measured in units of time / unit of data) ==> system calls that move bytes of data from user-space to kernel-space
- cost of operation accessing N units of data: C = R + NU

### Latency and throughput
**latency**: time it takes to access a unit of data (s), low latency is good  
**throughput**: rate at which data can be accessed (units/s), high throughput is good  

### Hard disk drive 
- hard disk drive has circular platters that are stacked ontop of each other and spin around a spindle
- arms that terminate in a head, and the heads swing together as a unit
- there are concentric circles of encoded data, called tracks, on each platter
- "seek time" = time it takes for arms to move to the correct location of platter
- "rotational latency" = time it takes for the track to move to the correct location under the head
- seek time + rotational latency = per-request count

## Cache optimizations
1. Batching - combine multiple requests into one request, reducing total per-request cost R
2. Prefetching (read only) - fetch data before it is needed by assuming sequential access and reading more data than the user requested, reducing number of requests;
ideally cache reads underlying storage before user requests corresponding data / user reads from cache
3. Write coalescing (write only) - do not write data that will be overwritten later by assuming cache line is updated multiple times, reducing number of requests; 
ideally cache writes underlying storage after user wites to cache
4. Parallell access - perform accesses in parallel with other work

## Cache correctness
Caches aim for transparency. A fully transparent cache is **coherent**.
- cached reads return same data as direct reads
- cached writes perform same ultimate modifications as direct writes
- stdio cache is not coherent
