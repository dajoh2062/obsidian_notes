

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


