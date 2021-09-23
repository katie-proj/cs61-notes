# Lecture 7
## Basics of Assembly

### Machine code and assembly
* computer processor/CPU reads instructions (few bytes) from memory that tell the processor what to do
* instructions have a byte representation (**machine code**) and textual representation (**assembly language**)
1. CPU reads instructions from memory
2. CPU decodes each instruction and performs the corresponding operation before going to the next
3. CPU executes instructions sequentially unless redirected explicitly by an instruction (a branch instruction--like ```goto```)

* source code = C++ code programmer writes, which is passed to compiler
* compiler --> operations on source code and generates assembly file, which is passed to assembler
* assembler --> object file .o has machine code
* linker --> generates executable in machine code

### Assembly code
left column: address or offset at which code appears
middle column: machine code representation
right colummn: assembly language
* assembly code will add extra garbage instruction to ensure 16-bit size

### directives, labels, instructions
directive: an isntruction to the assembler; controls aspects of the output that aren't machine code  
* .file -- what source file was
* .text -- which segement should store the generated instructions
* .global, .type -- information for the linker about the function

label: marks the next instruction, making it referenceable by other instructions and files  
instruction: assembly language  

### Common Instructions
Three classes of instruction
* arithmetic: perform computation on values
* data movement: move data to and from pirmary memory
* control flow: change the instruction sequence

#### ret (return)  
```ret``` returns from the current function, a control flow instruction

#### endbr64 (compiler flags)  
- beginning of the program

#### mov 
- ```mov``` instruction is a data movement instruction
- FORMAT: ```mov SRC, DST```

#### Registers
- registers comprise the fastest kind of memory available to the CPU
- maachines have tons of memory but few registers, x86-64 has just 14 general-purpose register that are each 64 bits wide
- registers have no addresses, but rather names (e.g. ```%rax``` is the register that holds return value), so they cannot be dereferenced using a numeric address/ptr

#### Register slices
- although rgisters are 64 bits wide, the data we handle is often smalller, so names are provided for slices of each register
```%rax``` = entire register (bits 0-63)  
```%eax``` = the lowest 32 bits (bits 0-31)  
```%ax``` = the lowest 16 bits ( bits 0-15)  
```%al``` = the lowest 8 bits (bits 0-7)  
```%ah``` = bits 8-15  
- instructions must match sizes (e.g. ```movl``` used for 32-bit, ```movq``` used for 64-bit)  
- function's first parameter is passed into ```%rdi``` register  

#### Data Operands and Address modes
- ```$X``` = immediate value (constant)
- ```%X``` = register value
- ```a(%rip)``` = global symbol
- ```(%X)``` = indirect reference (dereferencing a pointer)
- ```8(%X)``` = offset indirect reference (dereferencing a structure or array)

#### Arithmetic (computation) instructions
- ```OP SRC, DST``` = DST := DST OP SRC
- ```xorl %eax, %eax``` = ```%eax := %eax ^ %eax``` (exlcusive or, returns 0)

#### Moving into register slices
- ```mov[SIGN][SRCSIZE][DSTSIZE]```: SIGN ```z``` (extend with zeroes) or ```s``` (extend with sign bit); 
