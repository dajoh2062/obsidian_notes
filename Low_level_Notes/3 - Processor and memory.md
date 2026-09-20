
### CPUs

Two main computer architectures:
- Von Neumann: Shared instruction and data memory, typically used in general purpose computers.
- Harvard: Seperate memories for instructon and data.

![[{5BF1FF21-9239-45EB-BB8D-DFAB2920B625}.png]]


Main processor functions:
- Fetch instructions from IMEM.
- Decode operations and locate operands.
- Execute; Read operands, perform opertation and store result.

Single cycle design:
- Each instruction is one clock cycle.
- Other components are idle while one is active.
- Inefficient and slow.

Pipelined processor design:
- Only one stage per instruction per cycle.
- Faster cycles but some more cycles required in total.
- Overlapping instructions improves hardware utilization and performance.
![[{559EA409-1A07-4D0E-B9F7-B954450AC1E7}.png]]

Performance comparison:
- Pipelined processor is upto 3x performance of single cycle.
- Execution = Instruction count x CPI x Cycle time
- Cycle time in pipelined is 1/3 of single cycle as fetch, decode and execute can run simultaniously.

Limitations to pipeline performance:
- Data dependencies: An instruction needs the result of previous instruction.
- Control dependencies: An instructions execution depends on the outcome of a branch instruction.
- Hazards: Dependency leads to incorrect execution, if not handled properly.
- Pipeline needs to be paused to avoid hazards, thus lowering CPI and performance.
- Branch penalty: Pipeline gets flushed on wrong execution path choice.

Avoiding control hazards:
- Stalling until the branch is resolved: Correct, but wastes cycles.
- Branch prediction: Predict the next instruction address and continue fetching. If the prediction is wrong, flush the instructions from the wrong path and fetch from the correct address.
- Branch delay slots: In architectures that support them, a fixed number of instructions after a branch execute regardless of its outcome. The compiler fills these slots with useful, safe instructions, or inserts

Avoiding data hazards:
- Forwarding (bypassing): Send the result directly from the producing pipeline stage to the instruction that needs it, before it is written back to the register.
- Stalling: If the result isn’t ready even with forwarding, pause the dependent instruction. This introduces a bubble into the pipeline.
- Instruction scheduling: Place independent instructions between the producer and consumer, giving the result time to become available. Reordering must preserve the program’s meaning.

Performance boosting techniques:
- Out-of-order execution: Reorder instructions to minimize stalls.
- Superscalar processors: Fetch, decode and execute multiple instructions per cycle.
- Multithreaded processors: Execute multiple instructions on parallel.
- Other: Caching, prefetching, vector execution...

### Memory

Memory requirements:
- Large, fast and high bandwidth.
- Solution: Memory hierarchy.

![[{371AB2A6-3CA8-4499-9CFD-7F35BEDCB174}.png]]

SRAM:
- Fast access, no capacitator.
- Lower density, only 6 tranistors per cell.
- Higher cost.
- No refresh needed.

DRAM:
- Slower access, capacitator.
- Higher denisty, one transistor and 1 capacitator per cell.
- Lower cost and bigger.
- Requires refresh.

![[{23DC0430-3589-435C-B368-B7E9FA955430}.png]]
Speeds and sizes:
![[{934AAAA6-DAE2-4349-8E28-3B8CA6852A70}.png]]

Why is memory hierarchy effectire:
- Temporal locality: A recently accessed memory location (location or instruction) is likely to be accessed again in the near future.
- Spatial locality: Memory locations (instructions or data) close to a recently accessed location are likely to be accessed in the near future.
- Examples: Loops, functions, arrays, variables, objects.
- Hierarchy gives the impression of a single, large and fast memory.



### Cache


Block/Line: 
- 32-128 bytes of data.

Hit:
- Data is found in cache.

Miss: 
- Data not found in cache.
- Must continue the search in the next level of hierarchy.
- Added to cache when located.

Hit rate:
- Fraction of accesses that are hits at a given hierarchy level.

Hit time: 
- Time required to access a level of hierarchy.

Missrate / miss ratio:
- 1 - hit rate.

Miss penalty:
- Time required to fetch a block into some level from the next level down the hierarchy.

Accessing caches:
- A direct-mapped cache stores memory data in fixed-size blocks, with one block per cache line. Each memory block maps to exactly one possible line.
- The memory address is split into tag, index, and offset.
- Index: Selects the cache line to check.
- Tag: Compared with the line’s stored tag to check whether it contains the requested block.
- Offset: Selects the requested byte within that block.
- Hit: The line is valid and its tag matches. Miss: Otherwise, fetch the required block from the next memory level, replacing the selected line’s contents if necessary.
- Calculate offset bits = log₂(bytes per block), index bits = log₂(number of lines), and tag bits = remaining address bits.




