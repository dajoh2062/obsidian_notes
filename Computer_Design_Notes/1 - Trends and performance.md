
### The computer revolution

Progress in computer technology underpinned by Moore's Law

Makes novel applications feasible:
- Computers in automobiles
- Cell phones
- Human genome project
- World Wide Web
- Search engines

### Classes of computers

Internet of things/Embedded computers:
- Components of systems.
- Usually power/performance constraints

Personal Mobile Devices / PMDs:
- Cell phones, smart watches, tablets, laptops.

Desktop computers:
- General purpose.
- Cost/performance tradeoff.

Server computers:
- Network based.
- High capacity, throughput and reliability.

Cluster/Warehouse scale computers.
- Collections of desktop computers or servers connected by a a network to form a larger scale computer. 
- Usually high price/performance.

### Improving performance with Parallelism

Types of parallelism:
- DLP:  Data-level parallelism, same operations but with different data.
- TLP: Task-level parallelism, divide work into tasks that can operate in parallel to some degree.

Sources of parallelism in computers:
- Instruction-Level Parallelism (ILP): Independent instructions can be executed in parallel. 
- Vector processing or Single Instruction Multiple Data (SIMD) parallelism: A single instruction can be applied to multiple data elements at the same time.
- Thread-Level Parallelism (TLP): Software can be organized as different threads that can operate in parallel (commonly threads communicate).
- Request-Level Parallelism: In request-response server systems, software can process tasks in parallel (threads don’t explicitly communicate but may access shared data structures).
- Memory-Level Parallelism (MLP): Processor memory requests (loads and stores) can be issued in parallel to hide memory latencies

### Flynn's Taxonomy

SISD:
- Single instruction stream, single data stream.
- One processor executes one instruction on one piece of data at a time.

SIMD:
- Single Instruction, Multiple Data streams.
- GPUs, Vector processors.
- The same instruction is applied to many data values simultaneously.

MISD:
- Multiple instruction streams, single data stream.
- No commercial implementation.

MIMD:
- Multiple instruction streams, multiple data streams.
- Tightly-coupled MIMD: multi-cores, many-cores
- Loosely-coupled MIMD: clusters, data centers.

###  Classical Computer architecture definition (ISA)

The Instruction Set Architecture (ISA):
- The actual, programmer visible instructions set.
- In other words: The language you can use to get the computer to do stuff.
- We will focus on RISC-V, alternatives are x86, ARM, etc.

ISA components and choices:
- Operand types (typically registers or memory locations).
- Memory addressing (typically byte-addressing).
- Addressing modes (e.g., register, immediate, displacement).
- Types and sizes of operands (word, char, double word, etc.).
- Operations (data transfer, logical, control or floating point).
- Control flow instructions (conditional branches, unconditional jumps, etc.).
- Encoding (fixed vs. variable length).
![[{3FE092A0-5AB4-45E0-86A9-BE5E4389E8EF}.png]]

### Genuine Computer Architecture Definition

Objective:
- Design the organization and the hardware to meet objectives and functional requirements.

Computer organization:
- High-level aspects of a computer’s design.
- Examples: Memory system, interconnect, design of the CPU.

Computer hardware:
- Detailed logic design and packaging of the computer.

Definition:
- Computer architecture covers the ISA, the organization and the hardware.


### Below Your Program

Application software:
- Software written in High-level languages (HLL).

System Software:
- Compiler: Translating HLL code to machine code.

Operating System:
- Handling input/output.
- Managing memory and storage.
- Scheduling tasks and sharing resources.

Hardware:
- Processor.
- Memory.
- I/O controllers.


### Levels of Program Code

High-level:
- Abstraction closer to problem domain.
- Provides productivity and portability.

Assembly language:
- Textual representation of instructions.

Hardware representation:
- Binary digits, bits.
- Encoded instructions and data.

![[{9E7B89DC-DA45-4A5C-8B53-6189295F9B09}.png]]

CPU:
- Arithmetic: Performs operations on data.
- Conditional branch: selects next instruction depending on branch outcome,  enables the parts of the data-path needed to execute a given instruction.
- Memory access: Off chip access is often slow. solution: cache.


### Single-Core Processor Performance Growth

![[{217F26E5-1DED-46CD-A327-A9235A372CC8}.png]]
![[{D7CD3363-150A-4998-87D8-E645425DCA2A}.png]]

![[{83697EE5-99C8-4298-BF29-0E33F3CF32E7}.png]]

### Principles of Quantitative Design

Take advantage of parallelism:
- Exploiting parallelism is the pervasive strategy for improving the performance of computers.

Leverage locality:
- Programs tend to reuse data and instructions, think of loops and arrays.
- This results in reuse over time (temporal locality) and in (memory) space (spatial locality).

Focus on common case:
- Favor the frequent case over the infrequent case, increases total performance.
- Amdahl's Law: Massively improving an infrequent case gives negligible improvment.

Amdahls Law:
- T_Improved = T_Affected/ImprovementFactor + T_Unaffected
![[{D9E7E159-37D4-4898-AD72-AB9D0D176845}.png]]

### Understanding performance:

Algorithm:
- Determines number of operations executed.

Programming language, compiler and architecture:
- Determine number of machine instructions executed per operation.

Processor and memory system:
- Determine how fast instructions are executed.

I/O system (and may include the OS):
- Determines how fast I/O operations are executed.

Response time/Turn around time:
- Time from issuing a command to its completion.

Execution time:
- The time the processor is busy executing the program.
- Turn-around time includes the time the process waits to be executed, execution time does not.
- Also: user execution time vs. system execution time.

Throughput:
- Total work done per time unit. 


### Relative Performance

Definition: Performance = 1 / Execution Time

![[{306E64B5-B513-4418-8D63-F9F5263ECA8F}.png]]

### Measuring Execution Time

Elapsed time / "Wall clock time":
- Turn around time, including all aspects: processing, I/O, OS overhead, idle.
- Determines system performance.

CPU Time:
- Time spent processing a given job: Discounts I/O time, other jobs shares.
- Combines user CPU time and system CPU time.
- Different programs are affected differently by CPU and system performance.

### Benchmarks:

Must use real applications to test:
- Different CAs techniques pay off in different phases.

Must use a collection of real applications:
- A computer is general purpose and should perform across many applications.

Benchmark suites:
- SPEC CPU, SPEC OMP, PARSEC, NAS, TPC, etc.
- Each suite attempts to be representative for the workloads of a chosen domain.

Discredited benchmarking approaches:
-  It is too easy to cheat if the benchmarks are simple.

### CPU Clocking:

Operation of digital hardware governed by a
constant-rate clock.
![[{6CB0FFEE-1C03-42A4-A0BA-8F432C578E58}.png]]

Clock period:
- Duration of clock cycle.
- Example: 250 x 10^-12 seconds.

Clock frequency (rate):
- Cycles per second.
- Example: 4.00GHz = 4.0 x 10^9 Hz.
![[{CB430590-79E4-4BD7-BDA7-1864CA7EA3BB}.png]]
Performance can be improved by:
- Reducing number of clock cycles.
- Increasing clock rate.

![[{F863EBB6-6875-453C-848D-CAE9E180BC0A}.png]]
![[{0CADBA58-181C-4587-BACA-840B95F3F9E0}.png]]

![[{336EDC4E-BB5E-4454-8157-8A4690DB0524}.png]]
![[{F48B5FC6-83EE-4AA3-B131-E4F31FD96264}.png]]
![[{4845F25D-1F77-4F83-B38C-157C15DC2C92}.png]]
![[{85E546B0-5FDC-4D78-BA91-944BD7A4E53B}.png]]
