# Lecture 5
## Undefined Behavior and Computer Arithmetic

### Undefined Behavior
* specification (language standard) imposes requirements on implementation and on user of implementation
* compiler trasnlates any program that obey's specification rules  

As-if rule: the compiler will eliminate a function or calculation that is redundant (no observable output) and can figure out a cheparer way to perfrom a computation.  
* undefined behaviro is not the same as implementation behavior

### Santization
* compile with ```make SAN=1```
* sanitizers are efficient checkers that catch many undefined behaviors
* sanitizers are built into compiler and add more assertions (decorate lines with assertions) that catch undefined behavior, but this does slow down runtime

### Computer Aritmmetic
bitwise operate is one that operates on hte bits of its input(s) independently
* ~ = complement
* & = and
* | = or
*  ^ = exclusive or
*  a << i = moves bits in a left by i places, shifting in 0s (left shift by i = multiplying by 2^i)
*  a >> i: move bits in a right by i places (right shift by i = dividng by 2^i)

```-x = (~x+1)```  
```x & (2^i - 1) = x % 2^i```
* bitwise operators allow us to efficiently represent small sets
