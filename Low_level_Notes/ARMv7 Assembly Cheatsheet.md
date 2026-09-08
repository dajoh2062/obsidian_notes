
```asm
.global _start        // program entry symbol
.text                 // code section
.data                 // data section
.asciz "Hello"        // null-terminated string

mov  r1, #0           // r1 = 0
ldr  r0, =input       // load address of input
ldrb r4, [r0, r2]     // load one character: input[r2]
str  r1, [r0]         // store 32-bit value
strb r1, [r0]         // store one byte

add  r1, r1, #1       // r1++
sub  r3, r3, #1       // r3--

cmp  r4, r5           // compare values

beq  label            // branch if equal
bne  label            // branch if not equal
blt  label            // branch if less than
bgt  label            // branch if greater than
ble  label            // branch if <=
bge  label            // branch if >=
b    label            // unconditional branch

bl   function         // call function
bx   lr               // return from function

push {r4, lr}         // save registers
pop  {r4, lr}         // restore registers

cmp  r4, #'A'         // compare with character
cmp  r4, #' '         // compare with space
cmp  r4, #0           // compare with null terminator

label:                // jump target / loop point
_exit:
    b .                // infinite loop / stop here
```