

Limitations of scalar pipelines:
- Unifying instruction types is a problem
- Maximum IPC = 1.
- In-order execution.

Diversified pipeline:
- Higher clock frequency than unified pipeline.
- Multiple EX paths, divided into type of operation (INT, MEM, FP, BR).
- Problems: Out-of-order completion and multiple write operations to the register file in the same clock cycle. Also exceptions.
- Can cause WAW hazard.
![[{D7D48086-5F76-42F6-81F6-E746CB27C81B}.png|511]]

Interrupts:
- Due to external factors.
- Processing: Stop fetchin instructions, drain pipeline, store state, handle interrupt, restore state and resume execution.

Exceptions:
- Result of program execution (division by zero, page fault, overflow).
- Store the state just before the instruction that caused exception, handle exception, resotre state and resume execution.

Superscalar pipeline:
- Parallelism in time, pipelining.
- Parallelism in space, superscalar execution.
- Still inorder issue, so causes stalls before indepenedent instructions.

![[{B0BA7E25-1256-4FA4-96D9-4A19336099A4}.png|437]]
### Out of order execution


Idea:
- Remove all output- and anti depencences from the dynamic instruction stream through register renaming.
- Only real data dependences remain.
- Makes instructions execute as soon as their are available.

![[{8150C08D-B99E-4697-9846-0B0D7973C35A}.png]]

Superscalar OoO processor:
![[{FD57C987-978A-409A-A171-C780FF9F9A97}.png]]


Register renaming:
- Eliminates WAR and WAW register dependences (not RAW).

Dispatch:
- Inserts instructions in issue buffers and reorder buffer.

Issue buffer:
- Yet to execute instructions.
- Inserted in program order. 
- Execute on FU and leace the issue buffer possibly out of order

Reorder buffer:
- Keeps track of instructions currently under execution.
- Inserted in program order and leave the reorder buffer in program order.
- Maintains program order.

Complete:
- Architecture state updated, and to software the instruction appears to be fully executed.
- For non-stores: complete = retirement. 
- For stores: Moves data from store queue to store buffer.

Retire:
- For stores: Data written into memory hierarchy, leaving store buffer.

Sequentiality as an illusion:
- Software assumes instructions execute in program order.
- Harwdare exploits instruction level parallelism.
- Parallelism in time: pipelining. 
- Parallelism in space: superscalar execution and out of order execution.


### Register renaming

![[{4BC1DBC7-75E1-4650-A0AE-2649BCC848D9}.png]]
![[{549A85B6-649D-4F8E-9BB4-EBF00C86ECD1}.png]]

![[{18B2B598-48B8-477D-9D5B-E48CED794440}.png]]


Implementation:
- Registers mapped to archiitectural registers are in the "architectural" register state, other physical are "available".
- Input operands: Read the physical register that corresponds with the architectural register.
- Output operand: Select an "available" physical register, and change state to "rename register, value not computed".
- In the case that there are no more available physical registers, stall the pipeline until physical registers become available.
- When an  instruction finishes its execution on a functional unit; finish. State changed to "rename register, value computed".
- When an instruction leaves the ROB; Complete. Change the state  of the physical regiser to "architectural register", and the physical regiser previously associated with the same architecture register changes its state to "available".

![[{6A6D2159-6192-46C1-B1D6-765C246A4E10}.png]]![[{B6F91782-CBF2-4CA0-A912-A7FAE851E24C}.png]]
![[{38687162-DD3D-4F39-81DE-FAE42E38B4B8}.png]]

### Data flow execution (OoO)

![[{8AE4BD2C-DDF6-4BC3-B5BA-E8F5B5A32A22}.png]]
WAR:
- Writing to a register that is to be read by a previous instruction.
- Solved by register renaming.

WAW:
- Writing to a register that a previous instruction is going to overwrite. 
- Solved by register renaming.

RAW:
- Read after write instruction. 
- Yet to be solved.

### Dispatch:
- Allocate space and insert instructions into issue buffers.
- Allocate space in ROB.
- If no space in issuebuffer or reorder buffer, stall pipeline in frontend.

### Issue buffer / reservation station
- Buffering instructions waiting for input. 
- Once available, issue to functional unit.
- Decouples front-end from backend of the pipeline.
- How it works:
	- All input operands available-> ready = 1.
	- If ready and FU available; Issue.
	- When execution is done; Finish. Put the target register id and result on the forwarding bus.
	- Instructions watch the result bus for available registers.

![[{CC000FA5-FD89-464E-B917-4CDC0C6285D9}.png]]![[{CA021D98-6CA9-4CBC-BD82-1BCFD19A584C}.png]]
### Reorder buffer
- Contains all in-flight instructions.
- Includes all instructions between dispatch and before completion (issue buffer + ex).
- Circular buffer with head and tail pointer.
- Dispatch happens at tail,  new instructions per cycle is limited by dispatch width. In-order dispatch.
- Number of instructions leaving the ROB is limited by completion width. Happens at head, in-order completion.
![[{1526C6F0-0D95-46DB-BEF0-F9A1707FDCF3}.png]]

Instruction window:
- Combining the ROB and IB into one structure.
- Not common anymore, leads to large structure and HW complexity.

Exceptions:
- Recall: save architecture state prior to exception, handle exception restart execution from the faulty instruction.
- In OoO processors: Recall ROB. Instructions that generate exception set a flag in ROB entry, instructions prior to exeception can complete, exception is handled when exception instruction is about to complete.
![[{4039B2E4-EBFF-476B-A0AB-40ECEF55C3B8}.png]]

Mispredicted branches:
- Architectural and physical register mapping needs to be restored to state just after branch.
- More efficient solution:
	- Checkpointing: Snapshot of mapping table after each branch.
	- Restore to snapshot upon mispredicted branch.

Mispredicted branches:
- A branch prediction guesses which instructions the CPU should execute next.
- If the prediction is wrong, instructions from the incorrect path are discarded.
- Register mappings must be restored to the state just after the branch.
- Checkpointing: Save a snapshot of the mapping table after each branch.
- A wrong prediction can then be handled by restoring the saved snapshot.

Data captured scheduling:

- The reservation station holds instructions waiting to execute.
- It stores the actual input values when they become available.
- A functional unit (FU) broadcasts its result with a register tag.
- Tag = which register/result, such as `R1`. Data = its value, such as `6`.
- Waiting instructions recognize the tag and capture the value.
- Once their inputs are ready and a suitable FU is available, they can execute.

Non-data captured scheduling:

- The reservation station stores input register tags and readiness information, rather than actual input values.
- Actual values are stored in the physical register file.
- A tag notification tells waiting instructions that an input is ready.
- After selection, instructions obtain their values from the register file or bypass paths.

Advantages of non-data captured scheduling:
- A simpler reservation station because it does not store full input values.
- Register tags require fewer bits than actual register values.
- Less wiring is needed between functional units and reservation-station entries.

Pipelined organization:
- Scheduling, reading registers, and execution are divided into pipeline stages.
- Reservation station: Holds waiting instructions.
- Selected instructions: Instructions chosen to proceed toward execution.
- Register file: Supplies their input values.
- Opcodes + inputs: The operation to perform, such as addition, together with its inputs.
- Functional units: Perform the operations and produce results with destination tags.

Wakeup and bypassing:
- Wakeup: Notify a waiting instruction that an input is becoming available.
- With known execution timing, a selected instruction’s destination tag can wake dependent instructions before its result is produced.
- The actual result must arrive by the time the dependent instruction executes.
- Bypassing/forwarding: Send a result directly to a functional unit’s input, without waiting for another register-file read.
- The orange `=` symbols compare register tags to find the required result.
- Old results: Temporarily stored recent results that can also be forwarded.

Execution example:
- The instructions depend on one another in this order: `R1 = R0 × 2`, then `R2 = R1 × 2`, then `R3 = R2 + R1`.
- Cycle 0: All three instructions are waiting. `R0` is already available.
- Cycle 1: The instruction producing `R1` is selected. Its destination tag wakes the instruction needing `R1`.
- Cycle 2: The first instruction has its inputs and moves toward execution. The instruction producing `R2` is selected.
- Cycle 3: The `R1` result becomes available and reaches the second instruction through bypass path 1. The instruction producing `R3` is selected.
- Cycle 4: The third instruction receives the new `R2` result through bypass path 1 and the older `R1` result through bypass path 2.
- For example, if `R0 = 3`, the calculations produce `R1 = 6`, `R2 = 12`, and finally `R3 = 18`.
- Bypassing reduces waiting, but the instructions still need the correct results from earlier instructions.

Limits of dataflow execution:
- Register renaming removes WAR and WAW register dependencies.
- RAW dependencies remain: Some instructions need results produced by earlier instructions.
- Without predicting those results, dependent instructions must wait for them.
- These dependency chains limit how quickly the program can execute, even with many functional units.

Value prediction:
- Predict an instruction’s result before it has actually been calculated.
- Can be used for results from loads and arithmetic operations.
- Dependent instructions can start earlier using the predicted value.
- The prediction must later be checked against the actual result.
- If incorrect, the CPU must recover and re-execute the affected work.

Value locality:
- Value prediction works because some instructions repeatedly produce the same value or follow predictable patterns.
- Repeated value: Repeatedly loading an unchanged value from memory.
- Predictable pattern: A loop counter producing `0, 1, 2, 3, …`.
- The CPU can track previous values and their stride—the difference between successive values—to predict future results.

Lecture summary:
- Scalar pipelines have a maximum IPC of 1 and can stall independent instructions behind a blocked instruction.
- Different instruction types benefit from different execution paths.
- Register renaming removes WAR and WAW register hazards.
- Issue buffers allow ready instructions to execute out of order.
- The reorder buffer (ROB) ensures instructions commit in program order, supporting precise exceptions.
- Out-of-order execution improves performance at the cost of more complex hardware.