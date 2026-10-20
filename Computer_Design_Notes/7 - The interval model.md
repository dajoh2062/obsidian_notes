
What is Computer science?
![[{FFDFC701-4A0D-4D85-91CD-0A8640BEB34E}.png]]
Evaluation requirements:
- Validity: How well are you measuring.
- Reliability: Are the result stable and consistent.
- Reproducibility: Do you provide enough information for your independent researchers to verify findings.
- Other issues: Implementation must be correct, and models must be sound.


Computer architecture Evaluation options:
- Analytical modeling: Early studies and large design spaces, black-box vs. white-box models. You can quickly explore thousands of possible combinations of cache size, processor width, and memory latency. These combinations form the design space.Uses knowledge of how the processor works to construct equations (white box) and Predicts performance from relationships between inputs and outputs, without explicitly describing the internal mechanisms (black box).
- Simulation: A simulator represents processor components and follows how instructions move through them. You change the simulated cache size and run a program to see what happens. Its advantage is capturing interactions. For example, a cache miss does not necessarily stop all execution: an out-of-order processor may execute other independent instructions while waiting. A detailed simulator can represent that overlap.
- Experiments on real hardware: Reliability: Background programs, temperature, and changing clock speeds can make repeated runs differ. Validity: An apparent cache benefit might actually come from another difference between the processors you compare. Effort: Testing a new cache design may require building new hardware.

Sum:
- Analytical model: Explore many ides and quickly identify promising designs.
- Simulation: Investigate selected designs and examine behaviour and interactions.
- Hardware experiments: Check physical behavior to see what happens in actual implementation.


Interval analysis:
- An analytical performance model for out-of-order processors.
- Mechanistic (white-box) and not empirical (black-box).
- Models the mechanisms in the processor that lead to performance realted behavior, such as cache miss and branch mispredict.
- Empirical models differ as they use a alot of data and program behaviour to use machine learning to find relationships in performance.
- Both can be accurate and used together (hybrid), but mechanistic modeling gives improved understanding of architecture.

Sniper simulator:
- Sniper uses interval modeling to simulate CPU performance.
- Predictions have up to about 25% error in the reported results.
- Simulation runs about 200× faster than detailed cycle-accurate simulation.
- Useful for quickly studying systems where CPU core details are not the main focus.
![[{4DF8BE3F-6E1F-4B7C-929F-0E5028C14C0A}.png]]

### Interval modeling


Alpha 21264 cpu example:
- Can fetch 4 IPC and execute upto 6 IPC.
- But SPEC benchmarks shows 1.5-2.5 useful IPC and Database application shows 0.5 useful IPC.
- Why the difference (possible factors)?
	- Limited ILP (instruction-level parallelism): Instructions depend on each other, so they cannot all execute together.
	- Instruction-cache misses: The CPU waits for instructions.
	- Branch mispredictions: The CPU follows the wrong path, discards that work, and restarts on the correct path.
	- Data-cache misses: Instructions wait for required data.

![[{3424ACFE-449A-419B-9D40-3B2C5F28BF17}.png]]

Is ILP limiting factor?:
- The instruction window holds instructions waiting to execute.
- With more instructions to choose from, the CPU is more likely to find independent work.
- The graph therefore shows potential IPC increasing as window size increases.
- The slide’s answer is no, assuming a balanced processor design with large enough window size, enough ALU and other resources and minimal cache miss/ branch penalties.
- Total execution time in cycles, assuming balanced design with B the width of  processors and N total instruction count: \[C] = \[N]/\[B]



Instruction cache misses?:
- If f4 is correctly predicted and next instruction therefore is g, but I-cache miss occurs, assume miss latency of 10 cycles. The instructions in pipleline and instruction window completes, then we wait for the g instruction to enter the frontend pipeline window / execution. 
- This makes extra execution time per I-cache miss similar to the rigid I-cache miss latency (shown in simulator).
- New equation: \[C] = \[N]/\[B] + \[M_IL1] \* \[L_IL1] + \[M_IL2] \* \[L_IL2]
- New equation: \[Cycles] = \[Number of instructions]/\[Width of processors] + \[Misscount I-L1] \* \[Penalty I-L1] + \[Misscount I-L2] \* \[Penalty I-L2]


Branch mispredictions?:
- If f4 is incorrectly predicted taken, the CPU fetches and may execute instructions from further loop iterations instead of g. When f4 executes, the CPU discovers the mistake and discards the instructions from the wrong path. It then fetches g, which must pass through the frontend pipeline before entering the instruction window / execution.
- This makes extra execution time per branch misprediction approximately branch resolution time + frontend pipeline refill time.
- Branch resolution time is the time from f4 entering the instruction window until it executes.
- New equation: \[C] = \[N]/\[B] + \[M_IL1] \* \[L_IL1] + \[M_IL2] \* \[L_IL2] + \[M_br] * (\[C_res] + \[L_fe]).
- New equation: \[Cycles] = \[Number of instructions]/\[Processor width] + \[Miss count I-L1] \* \[Penalty I-L1] + \[Miss count I-L2] * \[Penalty I-L2] + \[Branch misprediction count] * (\[Branch resolution time] + \[Frontend refill time]).

Data cache miss?:
- Short-latency misses (L1 miss):
	- Short L1 D-cache miss delays can be mostly hidden through out-of-order execution.
	- This requires enough independent instructions and sufficient processor resources.
	- Therefore, the model does not add a separate penalty for short L1 D-cache misses
- Long-latency misses (L2/L3 miss):
	- If c2 also misses in L2, the CPU requests the data from DRAM, assuming a latency of 250 cycles. The outstanding request is tracked in an MSHR (Miss Status Holding Register). Independent instructions continue executing, but c2 blocks in-order completion, so later instructions cannot retire. The instruction window fills up, preventing new instructions from entering. When the data returns, c2 completes and execution can progress again.
	- Unlike an I-cache miss, which eventually empties the instruction window, a long D-cache miss causes it to fill up.
	- The CPU can hide only a small part of this long delay.
	- Extra execution time for one isolated L2 D-cache miss ≈ L2 miss latency, which is the DRAM access time here.
	- Memory-level parallelism (MLP) can help overlapping some of the waiting times for different data. Requires MSHR and DRAM bandwidth. The instructions must also be within W distance.
- New equation: \[C] = \[N]/\[B] + \[M_IL1] \* \[L_IL1] + \[M_IL2] \* \[L_IL2] + \[M_br] * (\[C_res] + \[L_fe]) + \[M_non-overlapping_DL2] \* \[L_DL2]
- New equation: \[Cycles] = \[Number of instructions]/\[Processor width] + \[Miss count I-L1] \* \[Penalty I-L1] + \[Miss count I-L2] \* \[Penalty I-L2] + \[Branch misprediction count] * (\[Branch resolution time] + \[Frontend refill time]) + \[Non-overlapping D-L2 miss count] * \[Penalty D-L2]

CPI stack:
- Dividing every part by number of instructions gives a CPI stack, showing contribution of Cycles per instruction.

![[{809AFD97-E82A-4FA6-9EE4-F8F532DB9932}.png]]
Insights from interval modeling:
- Not limited by ILP: Assuming balanced processors, OOO execution hodes short latency I-cache misses.
- Limited by mispredicted branches: Cost equal to branch resolution time + front-end refill time.
- Limited by I-cache misses: Cost equal to access count and access time to next level in hierarchy. Independent of pipeline depth.
- Limited by D-cache misses: Only long latency misses, measured by non overlapped count and penalty. MLP helps performance as long as DRAM and MSHR is sufficient.

![[{2EF307CA-3AA4-409B-8740-D6AA5F956CD3}.png]]![[{C001CAB5-AC70-4D97-AFB0-3034D4571230}.png]]

OOO vs In-order Architectures:
- In order: execution follows program order, determined by software (programmer/compiler).
- Limitations: An instruction stall blocks entire pipeline.
- Examples: IBM Power6 (general purpose), Intel Atom (Embedded), Sun niagara (Application specific).
- Advantages: ILP analysis is done by compiler, analysis in hardware is limited. Hardware is less complex allowing higher frequency and less power consumption.

![[{2A0553B3-52C2-464E-A88A-2B48C93343B5}.png]]

Why is OOO better?
- Hides inter-dependences better.
- Hides short execution latencies better like L1 d-cache misses.
- Cost of mispredicted branch is higher though, because of branch resolution time.
- Cost of higher level cache is also smaller on OOO by using MLP.
- Cost for instruction cache misses is identicall.

Sum:
- Interval modeling estimates performance from normal execution plus major delays.
- A balanced CPU needs enough backend resources to keep up with the frontend; buffer sizes must grow roughly with the square of pipeline width.
- Main delays: branch mispredictions, instruction-cache misses, and long data-cache misses.