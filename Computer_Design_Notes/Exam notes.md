

# 0 - Course introduction


What:
- Detailed discussion of simple pipelined CPUs.
- Introduction to advanced techniques such as:
	- Out of order execution.
	- Branch prediction.
	- Speculative execution.
- Instruction level parallelism - focused processor cores.

Curriculum:
- Exercise materials (8) .
- Lecture slides (8).
- Computer Architecture,A Quantitative Approach, 6th (or 7th) Edition.
	- Chapter 1
	- Chapter 2.1, 2.2  and 2.3
	- Chapter 3 (except 3.2, 3.7, 3.11 and 3.12)
	- Appendix A
	- Appendix B
	- Appendix C (except C.6)


# 1 - Trends and performance

Chapter 1: Fundamentals of Quantatative design and analysis

### The computer revolution:

Moores law:
- is the observation that the number of transistors on a computer chip roughly doubles every two years.


### Classes of computers

Embedded computers:
- Components of systems.
- Restricted power/performance/cost constraints.
- Usually specific tasks and applications, and tailored to purpose.

Personal mobile devices (PMDs):
- Wireless devices. 
- Multimedia user interface such as cell phones, tablet computers.
- Stringent power/performance/cost constratins.

Desktop computers:
- General purpose.
- Variety of software.
- Cost/performance tradeoff.

Server computers:
- Network based.
- High capacity, throughput, reliability.
- Business applications.

Clusters/Warehouse - scale computers.
- Collections of desktop computers or servers connected by network to form a larger computer system.
- Price-performance oriented.

### Parallelism

Main way of improving computer performance.

Types of parallelism:
- Data-level parallelism (DLP); same operation on different data.
- Task-level parallelism (TLP); Divide work into tasks that can operate mostly in parallel.

Sources of parallelism:
- Instruction-level parallelism (ILP): Independent instructions that can be executed in parallel. Can support DLP and TLP.
- Vector processing / Single Instruction Multiple Data (SIMD): Single instruction being applied to multiple data elements at the same time. Supports mostly DLP.
- Thread-level Parallelism (TLP): Software organized as different threads that can operate mostly in parallel. Either communcicating to eachother or dividing a domain. Can support DLP and TLP.
- Request-level Parallelism: Parallel request-response server systems. Can handle many users with multiple requests. Supports mostly TLP.
- Memory-level Parallelism: Processor memory requests (loads and stores) can be issued in parallel to hide some of the latency. Supports either by overlapping independent memory accesses

Flynns Taxonomy:
- Way of categorizing computers.
- SISD: Single instruction stream, single data stream.
- SIMD: Single instructon stream, multiple data streams. Vector architectures, Graphic processor units (GPUs). 
- MISD: Multiple instruction streams, single data streams. No commercial implementation.
- MIMD: Multiple instruction streams, multiple data streams. Tightly coupled; multi-cores. Loosely coupled: Clusters, data senters.

### Computer architecture

Classical definition; The ISA:
- Instruction set architecture.
- Instruction set available to programmers and programming languages.
- How you get the computer to do stuff.
- RISC-V, x86, Arm and more.

ISA components and choices:
- Operand types. Typically registers, memory location or immediate.
- Memory addressing format. Typically an andress covers a 1 byte area.
- Addressing modes.
- Types and size of operands. (word, char, double word).
- Operations (data transfer, logical, control, floating point).
- Control flow execution (conditional branches, unconditional jumps, etc).
- Encoding (fixed or variable).

RISC-V instruction formats:
![[{FCAEF098-403E-404B-9FCC-9E8E9D97020B}.png]]

Genuine Computer Architecture definition:
- Design the organization and the hardware to meet objectives and functional requirements.
- Computer organization: High level aspects of computers design, such as memory system, interconnect design of CPU and more.
- Computer hardware: Detailed logic design and packaging of the computer.
- Computer architecture therefore cover the ISA, the organization and the hardware. 

### Software abstraction level

Application software:
- Written by programmer in High-level language.

System software:
- Compiler: Translates HLL to machine code.
- Operating system: Handling input/output, managing memory and storage, scheduling tasks, sharing resources.

Hardware:
- Processor, memory, I/O controllers.

Levels of program code:
- High level language: Provides productivity, portability and readability.
- Assembly language: Readable representation of compiled program into instructions.
- Hardware representation: Encoded instructions and data into bits.


CPU general requirements:
- Arithmetic.
- Control: Conditional or unconditional.
- Memory access: Read or write to memory hierarchy.

### Technology trends

Single core processor performance:
- Single-core performance growth has slowed dramatically, from roughly 52% per year during its fastest period to 3.5% per year near the graph’s end.

Latency vs. bandwidth:
- Bandwidth (how much work or data is handled per second) has improved much faster than latency (the time needed for one operation).

Clock frequency:
- CPU clock frequencies rose rapidly until the early 2000s, after which their growth largely flattened.

### Principles of quantitative design

Take advantage of parallelism:
- Memory, instructions, threads, request.

Leverage locality:
- Temporal locality: Programs tend to reuse same instruction/data close in time.
- Spatial locality: Programs tend to use close instructions/data to the previous.

Focus on the common case:
- Optimize for the frequent case, such as type of instruction or other pattern.
- Amdahls law: Massive improvement of infrequent case gives negligible total improvement.

Amdahls law:
- Dont expect proportional improvement in overall performance when improving one aspect (typically infrequent cases) of a computer.
- Time_improved = T_affected/ImprovementFactor + T_Unaffected.


### Measuring performance


What affects performance:
- Algorithm: Determines number of operations executed. 
- Programming language, compiler and architecture: Determine number of machine instructions per operation.
- Processor and memory system: Determine how fast the instructions are executed.
- I/O system (and OS): Determine how fast I/O operations are executed.

Response time:
- Turn around time (wall time): Time from issuing a command to completion, including waiting time.
- Response time alternate defintion: Time from issue to first response.
- CPU execution time: Time the processor spends executing work for the program, excluding time waiting for I/O or for CPU access.
	- User execution time: CPU time executing the program’s own code.
	- System execution time: CPU time executing operating-system code on the program’s behalf.
	- CPU time = user + system.

Throughput:
- Total work done per unit time.

Relative performance:
- P = 1 / T
- P_a / P_b = T_b / T_a = n
- A is n times faster than b.

Benchmarks:
- Should imitate or use collection of real applications.

CPU Clocking:
- Clock period: 250 ps = 0.25 ns = 250 \* 10 \^-12 s
- Clock frequency: 4.0 GHz = 4000 MHz = 4.0 \* 10\^9

CPU time:
- CPU_time = CPU_clock_cycles \* Clock_period
- CPU_time = CPU_clock_cycles / Clock_frequency
- CPU_time = Instruction_count \* CPI \* Clock_period
- CPU_time = (Instruction_count \* CPI) / clock_frequency

Performance improved by: 
- Reducing number of clock cycles.
- Increasing clock frequency.

Instruction count:
- Determined by program, ISA and compiler.

Average cycles per instruction (CPI):
- Determined by CPU hardware, and what instructions are used.
- Sometimes IPC is also used.

![[{530D64A8-249E-4CAF-A1D7-96BF88E18346}.png]]
![[{A4E3ADB8-5580-4745-84AE-2005F150D9C4}.png]]

The iron law:
- Cpu_time = \#Instructions \* CPI \* Clock_period
![[{7127B863-45A3-4EBA-AF3B-258B8CD451D6}.png]]
- Algorithm: Affects IC, Possibly CPI.
- Programming language: Affects IC, CPI.
- Compiler: Affects IC, CPI.
- Instruction set architecture: Affects IC, CPI, Clock cycle time.



# 2 - Instruction Set Architectures (ISAs)


Stored program Computers:
- Instructions are represented in binary just like data. Stored in memory.
- Programs can operate on and interact with other programs, such as compilers and linkers.
- Binary compatibility allows compiled programs to work on different computers. Standardized ISAs.

Instruction set:
- Collection of instructions a computer can execute.
- Both early and modern computers tend to have "simple" instruction sets.
- Differenct computers can have different isa but with common aspects.

RISC-V ISA:
- R-type: Two register inputs. *add, sub, and, or, xor, sll, srl, sra, slt, sltu*.
- I-type: One register and one immediate input. *addi, andi, ori, xori, slli, srli, srai, slti, sltiu, jalr* 
- S-type: Store type, store rs1 in rs2+imm address. *sw*.
- U-type: Can load upper 20 bits of a number into rd. 12 lower bits 0. *lui, auipc*.
- B-type: Branch type. *beq, bne, blt, bge, bltu, bgeu*
- J-type: Jump, optionally saving return address. *jal*
![[{5782A667-F9AE-4491-9C43-155F36CFB8FF}.png]]
- Opcode: Identifies the instruction family, such as arithmetic, load, or store.
- funct3, funct 7: Identify specific operation together with opcode.
- rs1, rs2: Source register numbers.
- rd: Destionation register number.
- imm: Constant value, included in the instruction.


### Classifying ISAs


How many registers:
- Depends, but quite a few. 16-32 architectural general purpose registers, 100+ physical registers.


Load-store (Register-register) architecture:
- 0 memory addresses, max 3 register operands.
- Simple fixed length instructions.
- Instructions also take similar number of clocks to execute.
- Higher instruction count, as you need seperate instructions for loading and storing values from memory. Also may lead to less of the program fitting in instruction cache.
- E.g: ARM, MIPS, RISC-V.

```asm
LOAD  R1, [A]       ; R1 = value at A
LOAD  R2, [B]       ; R2 = value at B
ADD   R3, R1, R2    ; R3 = R1 + R2
STORE [C], R3       ; save result at C
```


Register-memory:
- Max 1 Memory address, max 2 operands.
- Data can be accessed without seperate load/store instruction.
- Tends to have easy encoding and good instruction density.
- Intel 80x86.
- Cons: Two operand format often leads to values being overwritten. Asøp execution time varies as operations involving memory access can be slow.

```asm
LOAD  R1, [A]       ; R1 = value at A
ADD   R1, [B]       ; R1 = R1 + value at B
STORE [C], R1       ; save result at C
```


Memory-memory:
- Either max 2/3 memory address, and max 2/3 operands allowed.
- Most compact, and doesnt waste registers.
- Cons: Large variation in instruction size, and large variation in work per instruction. The memory accesses create memory bottleneck.

```asm
ADD [C], [A], [B]   ; value at C = value at A + value at B
```


### Memory addressing


Big endian (numbers in correct order):
- Least significant byte first. 
- Double word: 0123 4567

Little endian (most common):
- Most significant byte first.
- Double word: 7654 3210

Alignment:
- An address `A` of an object with size `s` is alligned if `A mod s == 0`.
- Simplfies implenentation. Improves speed by using some more storage.

Addressing modes:
- Register `add r1, r2, r3`.
- Immediate `addi r5, r4, 5`.
	- Immediate fields should be 8-16 bits.
- Displacement `lw x3, 8(x1)`.
	- Displacement should support 12-16 bits offset.
- Register indirect `lw x3, 0(x1)`.
- Pc relative `beq x1, x2, label`


### ISA operations:


Arithmetic and logical:
- Add, subtract, and, or, multiply, divide.

Data transfer:
- Load, store.

Control:
- Branch, jumps, traps, procedure call and return.

System:
- Operating system call, virtual memory management instructions.

Floating point:
- Add, multiply, divide, compare.

Decimal:
- Add, multiply, decimal to character conversion.

String:
- Move, compare, search.

Graphics: 
- Pixel and vertex operations.


![[{F759E511-B99C-4829-95E6-23AAB9A56C02}.png]]


Operand type and size:
- Opcode in the instruction tells what operand types.
- Integer size: 32- or 64- bit (word size).
- Floating point, 32 (float) or 64 bit (double).
- Characters: 1 byte ASCII or 2byte+ unicode.
- Decimal: Accuracy for business applications.


Control flow:
- Conditional branches. `beq x1, x2, label`
- Jumps. `jal x0, label`
- Procedure calls (jump but saved address). `jal ra, function`
- Procedure returns (return to saved address). `jalr x0, 0(ra)`
- Branches are typically less than 10 bits of displacement away. Larger than 12 bit must use absolute address from register (lui with 12 bit offset in jump).

```
@ --- Registers ---

@ x10–x11 (a0–a1): Return values
@ x10–x17 (a0–a7): Parameters passed

@ x1 (ra): Caller saved
@ x5–x7, x28–x31 (t0–t6): Caller saved / temporaries
@ x10–x17 (a0–a7): Caller saved

@ x8–x9, x18–x27 (s0–s11): Callee saved
@ x2 (sp): Stack pointer; callee saved

@ x0 (zero): Always zero; writes are discarded
@ x1 (ra): Return address (like ARM's LR)
@ x3 (gp): Global pointer; reserved
@ x4 (tp): Thread pointer; reserved
@ x8 (s0/fp): Optional frame pointer; callee saved

@ PC: Separate program counter, not one of x0–x31
```

Comparisons in branches:
- Not equal.
- Equal.
- Greater than or equal.
- Greater than.
- Less than or equal.
- Less than.


Memory layout:
![[{3741CDF1-41C8-49A5-A19D-028BD076F98D}.png]]



### Encoding an ISA


Overall summary of requirements:
- Load store.
- Addressing modes: Displacement, immediate and register indirect addressing modes.
- Support for 8/16/32 and 64 bit integers and 32/64 bit floating point.
- Need instructions for: 
	- Arithmetic.
	- Memory: Load store.
	- Pc relative conditionals.
	- Jump and link, with procedure return.


The encoding must balance:
- Many registers and addressing modes as possible.
- Instructions should be easy to decode.
- Fixed size instructions is preferred, 32 bits in riscv (for embedded devices, variable length instruction sets such as arm thumb are useful).



### The RISC-V ISA

Overall:
- General purpose register model, and load-store architecture.
- Supports displacement, immediate and register-indirect addressing.
- Supports 8-, 16-, 32- and 64-bit integers and 32- and 64-bit floating point data types.
- Focus on the simple, dominating instructions load, store, add, subtract, move register-register and shift.
- Branching and compare according to the analysis.
- At least 16 (preferably 32) registers. Orthogonal, minimalist ISA.

![[{69A33151-FE20-409F-9DE4-405F4A013C6F}.png]]
![[{FED2A542-E3FD-4D77-9039-D0819A596468}.png]]
![[{69E4BA7E-BAED-45E8-867A-5DEF1E4813C7}.png]]![[{F4D23D75-5116-4A22-842D-21622099A9EB}.png]]