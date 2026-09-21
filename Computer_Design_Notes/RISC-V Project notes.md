
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
	
```


```asm


```


