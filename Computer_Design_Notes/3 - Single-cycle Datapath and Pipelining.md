
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


