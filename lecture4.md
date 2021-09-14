# Lecture 4

## Pointer and Integer Arithmetic

### Arena allocator review
* **arena allocator** is an object whose purpose is to handle the allocation and deallocation of other objects
* arena allocators can improve performance relative to general-purpose allocators (e.g. many short-term allocations of small objects)

### Libraries and abstraction boundaries
* a library implements a contract called a **specification**, which imoses requirements on both the users of the library and the implementers of the library
```
// ALLOCATOR SPECIFICATION
struct node_arena {
  node* allocate();
  // should return pointer to a newly/freshly allocated node
  
  void deallocate(node* n);
  // argument n must have been previously returned by allocate() and not passed into deallocate() -- n should be a live node
  
  ~node_arena()
  // ~ is a destructor
  // free any storage ussed for allocated nodes and all memory should be deallocated before arena is destroyed
}
```

```
// Per-allocation statistics: count how often each node is reused
// Option 1: external storage design, create new data structure that keeps count of how many times each node is used; use std::map
// Option 2: internal storage design, add additional space to a node that keeps track of how many times it has been used

// void print_uses() {
// for (auto n : ???) {
//  printf("%zu uses @%p\n", ???, n);
// }


// ALLOCATOR IMPLEMENTATION and OPTION 1
struct node_arena {

  struct snode {
    node real_node; // user's information
    size_t count = 0; // implementer's information
  }
  
  node* allocate() {
    snode* n;
    if (this->scratch_.empty()) {
      n = new snode;
    } else {
      n = this->scratch_.back();
      this->scratch_.pop_back()
    }
    ++n->count;
    return (node*) n;
  }
  
  void deallocate(node* n) {
    this->scrtach_.push_back((snode*) n);
  }
  
  ~node_arena() {
    for (auto n : this->scratch_) {
      delete n;
    }
  }
  
  void print_uses() {
    for (auto n : ???) {
      printf("%zu uses @%p\n", ???, n);
    }
  }
}
```

### Pointer representation and address representation
* representation of pointer = representation of address tha tholds corresponding object
* can cast pointers to and from ```uintptr_t```, which is usually ```unsigned long```

### Pointer Arithmetic
* pointer comparisons (==, !=, <, >, <=, >=)
* pointer addition/subtraction (```p1 + k == &a[i] + k == &a[i+k]``` where p1 is a pointer, k is int)

### Valid Indexes
For an array of size ```N```, 
* can form pointers ```&a[0] ... &a[N]```
* can dereference pointers ```&a[0] ... &a[N-1]```

## Integer Representation
Since computer's use fixed object memory, arithmetic is modular (e.g. 1111 1111 + 0000 0001 = 0000 0000)  
Two's complement representation
* addition and subtraction of signed integers use the same representation as addition and subtraction of unsigned integers
* ```-x``` is represented the same as ```2^W - x```, where ```W``` is the number of bits in the integer

Undefined behavior
* signed integer arithmetic is not allowed to overflow, so adding one to a very large postive number should become a very small negative integer, but 
