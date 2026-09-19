

Moving immediate values to registers:

```asm
mov r3, #val  @val can be 0 to 255

mvn r3, #val  @val can be -256 to 1

@ For larger values:

ldr r3, =val  @ val can be any 32 bit valuem loads value instead of addres
```

Conditionals:
```asm
@ Equivalent to: if (a < b) x = 1; else x = 2;

.section .text
    ldr r0, =vars       @ r0 = address of vars
    ldr r1, [r0]        @ r1 = a
    ldr r2, [r0, #4]    @ r2 = b (4 bytes after a)

    cmp r1, r2          @ Compare a and b by setting condition flags
    bge else            @ If a >= b (signed comparison), jump to else

    mov r2, #1          @ If a < b, set result to 1
    b endif             @ Skip the else block

else:
    mov r2, #2          @ Otherwise, set result to 2

endif:
    str r2, [r0, #8]    @ Store result in x (8 bytes after a)

.section .data
vars:
    .word 1             @ a = 1; offset 0
    .word 2             @ b = 2; offset 4
    .word 0             @ Space for x; offset 8, initially 0

@ Each .word occupies 4 bytes (32 bits).
@ With a = 1 and b = 2, the result is x = 1.

```


Loops:
```asm
@ Equivalent to: while (a != b) { /* loop body */ }

.section .text
    ldr r0, =vars       @ r0 = address of vars
    ldr r1, [r0]        @ r1 = a
    ldr r2, [r0, #4]    @ r2 = b (4 bytes after a)

loop:
    cmp r1, r2          @ Compare a and b by setting condition flags
    beq end             @ If a == b, exit the loop

    @ Loop body goes here
    @ Update r1 or r2 here if a or b should change

    b loop              @ Jump back to check the condition again

end:
    @ Execution continues here after the loop

.section .data
vars:
    .word 1             @ a = 1; offset 0
    .word 2             @ b = 2; offset 4

@ The condition is checked BEFORE each iteration.
@ With a = 1, b = 2, and an empty body, this loop runs forever.
@ Changing r1 or r2 does not automatically update a or b in memory.

```


Function calls:
```asm
bl label
```

- set r14 (Link register) to PC + 4
- set PC to label
- Returning from function:
	- Needs to move LR to PC: 
```asm
mov r15, r14
```


```asm
@ Equivalent to: int add(int a, int b) { return a + b; }
@ Called with: add(5, 10);

@ Calling convention:
@ r0 = first argument (a), and also the return value
@ r1 = second argument (b)
@ r14 = lr (link register): holds the return address
@ r15 = pc (program counter): controls where execution continues

add:
    add r0, r0, r1      @ r0 = a + b; place the result in r0
    mov r15, r14        @ Return by copying lr into pc (as shown on slide)
    @ bx lr is the usual return instruction to use in Thumb-2

main:
    mov r0, #5          @ Pass x = 5 as the first argument
    mov r1, #10         @ Pass y = 10 as the second argument
    bl add              @ Save return address in lr and branch to add

    @ Execution resumes here with r0 = 15
    @ This is a call example; main's own return code is omitted.

@ bl = branch with link: calls a function and records where to return.
@ b = branch: jumps without recording a return address.
@ Parameters and results ARE passed through shared registers.
@ Caller and callee agree on which registers to use.
```

ARM Register Convention for Function Calls
- Arguments: First four 32-bit integer arguments go in r0–r3; additional arguments use the stack.
- Return value: r0 for a 32-bit integer; r0–r1 together for a 64-bit integer.
- Caller-saved: r0–r3 and r12 may be overwritten; the caller saves them if needed.
- Callee-saved: r4–r11 must be restored by the called function if modified.
- Special registers:
- r13 = sp: stack pointer.
- r14 = lr: return address.
- r15 = pc: program counter.
- r12 = ip: temporary scratch register.
- Function calls: bl saves the return address in lr; bx lr returns. A function making another call must preserve its own return address.

Nested function calls:
```asm
@ Problem: every BL overwrites LR (the return address).
@ If add calls addNum without saving LR, it loses its return to main.

@ Fix: save LR on the stack before the nested call.
add:
    push {r4, lr}      @ Save return address; r4 keeps 8-byte alignment
    bl addNum          @ Call addNum; overwrites lr
    pop {r4, pc}       @ Restore r4; return using saved address

addNum:
    add r0, r0, r1     @ Calculate result in r0
    bx lr              @ Return to add
```


Stack:
- Region of memory, last in first out.
- R13/SP points to newest position address.
- Grows downwards from higher address to lower address.

```asm
push {r4, r5} @ push data onto stack
```

```asm
pop {r2} @ pop data from stack
```


![[{52066A2C-EEAB-47E7-A33E-0C9DBA888390}.png]]
Nested function call using stack:
```asm
@ Nested function calls: simplified lecture version
@ Problem: BL overwrites LR, losing the original return address.
@ Fix: save LR on the stack before calling another function.

add:
    push {lr}          @ Save return address to main (4 bytes)
    bl addNum          @ Call addNum; overwrites lr
    pop {pc}           @ Return to main using the saved address

addNum:
    add r0, r0, r1     @ Calculate result in r0
    bx lr              @ Return to add

@ Saving one register is enough to preserve the return address.
@ The lecture sets aside the separate 8-byte stack alignment rule.

```


Usecases of stack:
- Saving calle-saved registers (r4 - r12) if needed for temporary computations.
- Can be used to pass parameters and return values.
- Used for local variables within the function.

![[{AF8CC20E-2F34-413A-A80A-2652F89BAF5E}.png]]

### CISC vs RISC ISA

Complex Instruction Set Computer:
- Register-memory architecture, like x86.
- Computers programmed in assembly.
- Few registers, operands can be in memory.
- Very little memory, variable length instructions to save memory.

Reduced Instruction Set Computer:
- Used in ARM, MIPS, SPARC, RISC-V.
- High level languages and compilers were already in fashion.
- More registers, more memory, faster clock -> fixed length instructions.
- Load-store (register-register) architecture.

![[{E5FDCEA5-B65B-4DAE-A3A3-26EE1CFADBB0}.png]]
![[{1B85319A-18DF-43F7-AF0E-911A5F4ED10C}.png]]


Connecting I/O devices to CPU:
- Data register: holds data that is either read from the device for input or holds data that needs to be writen to device for output.
- Status regiser: status of read/write operation.
![[{669A7506-E3F8-42E0-84B3-3718C42CD9D4}.png]]

![[{46937B88-1550-43E3-B6E8-8F0C2D2D7A0B}.png]]
Memory mapped I/O device:
- IO controller registters are mapped to dedicated portion of memory.
- Regular load/store instructions can be used to access IO device.
- CON: Used memory.

Isolated I/O space:
- I/O devices are mapped in seperate memory space.
- Special instructions to access I/O device.
- A processor can support both memory mapped and isolated.


How to check I/O status for ready:
- Polling (busy-wait IO): CPU monitors IO continuously or regularly to find if it is ready. Wastes CPU time especially for infrequent events. CPU reacts fast.
- Interrupt: IO controller interrupts CPU to signal event. Slow response as CPU needs to finish current task. Better CPU utilization.

![[{2AA63296-5E08-4A25-B01A-3B67B2E0F01D}.png]]

Interrupt timeline:
- CPU saves address of current instruction.
- Jumps to interrupt handler.
- Handle interrupt; service IO device, and preserve registers.
- Return to foreground program; special instruction to return from interrupt handler.

![[{1FC70B81-C5AD-498B-B56B-918934296B52}.png]]

How does the CPU find a device’s interrupt handler?
- Common entry: Jump to a predefined routine, check which device caused the interrupt, then branch to its handler.
- Vectored interrupts: Use the interrupt’s vector to select the appropriate handler through a vector table.

What happens if two devices request an interrupt at the same time?
- The interrupt controller selects one according to priority.
- The other request remains pending until it can be handled.
- If priorities are equal, a predefined tie-breaking rule determines the order.
- With a shared interrupt line, software checks the devices’ status and services those requesting attention.
![[{CB58FB66-702B-4473-92F3-5043EA0068C5}.png]]

Summary of interrupt handling:
- I/O device: Raises interrupt.
- CPU: Checks pending interrupts every cycle and performs after priority.
- IO device: Sends its interrupt vector number to the CPU on receiving acknowledgement.
- CPU: Saves state (Pc and registers) and jumps to the interrupt handler.
- Software: Performs IO operation.
- CPU: restores the state and pc and returns to execution.

Exceptions:
- Internal errors during program execution that cause the CPU to leave its normal execution path.
- Caused by invalid conditions, such as division by zero, invalid opcodes, or illegal memory access.
- Handled similarly to interrupts: they are vectored and prioritized, directing execution to the appropriate handler.

Traps:
- Intentionally generated by special instructions, rather than caused by errors.
- Cause the CPU to leave its normal execution path to run a handler.
- Used to request operating-system services.
- In the lecture’s ARM terminology, the SWI instruction generates a trap.

