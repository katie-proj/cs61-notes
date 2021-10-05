# Lecture 10

## Random Questions
### Big vs. Little Endian
- how multi-byte values are represented (order of least vs. most significant bytes)
- **little endian**: least significant digit (smallest base) comes first / stored in smallest address
- **big endian**: most signficant digit (largest base) comes first / stored in smallest address

### Callee vs. Caller Saved Registers
- most registers (e.g. rax) are caller-saved; when a function is called, that function has the right to put anyting into that register
- callee-saved registers (e.g. stack), must always be restored to original value before returning by callee by *saving the input* and returning that same input
- difference is who is responsible for restoring the register; callee is always restored, caller may or may not be restored (responsibility of the caller to decide)
- callee-saved regsiters typically are used when a function calls another function

## Sanitizers
- 😊😊
- undefined behavior sanitization will use conditional branches to check against undefined behavior

### Shadow memory
- address sanitization allocates a shadow memory that keeps track of what parts of real memory are allocated
- before dereferencing a pointer, the sanitizer checks that shadow memory allows it

### Buffer overflow
- ```%rsp``` -- buffer on the stack is 400 bytes -- canary -- entry ```%rsp``` -- return address --- saved local variables
1. clear buffer to all zeroes
2. copy input to buffer
3. hash function: divide buffer into 4 bytes and add chunks together
- during buffer overflow, the return address space of the stack gets overwritten by a new return address to ```exec_shell```
- canary = magic number, prevents buffer overwrite (boundary overwrite) by checking if the canary's value has been changed
