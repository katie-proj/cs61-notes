# Lecture 2

## Recap
* memory is divided into segments (heap, stack, code/text, data)
* memory holds different kinds of objects

## Sizes and Layouts
### Primitive Types
* char = character, 1 byte
* int = integers, 4 bytes
* float = 4 bytes
* pointer = data representation of pointer is the numeric value of address, 8 bytes
```
#include <cstdio>
#include "hexdump.hh"

int main() {
\\ local variables have lifetime bound to function (stop existing when function ends), stored on STACK (generally have high addresses)
char c1 = 61;
int i1 = 61;
float f1 = 61;
int* p1 = &i1;

printf("c1: %d\n", (int) c1);
print("i1: %d\n", i1); \\ %h would print hexadecimal form
print("f1: %d\n", f1); \\ prints float in way understandable to people
print("p1: %d\n", p1); \\ prints numeric value of address

hexdump_object(c1);
hexdump_object(i1);
hexdump_object(f1);
hexdump_object(p1); \\ value of pointer = value of address
```

**char**
* kind of integer
* any object in memory can be stored as an array of char
* called a byte, holds 8 bits
* ```hexdump``` prints the contents of memory by treating it as an array of char

**int**
* kind of integer
* when char and int variables hold the same value, they look similar

### Size
* every object with the same type has the same size
* size of object = # bytes reuired to store the object
* char = 1, short = 2, int = 4, long = 8, long long = 8, float = 4, double = 8, long double = 16, T* = 8

### Abstract Machine and Hardware Machine
* programs are written with reference to a language standard
* **language standard** is meant to define exactly how every program behaves when executed
* Python and Java have **opaque** language standard because the standard *completely* defines the meaning of every program, meaning the same program should behave identically on any hardware
* C and C++ have **translucent** language standard because the standard *partially* defines the meaning of a program, so some aspects of a program can behave differently on different hardware

### Standard Size of Primitive Types
* char has standard size 1
* all other sizes only set lower bound

### Variant Types
* integer types come in **signed** and **unsigned** varieties
* signed [2^(-B-1), 2^(B-1) - 1)]
* unsigned [0, 2^B - 1]
* positive signed numbers have the same representation as the corresponding unsigned numbers
* char may be signed or unsigned
* types can be qualified as **const** or **volatile**

### Objects in memory
* every object occupies a contiguous range of memory
* objects that might exist at the same time occupy disjoint ranges of memory (live ojbects never overlap)

### Compound types (collections)
* combpound objects are combinations of simpler ones (arrays, struct)
```
#include <cstdio>
#include "hexdump.hh"

int main() {
  int a[2] = {61, 62} \\ 8 bytes, two contiguous 4 bytes

  union {
    int a;
    int b;
    char c;
    car d;
  } u;
  u.a = 61; \\ object size is 4 and does not change (a.c = 61), will hold largest sized object
  
  struct {
    int a;
    int b;
    char c;
    char d;
  } s = {61, 62, 63, 64}; \\ 10 bytes with 2 extra bytes
  
  hexdump_object(a);
```
**Array rule**
* array members are laid out sequentially in memory with no gaps
* given ```T a[N]``` and 0 <= I < N:  
  ```addressof(a[I]) = addressof(a) + I*sizeof(T)```
  
 **Union rule**
 * union types define objects that might have one of several different underlying representation (one active member at a time)

**Struct rule**
* struct members are laid out sequentially in memory with no gaps

**Alignment**
* hardware and compilers can impose restrictions on the addresses at which certain types of objects can appear
* address of any int is a multiple of 4, address of any pointer is a multiple of 8
