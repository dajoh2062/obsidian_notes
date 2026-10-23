
Scoreboarding:
- Alternative to register renaming to hinder WAR and WAW hazards, but also RAW hazard.
- Stalls instructions more oftern than RR.
- Two stages needed for issue and operand fetch.
- Scoreboard keeps track of which registers are being used as RD in active instructions.
- Instructions in issue can issue if: required functional unit is available and no active instruction has same destination register. This takes care of WAW hazards.
- Instruction can fetch its operands when both operands are available. Meaning no in flight instructions are writing to the instructions input registers. Takes care of RAW.
- Functional unit executes instruction and notifies scoreboard when execution is complete.
- Instruction cannot write back to the register file if preceding instruction has yet to fetch operands, and one of the operands is this functions RD. Takes care of WAR hazards.
![[{B13DA078-B52F-4319-A3AB-52A5644B2CC9}.png]]
How Tomasulo works
- Instructions enter reservation stations.
- Operands are stored as values if ready, or tags identifying pending results.
- Instructions execute when their operands and an execution unit are ready.
- Results are broadcast to waiting instructions and registers.
- Register renaming removes WAR and WAW hazards; RAW dependencies require waiting.


Compared with scoreboarding:
- Scoreboarding: A central scoreboard tracks dependencies; Tomasulo distributes tracking across reservation stations.
- Scoreboarding: Stalls issue for WAW hazards and writeback for WAR hazards; Tomasulo avoids both through renaming.
- Both: Allow out-of-order execution while preserving RAW dependencies.

### Out-of-order vs. In-order performance


In-order cores still prevelant:
- Instruction execution in program order, determined by software (programmer/compiler).
- Limitation: An instruction stall blocks entire pipeline.
- Examples: IBM power 6 (general purpose), Intel atom (embedded), application specific (Sun Niagara).

In-order advantages:
- Less complex hardware.
- Higher clock frequency and less power consumption.
- ILP analysis is done by compiler and limited in HW.

Performance comparison:

![[{D9C36904-94BF-4A7E-9463-B2CC9B988C3E}.png]]
What makes OOO perform better:
- Hides instruction dependeces better using ILP.
- Hides non-unit execution latencies better such as short-latency D-cache misses.
- Cost of mispredicted branches is however bigger on OOO because of branch resolution time.
- Cost of L2/L3/LLC is smaller because of MLP.
- Cost for instruction cache misses are the same.


### Architectural design options:

Instruction ordering by stage:
- Issue, dispatch, execution and completion (commit/WB).
- The degree where instructions can deviate from program order is heavily dependent on the microarchitectural techniques implemented.
- Most important is that hazards are handled.

Processor design tricks, can be assembled in different ways:
- Dataflow execution (example Tomasulo).
- Scoreboarding.
- Prediction and speculation.
- Caching.
- Register renaming.

Alternative Scoreboard based Microarchitechture:
- Issue/OF: An instruction can issue when both ops are available and FU. Takes care of RAW hazards.
- WB: Result is written to rename register. Increases parallelism because register file is used for buffering and not the registers in FU. Rename register is promoted to arch register when the instruction is the oldest in-flight.
![[{97A1AF64-4C33-4ADD-8175-DCBB92C7D792}.png]]

Dependency chains:
- Sequence of dependent instructions.
- Many dependency chains follow this pattern: 
	- Some instruction compute address of a load (Address Generating Instructions / AGIs).
	- Data is loaded from memory.
	- Some computation.
	- Result is written back to register / memory.
- Within loops, we can overlap steps 3 and 4 of one iteration with steps 1 and 2 of later iterations. 

Performance potential:
- Observation 1: Memory hierarchy Parallelism is key to achieving performance.
- Observation 2: Allowing loads + AGIS to go out of order relativ to the rest of instruction stream  closes a significant part of performance gap between in-order stall-on-use and dataflow out-of-order.
![[{20219E2B-717B-46B3-9676-50E89B2E9E4E}.png]]

Iterative Backward Dependency Analysis (IBDA):
- Helps identify each loads AGI.
- When we see a load instruction or previosly identified AGI, mark the instruction that this instruction depends on as AGI.
- For each iteration of loop, dthe dependency chain grows by one instruciton.

The load Slice Core (ISCA 15):
- Loads and AGIs go to the Bypass (B) instruction queue.
- All other instructions go the A instruction queue.
- Stores go to both queues to avoid hazards though memory.
- Instructions are executed in order within each queue but (potentially) out-of-order across queues.
  ![[{032D3348-08AC-4CFE-BCB3-21E623CAE8DB}.png]]


Take aways:
- Computer design is a bag-of-tricks, and anything goes as long as program is executed correctly.
- Can use different combinatios of techniques to make different microarchitectures, with different strengths (performance, size, cost, efficiency).

