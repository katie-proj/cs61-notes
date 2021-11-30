# Lecture 18

## Eddie answering questions
Blocking vs. Polling: 

```
// POLLING
// the thread is runnable the whole time
this->m.lock();
while (this->full) {
  this->m.unlock();
  this->m.lock();   // this could block, but the loop overall won't block since the mutex is only held for a very short time
}

// BLOCKING
// this thread is not runnable (loop is only in the case of spurious wakeups), likely will only iterate once
this->m.lock();
while (this->full) {
  this->cv.wait(this->m);   // will block until nonfull conditional variable is notified
}
this->m.unlock();
```
- conditional variables are useful for polling and to signal across threads; always use use conditional variable in a loop to account for spurious wakeups
- mutex should be used for only a short amount of time, and needs to be locked/unlocked by the same thread
- threads will wait until they receive ```notify_all``` call, then the threads fight to gain access to the mutex --> only one thread gains access to the mutex and is able to run while the other threads are blocked

## Databases
### Key-value database
A map that's connected to the network. Operations are ```get, set, delete```.
- ```get(K)```: return the value stored for key ```K```
- ```set(K, v)```: change the value stored for key ```K``` to ```V```
- ```delete(K)```:  delete any value stored for key ```K```

### Network API
A network connection is a streaming connection, like a pipe, between two different computers. Because the "pipe" is between unrelated computers, there's no common
ancestor--no "parent process" to inherit the pipe from. So the solution is to use **client-server communication**.

### Clients and Servers
**Server**
- program that opens a **listening port** that can accept new connections
- passive endpoint
- client sits waiting and opens a port

**Client**
- program that opens a new connection to a preexisting listening port on some server
- active endpoint
- client actively goes to server to connect to a listening port

### WeensyDB Example
```
int fd = open_listen_socket(port);
// doesn't receive data, but rather new connections

while (true) {
  struct sockaddr addr;
  socklen_t addrenlen = sizeof(addr);
  
  // accept connection on listening socket, cfd is new connection
  int cfd = accept(fd, &addr, &addrlen);
  
  // handle connection, close cfd when done
  handle_connection(cfd, unparse_sockaddr(&addr, addrlen);
}
```
- database is a hash table (array of linked lists, each node of linked list having a key-value pair)
- hash function: bucket number = first character of key --> easy to create two keys that hash to the same thing

### WeensyDB Synchronization Issues
Use ```dbmap``` API.
1. Parallelism - create a thread so that multiple clients can access the server at the same time, detach thread after it forms a connection with server
2. Data Races - create a mutex to make it so that every access to a bucket locks, lock/unlock mutex within while loop of ```handle_connection```
3. Parallelism Again - coarse-grained locking used to fix data races has poor parallelism --> rather than just one mutex, create one mutex per bucket to have concurrency
4. Getting locked out - one connection currently has power to crash the server and lock out other connections --> read command value obtaining the lock to the mutex

### Potential tips for Pset from WeensyDB example
- Fine-grained locking: move unique_lock guard later and obtain mutex for one bucket --> ```std::unique_lock<std::mutex> guard(m[b])``` line each time before accessing 
the bucket ```b```  
- Only apply the unique_lock when accessing the bucket, as computing the bucket ```b``` relies on read calls. Since there is a constant # of buckets, we don't need
to lock before computing a bucket.
- mutex should be held for short periods, not long periods. Blocking will hold mutex indefinitely. Perhaps perform read calls before locking.
- Need to lock more than one bucket, if there is a command (```exch```) that tries to access multiple buckets
