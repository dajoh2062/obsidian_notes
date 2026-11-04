# 0 - Course introduction


What:
- Understanding hardware.
- Memory, processor, buses and input/output devices
- Software development.
- Assemby, c programming, compilation and optimizations, OS basics.
- Performance and energy efficiency.
- Generative and agentic ai.

Syllabus:
- Pre lecture slides
- Slides
- Quiz
- 4 lab
- Computers as components 5th edition

# 1 - Arm Assembler


Embedded system:
- Textbook definition: Any device that includes a programmable computer but is not itself a general purpose computer.
- Alternate definition: A computing system, specialized for only a few applications with none (or minimum) end user programmability, embedded into a larger product.
- Examples: Cars, Airplanes, appliances, IoT, defence.

![[{A5DEF709-724F-4AE4-A96D-7FA0F8C73DD5}.png|485]]

Embedded system design objectives:
- Predictability.
- Dependability.
- Efficiency; cost, weight and energy.


Embedded system properties:
- Reactive.
- Time constrained.
- Specialized.

Design flow:
- Requirements -> Specification -> Architecture -> Components -> Integration.

Requirements:
- Functional or non functional.

Specification:
- A technical description derived from the Requirements.

Architecture:
- A high level overview of system structure (components) and their interaction.
![[{AFC25D49-A2A8-4B10-8BFF-F4313B92F450}.png]]
Components:
- Choose the components to implement the architectureand to meet specification.
- Standard components: Cpu, memory, software libraries, database, GPS receiver.
- Custom components: Printed circuit boards, softwqare modules, user interface and so on.

Integration:
- Putting it all together to make the system work.
- Often unforseen bugs.

![[{AD6906FB-C0DD-4494-98E8-A078ACCC4F91}.png]]

ISA and Arm assembly:
- ARM: Most popular mobile and embedded ISA. Supports both little and big endian.
- ISA: Instruction set architecture. Defines how a program interact with the hardware. Also defines architecture state, memory management.
- Assembly instructions have a 1 to 1 mapping to machine instructions.
- Load Store architecture: Have to load and store data in seperate instructions from operations on the data. 

Arm variants:
- ARMv8: 64 bit architecture, 32 bit instructions
- ARMv7: 32 bit architecture, 32 bit instructions.
- Thumb: 32 bit architecture, 16 bit instructions size.
- Thumb-2 (used in lab):  32 bit architecture, 16/32 bit instruction size.

Thumb-2 registers
- 16 general purpose (r0 - r15) + 1 CPSR regiser (current program status regiser).
- R15: program counter PC
- R14: Link register LR
- R13: Stack pointer SP

![[{6BF4158A-9837-4CB6-B180-C88B4BAF862A}.png]]


Quiz takeaways:
- Embedded systems always interact with physical world.
- Increasing performance of ES does not really provide any benefit as the its task is not computation (As long as ES is good enough to do its task).
- Safety: systems operate without causing harm. 
- Security: Protecting systems from unauthorized access.
- Memory is accessed and addressed by byte.
- ISA purpose: Bridge between hardware and software, and to develop software before hardware has been produces.

![[{E129DF5C-25C2-4353-930D-8A20C1B2CDCE}.png]]

### C and assembler


Why assembler:
- Deeper understanding of hardware and computer architecture.
- Understanding memory, cpu registers, low level operations and control works.
- Take advantage of architecture specifics not always available in C.
- Security and reverse engineering.
- Appreciating abstraction.
- Legacy systems.

Why C:
- Powerful, efficient and popular (2. place after python).

![[{7D5177BE-96AB-45D3-99DA-47585CD388ED}.png]]

C and Asm example:

```C

external int asmAdd(int a, int b)       // function from .s file

external bool asmRoutine(int a, int b); // function from .s file

int main(){
	int result = asmAdd (2, 3);
}

```

```asm
.global asmAdd
asmAdd:
	ADD R0, R0, R1            @ R1 = b, R0 = a
	BX LR                     @ Returns R0
	
.global asmRoutine
asmRoutine:
	PUSH {R7}               @ Save callee saved R7
	AND R7, R0, R1          @ a & b (OR) into R7
	ADD R7, R7, #2          @ R7 += 2
	MOV R0, R7              @ Move result into return register
	POP {R7}                @ retrieve R7
	BX LR                   @ Returns R0

```



GenAi good and bad:
- Bad: Can use copyrighted code, might give bugs and use fake references, hallucinate. 
- Good: Learning faster, work faster.
- Less focus on memorizing syntax and libraries, and more higher level design, verification and validation.









# 2 - More assembly and input/output devices


Assembly cheatsheet:
```
@ --- Operations ---




@ --- Load and store ---

mov r3, #3     @value 3 into r3. (0 - 255)
mvn r3, #2     @value -3 into r3. (256 - -1)
mvn r4, #255   @value -256 into r4

ldr r3, =vars   @loads value of address of variables vars from .section .data  (pseudo op)

ldr r4, [r3]      @ loads value in first 4 bytes in vars
ldr r5, [r3, #4]  @ loads next 4 bytes in vars
str r6, [r3, #8]  @ saves to next 4 bytes in vars






@ --- Control ---

cmp r1, r2    @ Z if a==b, N if a<b, C if a>=b, V if overflow
              @ followed by branch instruction

beq label  @ r0 == r1
bne label  @ r0 != r1
bgt label  @ r0 > r1
bge label  @ r0 >= r1
blt label  @ r0 < r1
ble label  @ r0 <= r1

bhi label  @ Unsigned r0 > r1
bhs label  @ Unsigned r0 >= r1
blo label  @ Unsigned r0 < r1
bls label  @ Unsigned r0 <= r1

b  label   @ branch uncond and not save
bl label   @ branch and save address pc+4 in lr
bx lr      @ return to address in lr (mov r15, r14)


@ --- Registers ---

@ R0 - R1: Return values
@ R0 - R3: Parameters passed

@ R0 - R3: Caller saved
@ R4 - R11(R12): Calee saved

@ R12: IP (scratch)
@ R13: SP (Stack pointer)
@ R14: LR (Link register)
@ R15: PC (Program Counter)

```


Nested function call example:
```asm
.section .text
.global main

addNum:
    add r0, r0, r1
    mov r15, r14     @bx lr

add:
    push {r14}
    bl addNum
    pop {r15}

main:
    push {r14}
    mov r0, #5
    mov r1, #10
    bl add
    pop {r15}
```

Assembly stack:
- Stack used to save callee saved registers if they are used in function.
- Can alse be used to pass extra parameters and return values.
- Or just to saved nestsed function call return addresses.
- Also used for local variables in function if needed.


CISC:
- Complex Instruction Set Computer
- Register-memory
- x86
- Few registers and less memory use for instructions
- variable length instructions

RISC:
- Reduced Instruction Set Computer
- Load-store
- Arm, Risc-v, Sparc, MIPS
- More register and more memory use for instrucions, faster clock
- Fixed length instructions


![[{DD438C03-C326-4DF0-BE81-6F5F127CF890}.png]]![[{0796079E-CF2F-4A88-98F3-0A000164E6C9}.png]]

I/O controller:
- Connects CPU to device.
- Data reg: contains data read from or writing to device.
- Status reg: Status of read/write operation.


![[{A9C61033-DD73-45F9-A93D-B9DD78F25B24}.png]]
Addressing I/O devices: 
- Memory mapped: I/O controller registers are mapped to a dedicated portion of memory. Regular load/store instructions can be used to access I/O devices. Uses memory space.
- Isolated/port-mapped/I/O - mapped: Seperate memory space, with specialy instructions. Uses in/out in x86.
- A processor can support both modes.

Memory mapped I/O example:
```asm

DEVICE_BASE = 0x1000

REG1 = 0;
REG2 = 4;

LDR r1 =DEVICE_BASE
LDR r0, [r1, #REG1]      @ read from reg1
MOV r0, #8
STR r0, [r1, #REG2]      @ write to reg2


```


How to check I/O status:
- Busy-wait: Continuously check, uses a lot of CPU time and energy but fast response time.
- Polling: Can tune how often to check to balance response time with resource use. 
- Interrupt: Slower response (CPU needs to finish current task), way better utilization of CPU time and energy. I/O controller signals CPU.
- Core independent: better utilization of CPU time and energy, as it doesnt include the CPU at all. Use for simple tasks.


![[{F3C074E4-4CE6-484B-B1F6-7578DC917501}.png]]
![[{28CE147F-A13E-4936-A36A-E4595AF86553}.png]]
Common entry vs. vectored inputs:
- Common entry: Software determines which handler to run, All interrupts initially enter the same code. Every interrupt first sends the CPU to the same predefined address. Code there identifies the cause and chooses the handler.
- Vectored interrupts: Hardware selects the corresponding table entry, Different interrupts can enter different handlers directly.


Interrupt handling:
- I/O device: raises interrupts.
- CPU: Checks pending interrupts every cycle, and achnowledges the highest priority interrupt. Higher priority interrupts will also interrupt the execution of the lower interrupt. A priority register will store the priority of current interrupt. CPU can also mask interupts while doing something important (although som interrupts are non maskable).
- I/O device: sends its interrupt vector to the CPU on receiving acknowledgement.
- CPU: Saves state (pc and other registers), and jumps to interrupt handler like a regular branch procedure.
- Software: performs required I/O operation.
- CPU: Restores state and PC and returns to execution from before.

![[{41083CAA-61AB-47E1-98C5-CCC2CD5FEC4B}.png]]

Exceptions (fault):
- Internal error during program execution.
- Caused by invalid instruction execution, such as division by 0, invalid opcode, illegal memory access.
- Handled same way as interrupts.
- Also vectored and prioritized.

Traps: 
- Explicitly generated by special instructions, not errors.
- Used to request operating system services.


Bus (double meaning):
- 1: collection of wires that run between the components, such as cpu, mem, I/O.
- 2: The protocol that governs the communication between the two devices.
![[{EBF0BC5C-7676-4724-9A75-ECE274047FA1}.png]]

Address bus:
- n wires that specify an address.
- Often unidirectional.

Data bus:
- n wires that carry data.
- Often bidirectional.

Control bus:
- Wires to implement bus protocol, eg. read, write, interrupt request etc.
- Often bidirectional with individual wires being unidirectional.


Bus commonication (4 stage):
- Device 1 sends ENQ / REQ
- Device 2 sends ACK
-  Data transfer ...
- One device lowers its signal
- Other devices terminates

![[{8AC6BA22-6B23-4A61-BDE3-BCAF7D6E19FC}.png|566]]
![[{37584B71-8F48-42A3-B9EC-E39111AC7822}.png]]
1. CPU supplies an address and selects read or write.
2. CPU asserts ENQ: “Please perform this transfer.”
3. Device prepares or accepts the data.
4. Device asserts nACK: “Ready/accepted.”
5. They finish the handshake and return the signals to idle.

![[{ECC3ED13-CE76-455E-A372-63E810A04E93}.png|536]]
![[{B49CB675-E42F-48A6-B4FF-C23BF39CA898}.png|496]]![[{F545FDF6-814E-4631-B3D0-8660EC81AA75}.png|537]]

DMA:
- DMA stands for Direct Memory Access.
- A DMA controller moves data between an I/O device and memory, without the CPU copying each word itself.
- The CPU sets up the transfer: source, destination, amount of data, and transfer settings.
- DMA requests access to the bus and transfers data when granted access.
- Meanwhile, the CPU can do other work, using registers and cache.
- If the CPU needs the same bus while DMA is using it, the CPU may have to wait.
- DMA can transfer a few words at a time and release the bus so the CPU gets a turn.
- When finished, DMA typically interrupts the CPU to report completion.
![[{5028ACB8-B3C9-48AF-8692-49E6345EF736}.png|577]]
![[{02FFB875-AA61-4043-AB38-3D587C72F5E1}.png|583]]
UART:
![[{0D44F2B9-C413-4D09-9CA2-57AB0DC946C7}.png|548]]


# 3 - Processor and memory


Processor architectures:
- Von neumann: Shared instruction and data memory, typically used in general purpose computers.
- Harvard: Seperate memories for instruction and data, mostly in embedded computers.

![[{F163F14E-2F1A-4FFB-852C-9162BAF2E898}.png]]

Main processors functionality:
- Fetch: Get instruction from IMEM.
- Decode: What operation, and what are the operands.
- Execute: Read operamds amd perform required operation. Store result.


Single cycle processor design:
- All functions performed in one cycle. Therefore longer cycles.
- Inefficient hardware use (1/3)

Pipelined processor design:
- Breaks instruction execution into different phases.
- Executes only one phase each instruction per cycle.
- Overlaps execution and therefore higher frequency and better hardware util.


Performance:
- Execution time = Instruction count \* CPI \* Cycle time
- Since CPI and instruction count is same, execution time is at best 3 times faster in pipelined.
- Branch penalty: 2 (number of stages before ex). 

Limiting pipelined performance (hazards):
- Data dependencies.
- Control dependencies.

Avoiding control hazards:
- Branch delay slots: execute n instructions after branch that are always executed. Even more difficult with deeper pipelines. Or use NOPs which is unproductive.
- Branch prediciton (better solution): Predict the direction of branches for fetching next instructions, flush if mistaken.


Performance boosting techniques:
- Out-of-order execution: Reorder instructions to increase IPC.
- Superscalar processors: Fetch, decode and execute multiple instructions per cycle in the same core.
- Multithreaded processors: Execute multiple instructions streams (threads/processes) in parallel.
- Caching, prefetching, vector execution.


Memory requirements:
- Large, fast, high bandwidth and cost efficient.
- Solution: Memory hierarchy.


DRAM:
- Slower access
- 1 capacitator + 1 transistor
- Lower cost
- Requires refresh

SRAM:
- Faster access
- 6 transistors + 0 capacitators
- Higher cost
- No refresh needed

![[{8D03D1EB-A600-4B0C-A84D-794C6E762761}.png]]

![[{FCBE52DC-E966-4CE2-A1B3-6D57B8D5E21C}.png]]

![[{8BC036B5-953C-413A-B031-077EE0B7705A}.png]]
Locality:
- Temporal locality: A recently accessed memory location (instruction or data) is likely to be accessed again in the near future.
- Memory locations (instructions or data) close to a recently accessed location are likely to be accessed in the near future.

Cache terminology:
- Block/line: Unit of data stored in Cache. Typically in the ranges of 16-128 bytes.
- Hit: Data is found in cache.
- Miss: Must move to next level hierarchy. After located, copied to this cache.
- Hit rate: Fraction of accesses that are hits at given level of hierarchy.
- Miss rate: Fraction of accesses that are miss at a given level of hierarchy (1-hit rate).
- Miss penalty: Delay time required to fetch a block into some level from next level of hierarchy.

Accessing cache (direct mapped):
![[{A0D10C36-C311-4458-8B85-B36171E46E12}.png]]
The memory wall:
![[{40ABA1A3-3666-4A28-831D-279D983C727E}.png]]
Testing a function:
- Problem instance: one possible example of input values for all input parameters.
- Test case: problem instance together with the result of applying a correct implementation of the function being tested* on the given problem instance.
- Corcner case: Edge case, test more likely to cause an error.

Primitive solution:
- Testing different cases by manually entering or comment/uncomment test cases.

Automated testing (Stack):
- Read test case, apply function, compare with expected result and output in UART.

![[{15FE08EC-1058-457D-A8B3-E499C6900629}.png]]
![[{706C5034-2F6E-4520-A229-590E9983849C}.png]]
Fundamental computer performance concepts:
- Problem and problem size. 
- Problem instance.
- Algorithm.
- Data structure.
- Language.
- Implementation; libraries, parallelization, vectorization.
- Processors speed, performance, efficiency.
![[{E33C12B8-4401-4B57-A6DB-85CFB1823AEF}.png]]



# 4 - Introduction to C programming




















# 5 - More C programming, Cache and virtual memory



