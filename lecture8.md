# Lecture 8

## Calling Convention
* some aspects of machine code are fixed by the processor manufacturer
* some aspects of machine code are set by agreement among compiler and operating system developers
* calling convention: agreement between compiler and operating system developers, only codes with the same conventions can safely interact  
Elements of a calling function
* function arguments, function return values, local variable storage

## Type-safe linkage and mangled names
* the name of a C++ function encodes the types of its arguments, making compilations safer and supporting overloading (functions with different behavior based on argument types)  
Example: ```f(int) -> _Z1fi```  
* _Z: mangled name 
* 1: function name is 1 character long 
* f: actual function name 
* i: first argument is int 
Demanble using ```c++filt MANGLEDNAME```  
* unused argument is ignored
* ```%rsp``` = Stack pointer
* argument registers: ```%rdi, %rsi, %rdx, %rcx, %r8, %r9```
* caller needs to leave extra memory (breadcrumb) so that it knows the address of the next instruction after the call
* redzone = area just below ```%rsp```
* stack frame = memory from entry rsp of where the function begins to the entry rsp of the next function's beginning
* registers are not stored in memory, rather they are stored physically close to processor

11:49
34
