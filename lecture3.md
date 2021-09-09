# Lecture 3
## Layout and allocators

### Alignment and size

```
#include <cstdio>
#include "hexdump.hh"

int main() {
  struct {
    int a;
    int b;
    char c;
    char d;
  } s1 = {61, 62, 63, 64}
  
  struct {
    int a;
    char c;
    int b;
    char d;
  } s2 = {61, 62, 63, 64}
  
  hexdump_object(s1);
  hexdump_object(s2);
```
* hardware and compiler impose alignment restrictions on primitive types
* whenever an object of type T is stored in memory, its address must be a multiple of alignof(T)
* standard says alignments must be powers of two
* therefore every alignment is an integral multiple of all smaller alignments
* members of compound object must obey alignmnet restrictions:
Array: ```alignof(T[N]) = alignof(T)'''
Union: ```alignof(union { T_1 m_1; ...; T_N m_N; }) = max_I alignof(T_I)` // union of int (4) and char (2) has alignment 4``
Struct: ```alignof(struct { T_1 m_1; ...; T_N m_N; }) = max_I alignof(T_I)```
For structs, alignment cn require adding **padding** between some members.  
For an array T a[N]: addressof(a[I]) = addressof(a) + I x sizeof(T)
* ```sizeof(T)``` is multiple of its allignment ```alignof(T)```

### Struct Rule
* struct members are laid out sequentially in memory
* offset = # of bytes into struct that the member appears
```
struct s {  // alignment = 4, size = ?, padding = 0

  int a;    // alignment = 4, offset = 0 (always), padding = 0
  
  char b;   // alignment = 1, offset = 4, padding = 3
  
  int c;    // alignment = 4, offset = 8, padding = 0
  
  char d;   // alignment = 1, offset = 12, padding = 3
  
}; // size of struct = 16
```

### Alignment and malloc
* malloc rule: any non-null pointer returned by malloc and friend has the maximum alignment of any type

### Sequential fster than random
* many aspects of computer hardware and software are built to optimize for compact and sequential memory access (addresses that are closer together)
