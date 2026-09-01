![[{476B2365-5D34-4665-AC5F-49F960E5856F}.png]]

### Memory:

Addressing:
![[{342470E5-982C-49EB-AB40-E6B4D277FA0A}.png]]
The interconnect is an interface that lets external devices
get numbers from memory:
![[{0D528236-9B53-440D-80D2-34802C49EB77}.png]]
Temporary results are made (semi-)permanent
by copying them back into memory:
![[{E043B7DB-1DE2-420D-8530-FC3D48FCC85A}.png]]
The von Neumann architecture:![[{06AB34C2-898A-4D0C-B096-1A661195A17C}.png]]
Machine instructions are derived from more readable text in some programming language:

![[{6DDC53B9-1168-45DB-A067-A909B2122B9B}.png]]
Dynamic and allocated memory:
- Stack for function arguments and local variables.
- Program code places data of dynamic size in heap.
![[{476B2365-5D34-4665-AC5F-49F960E5856F} 1.png]]
![[{0ACEE757-184D-4792-BCC7-0B020A4BAE03}.png]]

Abstract computer, sequential processing model:
- A program to run.
- A translation mechanism that separates data and instructions.
- An instruction-interpreting unit.
- A data-manipulating unit.


Strength - The von Neumann computer is a bridging model:
- Programmers can improve performance for every computer.
- Hardware designers can improve performance for every program.
- NB: No bridging model for parallel computing.

Weaknesses:
- Memory bottleneck: Programs repeatedly read from and write to memory, so performance is limited by slow memory access even when the CPU is much faster.
- Sequential execution: A von Neumann machine mainly executes instructions one after another, so faster performance requires parallelism once clock-speed improvements stop.
- Inefficient memory/program model: All memory is treated as one large address space, with no built-in distinction between active and inactive data, and the basic model only runs one program at a time.

The fallout:
- We write program for the von Neumann style, and then detect parts that should be done in parallel; either manually or automatically. 
- Can only improve system performance by adapting the code to the computer it targets.

We will discuss:
- Multiple collaborating processes (distributed memory).
- Multiple instruction streams in one process (shared memory).
- Multiple operations in one instruction (vector operations).
- Multiple processor types in one program (hybrid programming).

Invisible adaptions in modern systems:
- Cache memory.
- Virtual memory.
- Instruction level Parallelism (ILP).





