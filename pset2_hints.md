# Pset 2 hints
## Emily's Demo (Section 3)
* movzwl helps provide hints on what type
* read gdp pages on course site
* gdb lets you stop a program between instruction so you can look inside registers

1. objdump bomb assembly code
2. set breakpoints to stop explosions

* put objdump assembly code in its own txt file
* write own pseudocode based off assembly code
* focus on phase code
* phases have inflection points, where if something is not true the bomb explodes
* trace back inflections
* need gdb to look inside registers (layout regs)
* set breakpoint: b *0x
* gdb ./bomb -- opens program in gdb
* r -- run until  you hit a breakpoint
* store breakpoints in external file that you can pass into gdb

1. gdb bomb -- start up gdb
2. info b -- get information about breakpoints, afterward hit n if prompted "make breakpoint penidng on future shared library load?"
3. b explode_bomb -- breakpoint 1 at explode_bomb
4. tui e -- GUI
5. layout regs -- shows registers
6. si -- single step
7. k -- kil program being debugged
8. layout n -- different views of looking at debugger, next line about to be run will be highlighted
9. ctrl + l -- cleans up tui display

## Christina + Abe's Section 4
1. gdb fun01 -- start up gdb 
2. b no_fun -- set breakpoint at no_fun
3. r -- run program
4. will hit breakpoint
5. r "!" -- will run program with input
6. vim .gdbinit -- create gdbinit file

gdbinit file:
- set confirm off // stop confirm asks
- layout asm // see assembly
- layout regs // shows register values
- b main // set breakpoint at main
- b no_fun // set breakpoint at no_fun

7. gdb -tui fun01 // start up tui
8. r // will start program
9. k -- kills prgoram, starts at beginning again
10. c -- continue until you hit a breakpoint

### Abe's hints
1. b fun -- set breakpoint at fun
```cmpb $0x0, (%rdi)``` - compares (first 8 bits) 0 with the value of %rdi
```movsbl 0x1(%rdi), %eax``` - mov signed from byte to long(4 bytes -> 8 bytes and sign extention) takes address stored in %rdi and add one (offset), put in %eax

**check if %rdi = 0**  
```\test %rdi, %rdi```
```je```
