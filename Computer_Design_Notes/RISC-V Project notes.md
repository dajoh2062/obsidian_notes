
### Registers

- x0 (zero): Always contains zero; writes are ignored.
- x1 (ra): Return address—where to continue after a function returns. Caller-saved.
- x2 (sp): Stack pointer—tracks the top of the stack. Must be restored before returning.
- x3 (gp): Global pointer—helps access global/static data. Normally left unchanged.
- x4 (tp): Thread pointer—helps access data belonging to the current thread. Normally left unchanged.
- x5–x7 (t0–t2): Temporary registers. Caller-saved.
- x8 (s0 / fp): Saved register; can also serve as a frame pointer for the current function’s stack frame. Callee-saved.
- x9 (s1): Saved register. Callee-saved.
- x10–x11 (a0–a1): Function arguments and return values. Caller-saved.
- x12–x17 (a2–a7): Additional function arguments. Caller-saved.
- x18–x27 (s2–s11): Saved registers. Callee-saved.
- x28–x31 (t3–t6): Additional temporary registers. Caller-saved.
### Instructions

```asm

@ adds immediate and x2 result in x1
addi x1, x2, 5

@ adds values in x2 and x3
add x1, x2, x3


@ compares signed values x1 and x2, and puts 1 in x12 if x1<x2, 0 otherwise
slt x12, x1, x2
slti ,x12, x1, 10

@ compares unsigned values x3 and x3, puts 1 in x12 if x3<x4, 0 otherwise
sltu x12, x3, x4
sltiu x12, x3, 19


@ shift x1 by x2 amount left, filling with 0s, 0011<<2 -> 1100
sll x14, x1, x2
slli, x14, x1, 2

@ shifts right x1 by x2 amount, filling left with 0s.  1100>>2 = 0011
srl x14, x1, x2
srl, x13, x1, 2

@ shifts x1 by x2 amount right, copying sign bit
sra x14, x1, x2
srai x14, x1, 2


@ bitwise or operation
or x15, x8, x9
or, x14, x4, 6

@ bitwise and operation
and x15, x8, x9
andi, x14, x7, 4

@ bitwise xor operation
xor x15, x8, x9
xori, x15, x2, 5


@ load value from memory into x3 from address 8(x12)
lw x3, 8(x12)

@ Store value in x3 to memory address given by 8(x12)
sw x3, 8(x12)	
```


Arithmetic flow:
- IF: Gets the next instruction by sending PC to IMEM, then PC+=4. Delays PC in IFbarrier to match instruction timing.
- ID: recieves instruction. Id gives registeraddress 1 and 2 to registers, which returns value in registers. ID sends instruction to decoder, that returns correct controlsignals (Regwrite Y, Memread N, Memwrite N, branch N, Jump N), Op1select (rs1), Op2select (rs2), Immtype (dont care) and ALUop (ADD/SUB/SLT/SLTU/SLL/SRL/SRA/AND/OR/XOR). ID then outputs readData1, readData2, rd, immediate, controlsignals, op1Select, op2Select and ALUop.
- EX: Recieves readdata1, readData2, rd, immediate, controlsignals, op2Select, ALUop. A mux chooses second operand based on op2Select value. ALU recieves ALUop, op1 and op2 and performs operation and returns result. Execute then outputs aluResult, rdOut, controlsignalsOut and storeData (value in readdata2 for sw).
- MEM: Recieves aluResult, rdOut, controlsignalsOut and storeData. For arithmetic functions, mem simply forwards the data to WB. 
- WB: returns writeData, writeAddress and writeEnable to ID, and writes to register.

Immediate aritmetic flow:
- IF: Gets the next instruction by sending PC to IMEM, then increments PC by 4 each cycle. IFBarrier delays PC by one cycle and passes the instruction through directly. Its delayed PC output is currently unused.
- ID: Receives the instruction and reads the register file. readData1 contains the value of rs1. The second read port also returns a value, but it is unused for these instructions because there is no rs2 operand. The decoder returns control signals (regWrite Y, memRead N, memWrite N, branch N, jump N), op1Select = rs1, op2Select = imm, immType = ITYPE, and the appropriate ALU operation (ADD/SLT/SLTU/SLL/SRL/SRA/AND/OR/XOR). ID extracts instruction bits [31:20] and sign-extends them to a 32-bit immediate. IDBarrier registers readData1, readData2, rd, immediate, controlSignals, op2Select, and ALUop before passing them to EX. op1Select is output by ID but is not connected to EX.
- EX: Receives the values from IDBarrier. The first ALU operand is readData1. The MUX selects the immediate as the second operand. The ALU performs the selected operation; shift instructions use only the lowest 5 bits of the immediate as the shift amount. EX outputs aluResult, rdOut, controlSignalsOut, and storeData. storeData contains readData2, but is unused for these instructions.
- MEM: Receives aluResult, rd, controlSignals, and storeData. No memory write occurs, and memory read data is unused. MEM registers the ALU result, destination register number, and control signals, then passes them to WB.
- WB: Selects the ALU result as writeData because memRead = N. It returns writeData, writeAddress = rd, and writeEnable = regWrite to ID, where the register file writes the result to the destination register (unless it is x0).

SW flow (sw x5, 8(x2) → stores the value of x5 at address x2 + 8):
- IF: Fetches the instruction from IMEM and increments PC by 4.
- ID: Reads rs1 (base address) and rs2 (data to store). Extracts and sign-extends the S-type immediate. Decoder sets memWrite Y, memRead N, regWrite N, op2Select = imm, and ALUop = ADD. IDBarrier registers the values for EX.
- EX: ALU adds readData1 + immediate to calculate the address. readData2 is forwarded separately as storeData.
- MEM: Writes storeData to DMEM at address aluResult.
- WB: No register write occurs because regWrite = N. The forwarded rd value is unused.

LW flow (lw x5, 8(x2) → loads the value at address x2 + 8 into x5):
- IF: Fetches the instruction from IMEM and increments PC by 4.
- ID: Reads rs1 (base address), extracts rd, and sign-extends the I-type immediate. Decoder sets memRead Y, memWrite N, regWrite Y, op2Select = imm, and ALUop = ADD. IDBarrier registers the values for EX.
- EX: ALU adds readData1 + immediate to calculate the address. storeData is unused.
- MEM: Reads DMEM at address aluResult and sends the memory data to WB, alongside the registered rd and control signals.
- WB: Selects memory data because memRead = Y, then sends it back to ID’s register file to be written into rd (x5).