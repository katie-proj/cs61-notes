# Lecture 1

## Outline
1. Data Representation
2. Assembly
3. Storage & caching
4. Kernel programming
5. Process management
6. Concurrency

## Data Representation
### Objects
* each program (dynamic) runs in a private data storage space, **memory**, where it remembers the data it stores (passive)  
* programs work by manipulating **values** (e.g. int, float)  
* **object**: region of memroy that contians a value

```
#include <cstdio>
#include "hexdump.hh"

int i1 = 61; // true variable, value can change
const int i2 = 62 // constant, value does not change

int main() {
  int i3 = 63;
  int* i4 = &i3; // pointer with reference to i3
  int* i5 = new int{75} // dynamic memory allocation, new integer initialized to 75
  
  printf("i1: %d\n", i1);
  printf("i2: %d\n", i2);
  printf("i3: %d\n", i3);
  printf("i4: %p\n", i4); // prints pointer (hexadecimal/base-16 -- starts with 0x), address of i3
  printf("value pointed to by i4: %d\n", *i4); // prints value pointed to
  
  // prints out address of object, sotrage that makes up object (contents), any readable characters
  hexdump_object(i1)
  hexdump_object(i2)
  hexdump_object(i3)
  hexdump_object(i4)
}
```
objects: i1, i2, i3, i4  
values: 61, 62, 63, &i3, *i4   
1 GB = 2^30 bytes  
byte = value between 0-255
int = 4 bytes, pointer = 8 bytes

* local variables (i3, i4) are stored closely together in **stack memory**
* global variables (i1, i2) are not stored next to each other because i2 is data that does not change is protected from modification by the hardware and stored in a different segment (**code text** segment) from i1 which can be changed (**data** segment)
* dynamically allocated objects are sotred in **heap memory**
* typically stack memory grows down; as more functions are called, stack addresses get smaller and smaller toward heap memory

#### Memory Segments
1. code text (constant objects)
2. data (global objects)
3. heap (dynamically allocated) -- programmer is responsible for managing heap memory
4. stack (local variables) -- program will dynamically handle (optimized)
