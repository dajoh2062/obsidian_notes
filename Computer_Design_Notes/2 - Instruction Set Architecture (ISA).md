
Stored program computers:
- Instructions represented in binary just like data, in memory.
- Programs can operate on programs.
- Binary compatibility allows compiled programs to work on different computers.

Instruction set:
- The collection of instructions of a computer.
- Different computers have different instruction sets, but with similarities.
- Early computers had very simple ISAs, and some modern still do.

![[Screenshot 2026-09-11 at 21.42.53.png]]

High level view on ISA design:
- Focused on the "common case".
- No such thing as perfect ISA.



### Classifying instruction set architectures


![[Screenshot 2026-09-11 at 21.47.39.png]]

### Register-based ISAs

ISAs from the 80s onwards are commonly load-store register ISAs:
- Accessing registers is faster than memory.
- Registers introduce fewer unnecessary dependencies than accumulator and stack ISAs.

How many registers:
- Depends on implementation, usually 16-32 general purpose per core (total usually 70-100 per core).
- More registers give longer instructions because more bits are required for the address in the instruction. 
- if a CPU has more registers available, the compiler has an easier time deciding where to keep temporary values. Decreases memory access needed.


![[Screenshot 2026-09-11 at 21.54.54.png]]
![[Screenshot 2026-09-11 at 21.56.57.png]]


### Memory addressing

Big vs. little endian:
- Ordering or bytes within a double word (0 is LSB and 7 is MSB, 8 bytes total)
- Little endian: 7 6 5 4 3 2 1 0
- Big endian: 0 1 2 3 4 5 6 7

Alignment:
- An address A of an object is aligned with its own size.
- Simplifies implementation and speed.
- Smallest size in general is usually a byte.

![[Screenshot 2026-09-11 at 22.16.15.png]]

![[Screenshot 2026-09-11 at 22.17.45.png]]


![[Screenshot 2026-09-11 at 22.32.27.png]]

![[Screenshot 2026-09-11 at 22.33.01.png]]

Addressing modes summary:
- ISA should support displacement, immediate and register indirect addressing.
- 75% - 99% of addressing used in experiments.
- Displacement address should be 12 to 16 bits (75-99%).
- Immediate fields should be between 8 to 16 bits. 8 bits captures about 85% of floats and 65% of integers.
- LUI (load upper immediate) instruction can support 32 bit immediate. 

### ISA operations
