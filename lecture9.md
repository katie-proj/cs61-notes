# Lecture 9

## State in programming languages
* unbounded number of **variables** of arbitrary type
* dynamic allocation
* functions are modularized (local variables in one function cannot be accessed from others)

## State in CPUs
* small number of named **registers** of limited type (computation acts on registers)
* large pirmary memory accessed by numeric address -- stores data too large for registers
* limited access control within a running program because instructions have undifferentiated access to memory and registers, requiring explicit save & restore

## Control flow in programming langugaes
* sequential execution by default (statements run one after the next) ```if, while, for```
* unstructured control flow ```goto```
* functions have control flow(call, return) and state (parameters, return value)

## Control flow in CPUs
* sequential execution by default
* only unstructured control flow (1. unconditional branch: go to a specific instruction, conditional branch: go to a specific instruction if a condition holds)
* CPu defines control flow of functions
* calling convention defines state

## Unconditional jumps in assembly - jumps from one instruction to another (forward, backward)
* ```j/jmp ADDR```
* ```j ADDR```
* ```j *%reg```: jump to address stored in ```%reg```
* ```j *ADDR```: jump to address sotred in memory ```ADDR```

## Condtional jumps in assembly
* ```je ADDR:```: jump to ```ADR``` if equal, continue to next instruction if not equal
### Condition flags
* arithmetic operations not only produce results, they also change a special-purpos register, **flags register**, by turning on/off certain bits based of the result
of the computation
* conditional jumps read flags from this register ```%rflags/%eflags```
* data movement instructions leave flags unchanged

#### ZF
* ```zf``` = zero flag, set iff result of computation is zero
* ```jz```: jump if ZF(ZF == 1)
* ```je```: jump if ZF
* ```jnz```: jump if !ZF(ZF == 0)
* ```jne```: jump if !ZF, no difference between ```jz``` and ```je```

#### Flag-only arithmetic
* commonly find ```cmp``` and ```test``` instructions near conditional branches
* these instructions perform arithmetic, but ignore the result *except for flags*
* ```cmp SRC1, SRC2```: set flags based on ```SRC2 - SRC1```
* ```test SRC1, SRC2```: set flags based on ```SRC2 & SRC1```, ```test %rax, %rax``` <=> ```if %rax == 0```

#### Other flags
* ```SF```: set iff result is negative (when considered signed)
* ```CF```: set iff computation overflowed when considered as unsigned
* ```OF```: set iff computation overflowed when considered as signed

## Comparison conditional jumps
* usually associated iwth ```cmp```
* ```ja(jnbe)```: jump if greater, unsigned; same as the test ```!ZF && !CF```
* ```jg(jnle)```: jump if greater, signed; same as the test ```!ZF && (SF == OF)```

## Conditional move instructions
* ```cmovCONDITION SRC, DST```
* perform move only if condition holds

## Stacks and function-local storage
1. "red zone" -- callee locals
2. ```%rsp``` dividing pointer
3. callee's locals -- stack
4. entry ```%rbp``` 
5. ret address
6. argument storage (if any)
7. caller's locals

* ```pushq REG``` - moves stack pointer lower (SAVE)
* ```popq REG``` - moves contents of stack to a register, moves stack pointer to higher addresses (RESTORE)

### Calling convention and save/restore
* a function must restore certain registers (```%rsp, %rip```) to their entry-time values before returning
* caller-saved registers (most registers): caller cares about the value, it must save that value and restore it after the callee returns
* callee-saved registers: if the callee uses the value, it must save that value first and restore it before returning
