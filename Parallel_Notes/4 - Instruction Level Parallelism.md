### Pipelining

Why:
- Improved switching frequencies made it tricky to complete one complex instruction each clock cycle.
- Early RISC architectures decided to break each operation into 5 stages to keep up:
	- IF -> ID -> EX -> MEM -> WB
- One instruction still takes 5 steps, but two instructions take 6, three takes 7 and so on.

![[{E495AF38-A333-4E1E-9983-B75FC1D581D2}.png|501]]
Stall and flush:
- Stall = Stop and wait. Flush = Throw away and restart from the correct path.
![[{6DE86BEB-A491-4240-8678-A4CBD20FB215}.png|485]]
### Out-of-order execution

Bernstein's conditions:
- Two statements will produce the same result no matter the order if;
![[{AC90617E-6CFA-48C4-8AE0-3CD537D291A7}.png]]
Dependences:
- Data dependence, Name dependence and Control dependence.

Data dependence:
- The result of an operation is an input to a following operation.
![[{8F6B2A1D-F342-4FA7-98BC-3B796167828F}.png|381]]
Name dependence:
- When a name is re-used for a different purpose, its first use must be finished before the second can begin.
- Programmers usually dont reuse variable names, and compilers fix most of it anyway. 
- Loop iterations can create this effect.
- At the instruction level, the CPU has a limited number of registers to use, they have to be recycled every so often.
![[{2512ABCD-7CA4-413C-A4A1-75192907F21D} 1.png|275]]
Control dependence:
- Branches in the program make it impossible to start operations simultaneously.
![[{8FDF5466-4354-4205-8E0D-421CD614626F}.png|219]]
Superscalar computer:
- A processor that uses multiple execution units to execute multiple independent instructions in parallel during the same clock cycle.

### Prefetching and branch prediction

- Programs often spend a lot of CPU time inside long loops.
- Loops commonly access memory in regular patterns.
- Example: accessing array elements one after another creates predictable memory addresses.
- CPUs can use these predictable patterns to prepare data and instructions in advance.

Prefetching

- Regular memory access can cause cache misses when execution reaches the end of a cache line.
- Prefetching detects patterns such as repeatedly accessing addresses with the same stride.
- The CPU starts loading upcoming data from memory before it is actually requested.
- This can reduce waiting time caused by cache misses.
- If the prediction is wrong, the prefetched cache space can simply be reused later.

Branch Prediction

- Branches such as if statements and loop conditions make it difficult to know which instructions should be loaded next.
- The CPU predicts whether a branch will be taken or not taken.
- A simple predictor may always guess taken or always guess not taken.
- Correct predictions keep the pipeline full and improve performance.
- Wrong predictions require incorrect instructions to be flushed from the pipeline.

More Advanced Branch Prediction

- Branch predictors can use previous branch results to improve future predictions.
- A 2-state predictor can adjust its prediction based on the previous branch result.
- A 4-state predictor can use more branch history before changing its prediction.
- More states allow the CPU to respond more carefully to occasional unusual branch outcomes.
- CPUs can also store branch history and statistics associated with branch instruction addresses.
### Vectorization
- Performs the same operation on multiple values at once.
- Uses SIMD: Single Instruction, Multiple Data.
- Common with arrays, matrices, graphics, and scientific calculations.
- Improves speed when many independent values need the same operation.

