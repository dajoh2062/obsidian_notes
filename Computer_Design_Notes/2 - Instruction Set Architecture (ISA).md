
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
![[Screenshot 2026-09-13 at 11.06.26.png]]![[Screenshot 2026-09-13 at 11.07.14.png]]

Operand size is typically determined by the operation:
- Integer arithmetic: 32 or 64-bit words.
- Floating point: Single or double precision IEEE (32 or 64 bit).
- Characters: 8 bit ascii or 16+ bit unicode.
- Business applications: binary code decimal.

![[Screenshot 2026-09-13 at 11.10.48.png]]

### Instruction for control flow

![[Screenshot 2026-09-13 at 11.15.28.png]]

![[Screenshot 2026-09-13 at 11.19.06.png]]

Basic block:
- Sequence of instructions with no embedded branches (except at the end).
- And no branch targets (except at beginning).

Types of control flow:
- Conditional branches.
- Jumps (unconditional branches).
- Procedure calls.
- Procedure returns.

Specifying branch-addresses:
- Explicit: Commonly offset to the PC. Advantage is the branch target is usually close in location, and branch is independent of where the program was loaded in memory.
- Implicit: Useful when the branch target is not known at compile time. Use register or other addressing mode to specify location.

Branch distance:
- Usually short under 10 bits of discplacement from PC.
- If branch target is too far away to encode with a 13-bit signed offset you cannot use the branch instruction directly. Branches have a 12 bit immediate. 
![[{6D6B25FF-3598-4139-B9D7-424D613B0CD0}.png]]
How to specify the branch target:![[{41FA1C1D-827D-46C5-BEB5-89E3E8BEA0E1}.png]]
Common comparisons in this compiler + architecture:
![[{A7EE32E8-3133-4E5C-9A64-E8BB2578BD2B}.png]]
There are two main types of registers:
- Caller-saved registers: If the caller wants their values preserved, the caller must save them before the call. The callee is allowed to overwrite them.
- Callee-saved registers: If the callee wants to use them, the callee must first save their old values and restore them before returning.

Memory layout:
- Text: Program code.
- Static data: Global variables. 
- Dynamic data / heap: Allocated memory.
- Stack: Automatic memory.![[{C2F49EE9-C131-4557-AF36-FBF9B23F7212}.png|182]]
ISA observations:
- Leaning towards a load-store architecture.
- Displacement, immediate and register indirect addressing modes.
- Support 8-, 16-, 32- and 64-bit integers, and 32- and 64-bit floating point.
- Need instructions for: 
	- Simple operations (arithmetic, load, store, etc.).
	- PC-relative conditional branches.
	- Jump and link instructions for procedure calls.
	- Register indirect jumps for procedure return.

ISA Encoding:
- Need to balance the desire to have many registers and addressing modes, smaller programs with fewer needed addressing modes and instructions being easy to decode.
- Embedded devices have limited memory, and having 32 bit fixed instructions waste memory.
- Response: Variable length instruction sets.

### Compilers
![[{CE13024E-6C8E-48AA-B2A2-52D1D34D508F}.png]]
Performance impact:
![[{AE6A3A7F-8EAF-471C-A181-3CEB6D9E14B9}.png]]

How can Architects help compilers 
- Provide regularity: ISAs should be orthogonal/independent. That means all instructions support all addressing modes.
- Provide primitives, not solutions.
- Simplify trade-offs between alternatives.
- Provide instructions that bind quantities known at compile time as constants.

### The RISC-V Isa

RISC-V Choices: 
- General purpose register model.
- Load-store architecture.
- Supports displacement, immediate and register-indirect addressing.
- 8, 16, 32 and 62 bit integers or 32/64 bit floating point data types.
- Focus on the simple dominating instructions load, store, add, subtract, move register-register and shift.
- Branching and compare according to the analysis.
- At least 16, but preferably 32 registers.
- Orthogonal, minimalist ISA.
- Our Dialect/focus is RV32I. Functionality includes base 32-bit integer instruction set with 32 registers.

Operand Types:
![[{F41306D5-5F0A-476F-9317-27970855D7F4}.png|595]]
![[{57598F1E-FF34-453A-B091-94FA9BBC1AC7}.png]]

RISC-V data transfer instructions:![[{EFC31C01-829B-486F-B121-CA98CA1D9099}.png]]


RISC-V ALU Instructions:![[{A6D6442D-8331-4BC3-8978-BE191CCC418B}.png]]
RISC-V Control instructions:
![[{2E42BFE5-5696-4B86-AC14-CC38E53FCA70}.png]]