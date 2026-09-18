
CPU performance factors:
- Instruction count, determined by ISA and Compiler.
- CPU and Cycle time, determined by hardware.

Three example implementations:
- Single cycle. 
- In order pipelined.
- Out-of-order superscalar and pipelined.

Instruction Execution:
1. Pc -> instruction memory, fetch execution. Compute next PC (target address or PC + 4).
2. Register numbers -> register file, read registers.
3. Depending on instruction class:
	- Use ALU to calculate:
		- Memory address for load/store.
		- Branch target address.
		- Arithmetic result.
4. Access memory if necessary.
5. Write result to register file if necessary.


CPU overview:
![[{0510CB2E-D6C8-4BE1-89E0-452488EC9DAA}.png]]

### Logic design Repetition:

Information encoded in binary:
- Low voltage = 0, High voltage = 1.
- One wire per bit.
- Multi-bit data encoded on multi-wire buses.

Combinational element:
- Operate on data.
- Output is a function of input. 

State elements:
- Store information.
![[{D615E41E-8E4E-4A51-B263-AD2EA241D7F7}.png]]

Registers:
- Stores data in a circuit.
- Uses clock signal to determine when to update the stored value.
- Edge triggered: update when clock changes from 0 to 1.
- Register with write control: updates on clock edge when writ control is 1.
![[{9266D360-7850-474C-8991-887E705498D8}.png]]
![[{5FBCB519-E438-400C-B5F1-20A83F4E32B5}.png]]

Clocking methodology:
- Combinational logic transforms data during clock cycles.
- Between clock edges,
- Input from state elements, output to state element.
- Longest delay determines clock period.

### Building a Datapath:


Datapath:
- Elements that process data and addresses in the CPU; registers, ALUs, MUXs, memory and so on.

Instruction fetch:
![[{626288D1-7543-49A8-B860-F04F0B4ABBEA}.png]]

ALU instructions:
1. Read two register operands.
2. Perform arithmetic/logical operation.
3. Write register result.
![[{06A6EA8A-2AE3-4B73-993D-09C8844430FF}.png]]

Load/store instructions:
1. Read register operands.
2. Calculate address, use alu with sign extend offset. 
3. Finish instruction. 
	- Load: Read memory and update register.
	- Store: Write register value to memory.
![[{42CA4FCC-0694-4845-8ABC-FE0C1CDC2C58}.png]]

Branch instructions:
1. Read register operands.
2. Compare operands: Use ALU, subtract and check zero output.
3. Calculate target address:
	- Sign extend displacement.
	- Shift left by to places (word displacement).
	- Add to pc +4, already calculated by instruction fetch.

![[{8F8CED78-A9B6-48D7-A642-510B3F1DF15A}.png]]

### Composing the Elements

First cut data path does an instruction in one clock cycle:
- Each datapath element can only do one function at a time.
- Hence, we need seperate data and instruction memory.
- Use multiplexers where alternate data sources are use for different instructions.

![[{EAF278B3-B1EE-4B53-B117-6E88D098DF08}.png]]

### ALU control

ALU used for:
- Load/Store: F = add
- Branch: F = subtract
- R-type: F depends on the funct field

![[{258599EF-8008-4159-ACC1-EF10E611482C}.png]]![[{37F5CBB6-44D4-4E13-943E-0A8E8EA34F14}.png]]

![[{924DFE49-94AD-4C84-9623-AC6399D3C02A}.png]]![[{89F02C4F-0BEE-422C-9247-77F4C6A60299}.png]]

![[{40CDE784-2582-4796-B721-D3592045EDA5}.png]]

![[{1445763D-694D-4F37-9FAF-AB0BCAAD12D0}.png]]![[{9E6426FE-B8C9-4584-B6E8-86072A5E7C3F}.png]]
Implementing jumps:
- Jump uses word address.
- Update PC with conceatenation of top 4 bits of old PC, 26-bit jump address, 00.
- Need extra control signal decoded from opcode.

![[{46D1A031-FC17-4B40-948E-66D855BFB456}.png]]


### Single cycle performance issues

Longes delay determines clock period:
- Critical path: load instrucion.
- Instruction memory -> register file -> ALU -> data memory -> register file.

Not feasible to vary period for different instructions:
- Violates principle: "common case fast".

Possible improvments:
- A multi-cycle implementation (not so common).
- Pipelining (very common).

### Control Units

Combinatorial control units:
- Lacks state.
- The single cycle processor has a combinatorial control unit. State is determined by current instruction.

Sequential control units:
- Larger and more complex than combinatorial control units.
- Can use smaller combinatorial units internally. 

### Pipeling basics

Laundry analogy:
- Without pipelining: each laundry load completes all 4 steps before the next starts, so resources sit idle and 4 loads take 8 hours.
- With pipelining: different loads do different steps at the same time, so the washer, dryer, folding, and putting-away stages work in parallel.
- Each individual load still takes 2 hours from start to finish, but the throughput increases because loads overlap.
- For 4 loads, pipelining reduces total time from 8 h to 3.5 h, about 2.3× faster.
- As the number of loads becomes large, the speedup approaches the number of pipeline stages: about 4× here.
- Main CPU analogy: pipelining does not necessarily make one instruction finish faster; it lets multiple instructions be processed at the same time in different stage

Pipelining big picture:
- Does not improve latency.
- Increases throughput as long as enough instructions to fill the pipeline.


![[{0BDE58A2-360C-43F9-9E48-5425EDA683B3}.png]]
RISC-V Datapath:
![[{5AC5B183-AB3A-4A57-B134-64285E362002}.png]]
RISC-V Datapath with Pipeline registers:
![[{8B871C23-175F-4455-B568-9EBD2E4DBD6A}.png]]

### Pipelining hazards

![[{0FFB9ED1-DE3D-49DE-96B8-53395407F657}.png]]

Program dependences:
- Three dependeces:
	- Data dependence through memory.
	- Data dependence through registers.
	- Control dependence.
- A hazard is a result of not respecting program dependences.

Data Dependence:
- Real dependence = read after write (RAW).
- Anti dependence = write after read (WAR).
- Output dependence = write after write (WAW).

Hazards due to memory dependences: 
- Answer: No, because all access to memory executes sequentially (only the mem stage accesses memory). 


Hazards du to data dependences:
- WAW: No, only WB writes, so in program order and sequential.
- WAR: No, reading is done in OF/ID stage, writing is done in WB stage.
- RAW: Yes, may happen if instruction reads and old value from the register file.

RAW hazard naive solution:
- Pipeline stall: too much penalty.

RAW hazard better solution:
- Forwarding: Forwarding sends a result directly from a later pipeline stage to the next instruction that needs it, instead of waiting for the value to be written back to the register file first.



![[{93CFCD69-8F8F-45EE-9BAA-1650C7F7090D}.png]]

Hazard due to control dependence:
- Cpu reaches a branch and doesnt yet know which instruction to do next.
- Example: beq x1, x2, target

Naive solution to control dependence hazard:
- Stall/wait for instruction result.

Better solution:
- Branch prediction + speculative execution.
- Prediction is right: 0 cycle penalty.
- Prediction is wrong: Discard wrong path instructions and fetch from correct path. Cycle penalty depends on how many pipeline stages contain wrong-path instructions when the branch is resolved.

Optimal pipeline depth:
- A deeper pipeline means you split instruction execution into more, smaller stages. Because each stage does less work, each clock cycle can be shorter, so the CPU can use a higher clock frequency. Higher cost due to mispredictions.

