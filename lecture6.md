# Lecture 6
## Representation of Code
```objdump -d```: takes instructions in binary form and translates it to assembly
* name of instruction defines behavior, argument of instruction defines paramters
* ```%rdi, %rsi``` = register that stores **arguments** to function
* ```%eax``` or ```%rax%``` = register that stores **return value**, eax is 4 bytes and rax is 8 bytes
* ```retq``` = return instruction, takes no arguments
