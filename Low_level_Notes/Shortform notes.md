
## C and Arm Asm



## Labs

### Lab 1

### Lab 2
### Lab 3

### Lab 4




## Exams

### 2025

1: No, B needs to store the contents of callee saved registers.

2: Push/pop instructions always have their address in Stack Pointer (SP) register and SP is
updated automatically as a consequence of executing push/pop instructions.

3: \[5, 4]

4: False; The negative performance effect of branch penalties can be reduced by adding more stages
to the pipeline

5: Compared to a von-Neumann architecture a Harvard architecture gives better utilization of
the total memory for data and instructions

6: False; Set-associative caches will give faster response to the processor on a cache hit compared to a cache hit in a direct mapped cache.

7: Static memory can lead to fragmentation in the memory that can be removed by calling a garbage collector.


8: Arrays in C are implemented as pointers to the first element of the array.

9: Statement 2: (a + 1) is equivalent to &a\[1]

10: A GP-scheduler follows process priorities more rigorously than a RT-scheduler.

11: Dennard scaling slowed down since Moore’s law did not hold any longer.

12: Vectorization can NOT be combined with bit level parallelism.


13:
1. Timer and timequantum: Timer periodically activates the kernel, and the length of timer period is known as time quantum. The smallest time duration we can control the CPU activity, and shorter than the execution time of any process.
2. Process Priority: Used by the kernel to decide which tasks should be run next time quantum.
3. Kernel purpose: The kernel is a part of the OS and decides from the ready queue which task has the highest priority and is also ready to execute (ie. not waiting for data). (It will also try to organize a fair sharing of resources). It decides which task will run next and causes that task to run for a new time quantum.
4. Process context and context switching: The set of registers that defines a process or task is known as its context, and switching from one process’s register set to another is known as context switching.
5. Scheduling algorithm: The scheduling algorithm is the method the kernel uses to select the next task to run. (A simple scheduling algorithm is round robin).


14:
```c
assert(funcToTest(1,0) == 100);
assert(funcToTest(3,4) == 12);
```


15:
Frontend: Scanning, parsing, Grammar and type checking
Middle: Code analysis, Intermediate representation, Code transformations
Backend: Instruction selection, Register allocation, Code generation


16:
```
int y = 8;
int z;
if(y>7){
	z=3;
} else {
	z = 4 * y;
}
```



17:

```c
uint8_t encode(bool sign, uint8_t exp, uint8_t mant){
	// Format SEEEEMMM
	
	uint8_t result = 0b00000000;
	
	uint8_t signMask = 0b10000000;
	uint8_t mantMask = 0b00000111;
	uint8_t expMask = 0b011111000;
	
	if(sign){
		result = result|signmask;
	}
	
	result = result | (mant & mantMask);
	result = result | ((exp << 3) & expMask);
	
	return result;	
}

```

18:
```c
float decode(uint8_t minifloat){
	uint8_t signMask = 0b10000000;
	uint8_t mantMask = 0b00000111;
	uint8_t expMask = 0b011111000;
	
	uint8_t exp  = (minifloat&expMask)>>3;
	uint8_t mant = (minifloat&mantMask);
	
	bool sign = ((minifloat&signMask)>>7) == 1;
	
	float result = absValue(exp, mant);
	
	if(sign) result = -result;
	
	return result;
	 
	
}
```

19:
```c
bool addToSamples(Samples* ptr, float value){
	
	if((*ptr).avail > 0){
	// can also use NUM_SAMPLES - (*ptr).avail
		int count = (sizeof((*ptr).values)) / 4;
		(*ptr).values[count-(*ptr).avail] = value;
		(*ptr).avail--;
		return true;
	}
	else{
		printf("buffer is full\n");
	}
	return false;
}
```

20:
```c
void calculateCheckSum(Samples* ptr){
	int temps[NUM_SAMPLES];
	long sum = 0;
	
	for(int i = 0; i<NUM_SAMPLES; i++){
		temps[i]=(int)(ptr->values[i]*10);
	}
	for(int i = 0; i<NUM_SAMPLES; i++){
		sum += long(temps[i]);
	}
	
	ptr->simpleCheckSum = sum;
	
}
```


21:
```c
char sendBuffer[
    sizeof(float) * NUM_SAMPLES + sizeof(long)
];

void sendData(Samples* ptr) {
	memcpy(sendBuffer, ptr->values, sizeof(float)*NUM_SAMPLES);
	memcpy(sendBuffer + sizeOf(float) * NUM_SAMPLES,
	ptr->simpleCheckSun, sizeof(long) );
	ptr->avail = NUM_SAMPLES;
}
```


22:

```c

void sendDataCompact(Samples* ptr) {
    memcpy(sendBuffer, &ptr->values[0], sizeof(float));

    int8_t temp[NUM_SAMPLES - 1];

    for (int i = 1; i < NUM_SAMPLES; i++) {
        float diffFloat = ptr->values[i] - ptr->values[i - 1];
        temp[i - 1] = (int8_t)lroundf(diffFloat * 10.0f);
    }

    memcpy(sendBuffer + sizeof(float),
           temp,
           sizeof(temp));

    memcpy(sendBuffer + sizeof(float) + sizeof(temp),
           &ptr->simpleCheckSum,
           sizeof(long));


}
```



















### 2024


1: Not true: We get more space in memory for storing the data of the user program.

2: Not true: Set-associative caches will give faster response to the processor on a cache hit compared to a cache hit in a direct mapped cache.

3: Not true: Busy waiting without a call to the sleep-function as used in polling is simpler code and will save energy.

4: Compared to a von-Neumann architecture a Harvard architecture gives better utilization of the total memory for data and instructions.

5: Not true: The programming of the computer system becomes easier since the CPU does not need to handle memory transfers.

6: Not true: The existence of virtual memory in an embedded system makes it easier to guarantee hard real time requirements.

7: \[0 ,3]

8: Not true: Its makes compilation from C to ARM assembler faster.

9: Not true: An embedded OS typically has a larger memory footprint than an ordinary general purpose OS since the embedded system needs to handle many different IO devices.

10: Not true: Smart buildings and urban development.

11: Supply voltage.

12: Not true: These kind of systems are in general easier to program since they have a richer (more varied) architecture.

13: Not true: Vectorization can NOT be combined with bit level parallelism.

14:
1. The linker reads object binary code files, included static libraries if used, and combine them into one executable file.
2. The assembler reads the assembly code and translates it to binary object code. Assembly have a one-to-one translation to machine isntructions, and goes from readable by humans to non readable.
3. The linker resolves references from different files, and if a reference is not in the same object code it should be found in one of the other input files.
4. The loader reads the executable files, and prepares the memory and the different data areas. 
5. The assembler filles the symbol table with addresses for labels and variables.
6. The compiler transforms from C to assembly code.

15: 
1. The timer is there to keep track of how much time each process is given by the kernel, and time quantum is the amount of time given. The timer periodically activates the kernel, and the period is called time quantum. A process runs until its time quantum is finished, or the task finishes.
2. Process priority decides which process to run next, often decided by deadline, period or some other metric to rank processes on importance, and processes that arent waiting for I/O. 
3. The kernel is a part of the OS and decides from the ready queue which task has the highest priority and is also ready to execute (ie. not waiting for data). (It will also try to organize a fair sharing of resources). It decides which task will run next and causes that task to run for a new time quantum.
4. A process context is its register values, such as the general purpose registers, stack pointer value, program counter value and so on. Context switch is when a process saves those values, then switches place with another process. 
5. Scheduling algorithm is how the kernel decides to split the CPU between processes or threads, and select the next task to run. This can be round robin.


16:
The function counts the length of string in data section. the length is 11.

17: 
ResultA: 0x00000002
ResultB: 0x00000003

18:
Result: 8, 0, 205, 171, 43776

19: 
terminates p:4, s:2, n:4, values\[4] =1
![[{CD6EBBA5-BF82-4597-9DFD-E89F2B645FE2}.png]]

20:
```c
int countNodes(Node* n){
	int count = 0;
	
	if((*n)!=NULL){
		count++;
	}
	while(n->next != NULL){
		count++;
		n = n->next;
	}
	return count;
	
}
```

21:
```c
uint32_t SDLcolor(SDL_Surface *sSurf, uint16_t color){
	uint8_t r, g, b;
	
	uint16_t r_mask = 0x000f;
	uint16_t b_mask = 0x00f0;
	uint16_t g_mask = 0x0f00;
	
	r = (color & r_mask) << 4;
	b =  color & b_mask;
	g = (color & g_mask) >> 4;
	
	
	return SDL_MapRGB(sSurf->format, r, g, b);
}
```

22:
```c
void trianglePlain(Triangle* t, uint32_t* word1, uint32_t* word2){
	uint64_t temp = 0;
	
	// point 1
	temp |= (uint64_t)(t->p[0].x & 0x1FFu) << 55; // bit 63-55
	temp |= (uint64_t)(t->p[0].y & 0xFFU) << 47; // bit 54-47
	
	// point 2
	temp |= (uint64_t)(t->p[1].x & 0x1FFu) << 38; // bit 46-38
	temp |= (uint64_t)(t->p[1].y & 0xFFU) << 30; // bit 37-30
	
	// point 3
	temp |= (uint64_t)(t->p[2].x & 0x1FFu) << 21; // bit 29-21
	temp |= (uint64_t)(t->p[2].y & 0xFFU) << 13; // bit 20-13
	
	// color
	temp|= (uint64_t)(t->color & 0x0fff) << 1; // bit 12-1

	// making sure stop bit is 0
	temp = temp >> 1;
	temp = temp << 1;
	
	*word1 = (uint32_t)(temp >> 32);
	*word2 = (uint32_t)temp;
}
```


23:
```c
uint32_t* plainFormat(Node* n){
	int count = countNodes(n);
	
	uint32_t* buffer = malloc(sizeof(uint32_t)*count*2);
	
	uint32_t word1;
	uint32_t word2;
	
	for(int i = 0; i<count*2; i+=2){
		trianglePlain(n->t, &word1, &word2);
		
		buffer[i] = word1;
	    buffer[i + 1] = word2;
		
		n=n->next;
	}
	buffer[count * 2 - 1] |= 0b1;
	return buffer;
	
	
}
```














### 2023


1: instructions

2: r2: 0x4; r13: 0x404

3: A high-level language code always gets translated to a fixed number of assembly
instructions for a given ISA, irrespective of what microachitecture of the ISA is implemented.

4: Isolated I/O.

5: 20 CPI

6: All of the above

7: High temporal locality

8: Magnetic disc.

9: A virtual page can be placed anywhere in the physical memory

10: (a + 1) is equivalent to &a\[1]

11: 3 

12: Run-time analysis

13: Real-time operating systems usually feature a monolithic structure

14: Task with smaller period has higher priority

15: Task P3 always has a lower priority than P1 and P2 under EDF Scheduling

16: Constant propagation

17: Power gating reduces both static and dynamic power consumption



18:

Direct-mapped cache
0x34 miss
0x74 miss
0x30 miss
0x10 miss
0x37 hit
0x70 miss
0x16 hit
0x34 miss
0x10 hit
0x77 miss


2-way set associative cache
0x34 miss
0x74 miss
0x30 hit 
0x10 miss
0x37 hit
0x70 miss
0x16 miss
0x34 miss
0x10 hit
0x77 miss

19:
8GB vm-


20:

Function inlining is a code optimization that replaces a call to a function with the body of the called function. Function inlining will increase the code size if replicating the function body (i.e., the instructions in the function) at each call site results in more instructions than that are removed by eliminating the function call code (i.e., the instructions for passings function arguments, branch to the function, stack and local variable management, return etc.). This would typically happen if the body of the function is larger than the function call code size and the function is called from many places in the code. Increasing the code size is likely to result in more instruction cache misses. Further, it might require memory or storage constrained devices to add more resources to store the additional instructions. Function inlining will reduce the code size if the function body is smaller than the function call code size. Also, even if the function body is larger than the function call code size, function inlining will reduce the code size if the function is called only from one place in the code. Finally, function inlining might enable better or more optimizations that might reduce the overall code size


### 2022

1: LR

2: 

3: Push/pop instructions always have their address in Stack Pointer (SP) register and SP is
updated automatically as a consequence of executing push/pop instructions.

4: All true.

5: CPU can access registers and caches, but cannot access memory.

6: Both processors are guaranteed to execute same number of instructions while running the same program (Assume that the program is compiled with same compiler with same
optimizations enabled)

7: Tag: 20, Index: 7, Byte Offset: 5

8: Write-through

9: None of the above

10: Multi-level page table requires less memory than one-level page table for applications that do not use full virtual address space.

11: 0

12: arr\[4]

13: Dynamically allocated memory must be explicitly freed by programmer using “free”.

14:  var = var ^ (1 << 7);

15: Simulator

16: IR enables easier optimizations

17: Register allocation attempts to minimize memory accesses

18: Both of the first two statements are FALSE.

19: Each process has its own heap and stack

20: Voltage

21: 























## Lectures


### 1- Arm assembler



Embedded system:
- A device that includes a programmable computer that is not general purpose. A computing system specialized for only a few applications with little end user programmability.
- 3 design objectives: Predictability, Dependability, efficiency (cost weight power).
- System properties: Reactive, Time constrained, specialized.


Design flow:
- Requirements -> Specification -> Architecture -> Components -> Integration.


Arm Assembly: 
- Most popular mobile and embedded ISA.
- 1-to-1 mapping to machine instructions.
- Load store architecture.
- RISC type.
- Variants:
	- ARMv8: 64 bit architecture, 32 bit instructions
	- ARMv7: 32 bit architecture, 32 bit instructions.
	- Thumb: 32 bit architecture, 16 bit instructions size.
	- Thumb-2 (used in lab):  32 bit architecture, 16/32 bit instruction size.


Thumb-2 registers
- 16 general purpose (r0 - r15) + 1 CPSR regiser (current program status regiser).
- R15: program counter PC
- R14: Link register LR
- R13: Stack pointer SP
- R0-R3: Function arguments.

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


### 2 - More assembly and input/output devices

```asm
@ ARM32 cheat sheet

@ --- Move and arithmetic ---
mov  r0, #3         @ r0 = 3
mvn  r0, #2         @ r0 = ~2 = -3
add  r0, r1, r2     @ r1 + r2
sub  r0, r1, #1     @ r1 - 1
mul  r0, r1, r2     @ low 32 bits of r1 * r2


@ --- Bits and shifts --- 
and  r0, r1, r2     @ r1 & r2
orr  r0, r1, r2     @ r1 | r2
eor  r0, r1, r2     @ r1 ^ r2
bic  r0, r1, r2     @ r1 & ~r2

lsl  r0, r1, #3     @ shift left, zero-fill
lsr  r0, r1, #3     @ shift right, zero-fill
asr  r0, r1, #3     @ shift right, sign-fill
ror  r0, r1, #3     @ rotate right

@ --- Memory ---
ldr  r1, =vars      @ load address (pseudo-instruction)
ldr  r0, [r1, #4]   @ load 32 bits from address r1 + 4
str  r0, [r1, #4]   @ store 32 bits at address r1 + 4
ldr  r0, [r1], #4   @ load 32 bits, then advance r1 by 4
@ ldrb / ldrh   load 8 / 16 bits, zero-extend
@ ldrsb / ldrsh load 8 / 16 bits, sign-extend
@ strb / strh   store low 8 / 16 bits

@ --- Compare and branch ---
cmp  r1, r2         @ set flags from r1 - r2
beq  label          @ branch if r1 == r2
@ Equality  beq ==   bne !=
@ Signed    bgt >    bge >=   blt <    ble <=
@ Unsigned  bhi >    bhs >=   blo <    bls <=
b    label          @ unconditional branch

@ --- Calls and stack ---
bl   func           @ call, save return address in lr
blx  r3             @ call via register
bx   lr             @ return
push {r4, lr}       @ save r4 and return address
pop  {r4, pc}       @ restore r4 and return

@ --- Calling convention ---
@ r0-r3           argument registers, caller-saved
@ r0 / r0-r1      32-bit / 64-bit integer return
@ r4-r8, r10-r11  callee-saved
@ r9              platform-dependent
@ r12             caller-saved scratch (ip)
@ r13 = sp        r14 = lr        r15 = pc
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

![[{0796079E-CF2F-4A88-98F3-0A000164E6C9}.png]]

I/O controller:
- Connects CPU to device.
- Data reg: contains data read from or writing to device.
- Status reg: Status of read/write operation.

![[{A9C61033-DD73-45F9-A93D-B9DD78F25B24}.png]]
Addressing I/O devices: 
- Memory mapped: I/O controller registers are mapped to a dedicated portion of memory. Regular load/store instructions can be used to access I/O devices. Uses memory space.
- Isolated/port-mapped/I/O - mapped: Seperate memory space, with specialy instructions. Uses in/out in x86.
- A processor can support both modes.


How to check I/O status:
- Busy-wait: Continuously check, uses a lot of CPU time and energy but fast response time.
- Polling: Can tune how often to check to balance response time with resource use. 
- Interrupt: Slower response (CPU needs to finish current task), way better utilization of CPU time and energy. I/O controller signals CPU.
- Core independent: better utilization of CPU time and energy, as it doesnt include the CPU at all. Use for simple tasks.

![[{F3C074E4-4CE6-484B-B1F6-7578DC917501}.png]]![[{28CE147F-A13E-4936-A36A-E4595AF86553}.png]]

Common entry vs. vectored inputs:
- Common entry: Software determines which handler to run, All interrupts initially enter the same code. Every interrupt first sends the CPU to the same predefined address. Code there identifies the cause and chooses the handler.
- Vectored interrupts: The interrupt supplies a vector number. The CPU uses it to look up the handler’s address and jumps straight to that handler.

Interrupt handling:
- I/O device: raises interrupts.
- CPU: Checks pending interrupts every cycle, and achnowledges the highest priority interrupt. Higher priority interrupts will also interrupt the execution of the lower interrupt. A priority register will store the priority of current interrupt. CPU can also mask interupts while doing something important (although som interrupts are non maskable).
- I/O device: sends its interrupt vector to the CPU on receiving acknowledgement.
- CPU: Saves state (pc and other registers), and jumps to interrupt handler like a regular branch procedure.
- Software: performs required I/O operation.
- CPU: Restores state and PC and returns to execution from before.

Exceptions (fault):
- Internal error during program execution.
- Caused by invalid instruction execution, such as division by 0, invalid opcode, illegal memory access.
- Handled same way as interrupts.
- Also vectored and prioritized.

Traps: 
- Explicitly generated by special instructions, not errors.
- Used to request operating system services.


![[{EBF0BC5C-7676-4724-9A75-ECE274047FA1}.png]]
Bus (double meaning):
- 1: collection of wires that run between the components, such as cpu, mem, I/O.
- 2: The protocol that governs the communication between the two devices.

Address bus:
- n wires that specify an address.
- Often unidirectional.

Data bus:
- n wires that carry data.
- Often bidirectional.

Control bus:
- Wires to implement bus protocol, eg. read, write, interrupt request etc.
- Often bidirectional with individual wires being unidirectional.

DMA:
- DMA stands for Direct Memory Access.
- A DMA controller moves data between an I/O device and memory, without the CPU copying each word itself.
- The CPU sets up the transfer: source, destination, amount of data, and transfer settings.
- DMA requests access to the bus and transfers data when granted access.
- Meanwhile, the CPU can do other work, using registers and cache.
- If the CPU needs the same bus while DMA is using it, the CPU may have to wait.
- DMA can transfer a few words at a time and release the bus so the CPU gets a turn.
- When finished, DMA typically interrupts the CPU to report completion.



### 3 - Processor and memory


Processor architectures:
- Von neumann: Shared instruction and data memory, typically used in general purpose computers.
- Harvard: Seperate memories for instruction and data, mostly in embedded computers.
![[{F163F14E-2F1A-4FFB-852C-9162BAF2E898}.png]]


Single cycle processor design:
- All functions performed in one cycle. Therefore longer cycles.
- Inefficient hardware use (1/3).

Pipelined processor design:
- Breaks instruction execution into different phases.
- Executes only one phase each instruction per cycle.
- Overlaps execution and therefore higher frequency and better hardware util.

Performance:
- Execution time = Instruction count \* CPI \* Cycle time

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
- 1 capacitator + 1 transistor
- Requires refresh

SRAM:
- 6 transistors + 0 capacitators
- No refresh needed

![[{8BC036B5-953C-413A-B031-077EE0B7705A}.png]]


Locality:
- Temporal locality: A recently accessed memory location (instruction or data) is likely to be accessed again in the near future.
- Memory locations (instructions or data) close to a recently accessed location are likely to be accessed in the near future.


Cache terminology (Per level L1-L3):
- Block/line: Unit of data stored in Cache. Typically in the ranges of 16-128 bytes.
- Hit: Data is found in cache.
- Miss: Must move to next level hierarchy. After located, copied to this cache.
- Hit rate: Fraction of accesses that are hits at given level of hierarchy.
- Miss rate: Fraction of accesses that are miss at a given level of hierarchy (1-hit rate).
- Miss penalty: Delay time required to fetch a block into some level from next level of hierarchy.


The memory wall: 
- is the gap between how fast the CPU can process data and how fast main memory (RAM) can supply it.
- Size and throughput(bandwidth) increases while latency is the bottleneck.

Testing a function:
- Problem instance: one possible example of input values for all input parameters.
- Test case: problem instance together with the result of applying a correct implementation of the function being tested* on the given problem instance.
- Corner case: Edge case, test more likely to cause an error.



### 4 - Introduction to C programming


C facts:
- Both highlevel and lowlevel language.
- Fast and unsafe: Run-time errors not caught before runtime. 
- Procedural / imperative: Operations and data for operations.

 C datatypes:
- Char: 1 byte.
- Short: 2 bytes.
- Int: 2, 4 or 8 byte (depends on machine word size, usually 4 byte).
- Long: 4 or 8 byte.
- float: 4 byte.
- Double: 8 byte.
- Boolean (not primitive): 1 byte.
- Pointers: 4 or 8 bytes.
- Usually also offer signed and unsigned.

Memory areas:
- Heap: Dynamically allocated memory. Managed by programmer.
- Stack: Function/method local variables. Managed by compiler.
- Static: For data living the program lifetime, such as static variables or variables made outside a function.

```c
int main(){

// Other
define MAX(X,Y) (X>Y ? X : Y) // careful with macros!

// Types
const int x = 4   // will be replaced in preprocessor
volatile int y = 4  // not optimize anything


// Pointers
int value = 32; 
int *p = &value;   // save address of value in p
*p = 64            // value now contains 64


// pointers to array
int *p = arr + n;  // point to n element in array
int *p = &arr[n];  // same
int value = *(arr + n);
*p++ // use value pointet to by p then increment
*++p // increment then point
*(p)++ increment value pointed to by p


// Arrays
int m[] = {5, 8, 10};  // 3 integers
int n[2][10];         // 2 rows, 10 columns
point p[4];          // array of 4 points


// Structs
struct Point {
    int x, y;
};

struct Point p = {2, 3};
p.x = 5;
printf("(%d, %d)\n", p.x, p.y); // (5, 3)

// struct with typedef
typedef struct {
    int x, y;
} Point;

Point p = {2, 3};
p.x = 5;
printf("(%d, %d)\n", p.x, p.y); // (5, 3)
}


// generic swap function
void swap (void *a, void *b, int size){
	char buffer[size];
	memcpy(buffer, a, size);
	memcpy(a , b, size);
	memcpy(b, buffer, size);
}

int x = 10;
int y = 20;
swap(&x, &y, sizeof(int));


// Dynamically allocated memory
int *p;
if((p = malloc(n*sizeof(int))) == NULL){
	return 1;	
}
free(p);

```




Twos compliment:
- Flip all bits.
- Add 1.
- Sign extend if converting from short to int.


Endianess:
- Big endianness (regular order): Most significant byte first.
- Little endianness (most common): Least significant byte first.

Direct-mapped cache:
- tag | index | offset
- The index selects one cache location, which holds one line.
- Compare the address’s tag with that line’s stored tag to check for a hit.
- The byte offset selects the required byte within the line.
- Problem: Several addresses might repeatedly compete for a few/ the same cache location.


N-way set-associative:
- tag | index | offset
- The cache is divided into sets, each holding N lines, called ways.
- The index selects one set.
- Compare the tag against all N tags in that set. A match in any way is a hit.
- The byte offset selects the required byte within the matching line.
- On a miss, if the set is full, a replacement policy chooses which of its N lines to replace.

Fully associative:
- tag | offset
- The entire cache is one set, so there are no index bits.
- A memory block can go into any cache line.
- Compare the tag against every line’s tag to find a match.
- The byte offset selects the required byte within the matching line.
- Problems: Slower to search for tag.


### 5 - More C programming, Cache and virtual memory

Cache replacement:
- Random.
- LRU (least recently used): Evict cache block that hasnt been accessed longest. Complex implementation.
- FIFO (first in first out): Same order as fill order.

Writing modified data:
- Write-back: When block is evicted, need to keep track of modified blocks ("D" - dirty 1/0). Saves resources.
- Write-through: When writing to cache we also write to memory. Simpler implementation and more coherence, but slower and resource intensive.

Cache performance:
- AMAT = (hit rate \* hit-time + missrate \* miss-latency)
	- Average memory access time.
	- For each cacle level.
	- In cycles.
![[{B074A89F-C379-4805-88BD-EA168DBD548A}.png|579]]


#### Virtual memory

Motivation:
- Capacity: Allowing physical memory to be smaller than the 32 bit address space. Also allow multiple programs to share limited physical memory.
- Safety: Prevent user programs to access memory used by OS. And to control access by one user program to the memory of other user programs.

Virtual memory:
- Make each program think it owns the entire memory.
- Virtual addresses: Addresses visible to the programmer.
- Physical addresses: Actual memory addresses, used to access cache/memory. 
- Address translation: Virtual address are translated on the fly to physical addresses (jointly by hardware and OS).
- Parts not recently used stored on disc.

Address translation:
- Paging: Using pages (fixed size) as translation units.
- Segmentation: Translation units are variable size memory regions.

Paging:
- "Page"/"virtual page" for virtual memory, and "page frame"/"physical page" for physical.
- 4-8 kb (4096-8192 bytes). Can be MB or GB in servers.
- Translation: Per program page tables. 
- Different programs can use the same virtual address.

![[{7E7C6161-AB71-4034-A03B-E037DC32CA6C}.png|590]]![[{46DEBEE6-C298-4279-8D9E-6A5CBD3313ED}.png]]![[{0E0CF499-571E-443E-B192-FE2ED426F734}.png]]
Translation Lookaside Buffer (TLB):
- TLB is a cache (typically fully assosiative) for page table.
- Close to CPU and fast.
- TLB miss: Access page table and save translation in TLB.


# 6 - Small microcontrollers and Energy efficiency

Microchip:
- Microchip Technology is an American company that designs and manufactures semiconductors.
-  Its products include microcontrollers (small computers that control functions inside devices), chips for power management and connectivity, memory, and security.


Basic electronics:
- Voltage  = Resistance \* current.
- Power = Voltage \* current
- Power Consumption \[W]  = \[J] / \[S]
- Watt Hours = \[W] \* \[Hr]
- Ampere hours = \[A]\*\[Hr]


![[{0307895D-010F-4604-8E4B-94755187F617}.png|700]]

#### Microcontroller architecture

![[{EFD2CBAD-5AB2-4EF5-8AA3-379BFFED9A3D}.png|621]]AVR / PIC differences:
- CPU design: AVR and PIC use different basic instructions and ways of doing calculations.
- Registers—tiny storage spots inside the CPU: AVR usually has 32 working registers; common 8-bit PICs do many calculations through one main working register, called W.
- Programming: The program must be built for the chosen chip. An AVR program cannot simply be loaded onto a PIC.
- Features: Memory, speed, pins, and built-in tools depend on the specific model, not just whether it’s AVR or PIC.

Microcontroller typical applications:
- Low power applications: Battery powered devices, IoT applications.
- Distributed logic and coexistence with larger processors: real-time control tasks, reading sensors, controlling simple user interfaces.



Battery capacity:
- Battery time \[Hrs]  = Battery capacity \[mAh] / Current consumption \[A]

#### Low power techniques

1 - Operating modes (AVR example):
- Active mode: All clocks active, CPU is running, Power consumption is proportional to the frequency of the system clock.
- Idle mode: CPU stops executing code, no peripherals are disabled, all interrupt sources can wake the device.
- Standby mode: Peripherals can be configured to be enabled or not, power consumption depends on what functionalities are enabled.
- Power-down: Only few peripherals are active, fewer wake up sources.
- Total power consumed = Active mode power + standby/sleep mode power +wake up power.

![[{488038AF-1DD1-4838-A58F-CF8699BBEFA4}.png|479]]
![[{BB2B5FEA-D1C8-48A8-8773-C83C619537F1}.png]]
2 - Busy-wait vs. Polling:
- Busy- wait: Checking the condition continously, keeping the CPU fully enganged.
- Pro: Low latency. 
- Con: High power consumption.
- Use case: Short, immediate waits.
![[{C8927A9F-C870-4D2A-ADBE-13D6A10A869D}.png|392]]
- Polling: Periodically checks a condition at certain intervals, allowing CPU to multitask or enter low power mode.
- Pro: Moderate power consumption.
- Con: Higher latency.
- Use cases: Less critical, periodic checks.
![[{C84490B7-592A-4510-8F3C-EE2219002F91}.png|557]]
- Interrupt-driven approach: Uses hardware interupt to notify cpu of an event, freeing the CPU to do other tasks or low power mode.
- Pros: Very low latency, low power consumption.
- Use cases: Real-time and power sensitive.
![[{0D29C0ED-A343-4395-9185-4A6B8FCA786A}.png|608]]
3 - Core independent Peripherals (CIPs):
- Peripherals capable of handling tasks without intervention from CPU. 
- Frees CPU time. 
- Provide short and predictable response times between peripherals. Reduces complexity and execution time
- Examples: Event system, Configurable custom logic, timers, counters, Analog to digital coverter.

4 - Event system:
- Peripheral to peripheral signaling.
- Enabled through event channels, not using the CPU.
- Reduces complexity, size and execution time of the software.
![[{1E11D7ED-C8D4-4AE9-86D5-EBC24D1C8028}.png]]

5 - Choice of Oscillator:
- Choice of oscillator and clock speed impacts power consumption.
- Lower frequency = lower power. usually.
- Tradeoff between accuracy and power consumption.
- Sometimes a higher frequency is advantageous, returning to lower power modes quicker.

6 - Unused Pins:
- Unused pins are chip connections you aren’t using.
- If left floating, their voltage is not held at a fixed level.
- Electrical noise can make them read randomly as 0 or 1, wasting power.
- An internal pull-up resistor holds the pin at a stable high voltage.
- Disabling the input buffer turns off the circuit that reads the pin, saving more power.
- The code applies both settings to all eight pin positions in a port—use this only if those pins are unused.
![[{33106493-A900-4391-AFEE-3910906F591B}.png]]

#### MISC

Bitmasks and bitwise operations:
- AND: &=
- OR: |=
- XOR: ^=
- NOT: ~(bits)


Analog comparator (AC):
- Output 1 when difference in volage is positive and 0 otherwise. 

![[{DCD865F8-58F5-4C71-9D1D-7CA98CE01817}.png|503]]

AVR Architecture:

![[{6E85CB18-E3F9-4FF9-836E-0212485D2788}.png]]

PIC Architecture:
![[{EF5B55B4-8530-46F5-824D-B5D4AAE83DF7}.png]]



### 7 - Energy efficient computing



Power vs energy:
- Power = Energy/time
- \[Watt] = \[Joule]/\[Second]
- Servers and desktop computers have the thermal envelope as their limit as overheating is the main concern (power), while phones and portable devices have the battery size as the limiting factor (total energy).

Dynamic power:
- Power consumed when a circuit switches between 0 and 1, usually when doing a useful activity.![[{F7AA6500-C880-4F80-9E48-F5839C722B23}.png|295]]
Static power:
- Power consumed by circuit while doing nothing. Transistors leak even in offs state.![[{BF140430-26B2-4E51-87E1-71E75BF948B6}.png|352]]
Reducing energy consumption:
- The smaller the transistors, leakage powers portion increases.
- Hardware techniques to decrease: Clock gating, power gating.
- Software techniques: Sleep modes, dynamic voltage and frequency scaling (DVFS).


Clock gating:
- Reduces dynamic power.
- Turning of clock signal to circuit that is currently unused.
- Keeps store value as long as EN = 0, which sets clock at 0. Makes frequency (f) 0.
![[{DC72D1F9-D119-4EEB-8324-A0E74F480F42}.png]]
Power gating:
- Reduces dynamic and static energy.
- Turns off power to the circuits not in use.
- An ordinary register loses its stored value when its supply is disconnected. After power returns, its value must be initialized or restored.
- Leakage is reduced way more than clock gating.
- Energy Overhead: require some energy to power down and power up a unit, so the energy saved should out-weigh.
- Performance overhead: Extra time required in relation to activating the unit

OS power managment:
- Hardware provides multiple power modes, which Os can use to reduce consumption.
- Sleep modes provide maximum power saving. Similar to power gating but with software. Overhead on entering and exiting. Multiple levels to adapt the tradeoff.

Dynamic voltage and frequency scaling (DVFS):
- Lets OS choose between multiple voltages and clock frequencies.
- Reducing frequency reduces dynamic power consumption, but energy remains the same.
- Reducing voltage reduces both dynamic and static power consumption.
- OS needs to adjust voltage and frequecy such that the CPU runs just fast enough for task.


#### Bare metal, runtime systems, and operating systems


Bare metal programming:
- Software runs directly on hardware. No system calls, libraries or drivers.
- Minimal portability, no hardware abstraction and harder debugging.
- Max performance and control. Predictable timing, ideal for Realtime systems.

Runtime environment (RTE) and runtime system (RTS):
- Memory management (stack, heap/allocated memory, garbage collection).
- Aspects of programming languages that needs to be handled during run time, such as communcation between processes/threads, signals, dynamic loading and more.
- Distinction between runtime system and operating system is not clear.

Operating system (Os) and Real time Operating system (RTOS):
- File system, process management, security, networking and more.

Performance monitoring unit (PMU):
- Counts hardware events that help explain execution time.
- Such as clock cycles, instructions completed, cache references, cache miss, branch mispredicts.
- Derived metrics: , instructions per completed clock cycle, cache miss rate, branch misses per instruction executed.
- Why use:  Big-O describes growth with input size; it does not capture instruction counts, memory accesses, or pipeline behavior. The counters show differences that execution time alone cannot explain.


#### Sustainability

Sustainalbe computing:
- Reducing the environmental impact of computing itself.

Computing for sustainability:
- Using computers to improve other systems, such as transport or energy supply.

Operational footprint:
- Impact from using equipment, including electricity and cooling.

Embodied footprint:
- Impact associated with manufacturing and other lifecycle stages beyond operation.
- A computer’s environmental cost includes raw materials, production, transport, use, maintenance, and disposal.
- Examples of reducing embodied impact include extending hardware lifetime, producing fewer chips, and designing smaller chips.

Jevons paradox: 
- Improving efficiency can make a resource cheaper to use, increasing demand enough that total resource consumption rises. Better efficiency per computation does not guarantee lower overall consumption.

#### Technology and performance trends

Moore’s law (slowing/stopped?):
 - Historical trend of increasing transistor counts per chip.
 - Doubled roughly every two years.

Dennard scaling (has stopped):
- Historical ability to shrink transistors while keeping power density roughly constant.
- Under ideal Dennard scaling, halving transistor dimensions quarters their area and power. More transistors could therefore fit into the same area without increasing power density.
- That scaling broke down because voltage and current could no longer decrease sufficiently while maintaining reliable operation. Leakage also became a larger problem.

Power wall:
- Power and cooling limits restrict further performance increases.


Dark silicon (not sustainable):
- Parts of a chip cannot operate simultaneously within its power or thermal budget.



Parallelisation, vectorization and specialization:
- Requires more complex programming but can improve energy efficiency.

#### Power, energy, and gating


Dynamic power:
- P_dynamic =  a \* f \* C \*  V\^2
- Where a is activity factor and C is how much capacity we have.
- Energy used caused by transistor switching.

Clock gating:
- Turning off clock to circuits not in use, reducing P_Dynamic by reducing activity factor temporarly.

Static power:
- P_static = I_leakage \* V
- \[W] = \[A]\*\[V] 
- Increases with temperature, and with smaller transistors.
- Leakage current, even when not used.

Power Gating:
- Turning of power supply to circuits not in use, mainly reducing P_static by decreasing voltage. Also reduced dynamic.
- More overhead than clock gating.



#### Thermal limits and measurement

Dennard scaling slow down and end (estimated 2005-2010):
- Transistor size decreased while power denisty was stable, allowing more transistors.
- Stopped due to not possible to drop voltage and current levels in proportion.
- Heating and static power leakage become a larger problem at smaller transistor sizes.
- Makes parallelism and multi core more important, in processors and memory, due to single core performance tapering.

Dark silicon problem:
- Because of power and thermal limitations, parts of a chip must remain powered off.
- This makes processors with smaller sized transistors, and same total size and power not gain the proportional performance.
- Also for parallel computing: Even if manufacturers can fit more CPU cores onto a chip, the power budget may prevent all of them from running simultaneously.

Thermal constraints:
- When a processor consumes electrical power, most of that energy eventually becomes heat.
- If the processor generates too much heat, it can overheat.
- Usually the constraint for human touched devices.

Power constraints:
- A power budget is the maximum electrical power a device or system is allowed to consume.
- A data center has limitations on how much electricity its infrastructure can deliver.

Difference electrical enery and Power:
- Electrical energy: Amount of energy transferred to other forms (heat) in an electrical circuit during some amount of time. Measured in Joules ( J = W \* S)
- Power: Work done” (electrical energy transferred) per unit time. Measured in Watt (W = J / s). Also: P (watt) = U (Volt) \* I (Ampere).


Thermal design power (TDP):
- Represents the average power the processor dissipates when operating at base frequency with all cores active under a high-complexity workload.

Hyperthreading / SMT:
- Allows one physical core to appear as two logical cores, to the OS, and therefore handle double the amount of threads. This makes it possible to better multiplexing multiple lightweight threads or processes.

Turbo boost:
- Dynamically increase clock to enhance performance when needed (within limits of TDP).
- Dynamic overclocking.

Relevance to low-level programming:
- Reading temperature, fan speeds, controlling fan speeds.
- Controlled temperature is necessary for precise energy measurements.
- Temperature affects on-chip energy measurements.
- Made possible by Intel RAPL (Running Average Power Limit)


#### The vertical approach and efficiency metrics

Vertical Approach to Energy Efficiency:
- Main idea: Improve energy efficiency across all levels of a computer system, from software to hardware.
- Applications: Optimize algorithms, data structures, programming languages and parallelization.
- System software: Optimize operating systems, compilers, task scheduling, resource management and DVFS.
- Computer architecture: Improve multicore systems, accelerators, caches, memory and communication.
- Holistic approach: Consider how hardware and software interact rather than optimizing each layer independently.
- End of Dennard scaling: Forced architects to exploit parallelism for better energy efficiency.

Energy Efficiency Metrics:
- Energy: \(E=P\* t / \[J] = \[W]\*\[S]), measured in Joules (J).
- Optimization goals: Maximize performance, minimize energy consumption, or balance both.
- Performance per Watt: FLOPS/Watt, used by Green500 to rank energy-efficient supercomputers.


#### Parallelism and specialization

Vectorization (SIMD):
- Vectorization: Performs the same operation on multiple data elements simultaneously (SIMD = Single Instruction, Multiple Data).

 Chip Multiprocessors (CMP):
- CMP: Multiple processor cores on the same chip, potentially sharing cache.
- Throughput: Multiple slower cores can achieve similar throughput to one fast core for parallel workloads.
- Energy efficiency: Lower frequency allows lower voltage, significantly reducing dynamic power.
- Example: Under ideal assumptions, 2 cores at half frequency and voltage consume only 25% of the original dynamic power.
- Limitation: Parallelization introduces overhead, and not all applications can be parallelized efficiently.

 MPSoC:
- Real-time systems: Dedicated cores make timing guarantees easier.
- SoC (System on Chip): Integrates multiple components onto one chip, reducing cost.
- MPSoC: SoC containing multiple processors; physically separated processors form a distributed system.

Accelerators and Parallelism:
- Accelerators: Specialized hardware for specific workloads, such as GPUs, DSPs and NPUs.
- BLP: Bit-Level Parallelism.
- ILP: Instruction-Level Parallelism.
- TLP: Thread-Level Parallelism.
- DLP: Data-Level Parallelism (e.g., SIMD).
- ALP: Accelerator-Level Parallelism, multiple accelerators execute different workload components concurrently.


### 8 - Tools for embedded systems programming and examples


Compiler warning (gcc):
- Catches messages from checks the compiler does when building.
- The warning flags can be combined.
- `-Wall`: only enables some warnings.
- `-Wextra`: More warnings.
- `-Wpedantic`: Catches use of extensions that does not conform to the standard.
- `-Werror`: Turns warnings into errors, causing compilation to fail.


#### Static analyzers (compiler, at gcc)

`-fanalyzer`

What:
- Goes furthur than compiler warnings, and checks different branches.
- Follows possible paths through the code and looks for problems;
	- Null pointer dereference. 
	- Uninitialized variable use.
	- Memory leaks.
	- Use after free.
	- Double free.
	- Out-of-bounds access.
	- Resource leaks.
	- Arithmetic problems like division by 0.
- Cons: Can miss real bugs or report false positives.




#### Runtime sanitizers (compiler, at ./a.out)

ASan - Address Sanitizer:
- `-fsanitize=address`
- Catches:
	- Out of bounds read/write
	- Use after free
	- Double free or invalid free
	- Use after return

UBSan - Undefined Behavior Sanitizer:
- `-fsanitize = undefined`
- C lets you write programs with undefined behavior.
- That can produce inconsistent results or errors.
- Can catch:
	- Signed integer overflow.
	- Integer divison by 0.
	- Invalid bit shifts.
	- Null pointer dereferences.
	- Misalligned memory access.
	- Som array out-of-bounds access.

LSan - Leak sanitizer:
- `-fsanitize = leak"`
- Reports leaked allocations, sizes and where they are allocated.
- Detects memory allocations that werent freed and no longer reachable.



#### Runtime analyzers (at ./a.out)

Valgrind:
- Runs a program inside its own specialized environment where it can monitor, track, and check almost everything.
- Memcheck: invalid memory accesses, memory leaks, uninitialized memory ...
- Callgrind: records function calls and instruction costs.
- Find computational hotspots.
- Cachegrind: Estimates I$ and D$ behavior.

Linux Perf:
- Performance analysis tool 
- Collects performance data samples
- Other programs can visualize those samples.



#### Fuzzing (not really the compiler)

- A kind of automated testing
- Automatically feed a program millions of "random" inputs to discover bugs
- I say "random" because fuzzers can be quite intelligent in how they generate input and explore the space of possbile inputs.
- Combine fuzzing with the aforementioned sanitizers for a powerful bug finding combo
#### Debugger

You can:
- Set a breakpoint: pause when execution reaches a particular line.
- Step through code: execute one line at a time.
- Inspect variables and memory.
- Inspect the call stack: see which functions called the current function.
- Investigate a crash: see where execution failed and inspect the available state.


### 9 - Embedded Operating Systems


Operating systems:
- Dekstop: Windows, linux, macos.
- Mobile: android, ios.
- Embedded os: Blackberry QNX, GreenHills software, arm mbed os.


Why are desktop os not suitable for embedded devices:
- Usually monolithic or hybrid, providing numerous services for diverse hardware.
- Too large memory usage and disc space requirements.
- Dont provide special features needed: Real time guarantee, reliability by certain standards.

DO-178C:
- Software Considerations in Airborne Systems and Equipment Certification.
- Applications divided into 5 levels:
	- Catastrophic: Failure may cause deaths, and loss of airplane.
	- Hazardous: Failure has a large negative impact on safety, performance, or reduces the crews ability to operate the arcraft safely, and may cause fatal injuries among passengers.
	- Major: Failure significantly reduces safety margin or increases work load.
	- Minor: Failure slightly reduces safety margin, or increases work load.
	- No effect: Failure has no impact on safety, aircraft operation, or crew workload.


Embedded os:
- Diverse and manage various types of software.
- Many are designed with only one or a few specific functionalities.
- Usually resource constrained.

Embedded Operating systems Goals:
- Customizable: Functionalities.
- Resource efficient: small memory footprint.
- Predictable and deterministic: real-time capabilities.
- Energy efficient: dynamic power management.

Processes and threads:
- Two objects to be scheduled by OS.
- Different states during execution:
	- READY: Ready to run and pending.
	- RUNNING: Being executed by OS.
	- BLOCKED: Suspended due to an event, such as I/O operation.
- Scheduling: Policies that OS uses to determine execution order of ready objects.
- Scheduling algorithms have diverse objectives and demonstrate different effects on performance. Includes round-robin, completely fair scheduler, multi-level feedback queue and more.
![[Screenshot 2026-10-27 at 14.54.23.png]]

Completely Fair Scheduling (CFS):
- Default CPU scheduler in Linux.
- Choose the process with lowest virtual execution time: vruntime.
- Balanced binary search.
- Complexity: insertion, deletion and update is O(logN) and find minimum is O(1).
- Goal is fairness, and cannot guarantee real-time deadlines.

Real time guarantee/requirements:
- Many embedded systems have requirements for not only correctness of computation, but also time at which the results are produced.
- Predictability and determinism.
- Classification of consequence of missing deadline:
	- Hard Real-time: Catastrophic consequences, safety critical applications.
	- Soft Real-time: desirable features such as information systems or video.

RT extension for OSs:
- For linux: Used in som safety critical systems such as NASA. 
- Some time-sensitive high-performance computing applications emerged.
- PREEMPT_RT for linux: Makes more of the kernel preemptible, allowing high-priority tasks to run sooner.
- SCHED_DEADLINE: Three parameters; runtime, period and deadline. Earliest deadline first principles.
- Still cannot achieve the same level of real-time performance as real-time operating systems.

#### Real-time Operating Systems (RTOS)

What:
- OS that supports real-time scheduling and guarantee real-time deadlines.
- Typically microkernel based.
- Examples: Blackberry QNX, Zephyr, arm mbed os, Greenhills, freeRTOS, threadX.

![[Screenshot 2026-10-27 at 15.37.51.png]]

Real-time scheduling:
- Algorithms used to make tasks meet deadlines.
- Know more about the tasks.
- Such as: Deadlines, periods, worst-case execution time (WCET).
- Many embedded systems are closed systems.
- Static scheduling algorithms: Scheduling based on fixed parameters before execution. FPS, RMS.
- Dynamic scheduling algorithms: Scheduling based on dynamic parameters that may change at runtime. EDF, Least slack time.

Fixed priority scheduling (FPS): 
- Each task is assigned a priority number that doesnt change during execution, and tells CPU which task to run with the highest priority.


Rate-monotonic Scheduling (RMS): 
- Specific type of FPS. The task with the shortest period gets highest priority. Priority is frequency of task. 
- Simple to implement, widely used.
- Assume deadline = Period.
- No resource sharing and no context switching overhead.
- CONS: Lower system utilization.

![[Screenshot 2026-10-27 at 16.10.47.png|547]]
Earliest Deadline First (EDF):
- Closest absolute deadline gets the highest priority.
- Dynamic priority.
- Preemptive.
- Cons: Less common, complicated to implement.
- Assumptions: Deadline = period, uniprocessors, no context switching overhead, no resource sharing.
- Schedulability test: 
![[Screenshot 2026-10-27 at 16.32.17.png|476]]
![[Screenshot 2026-10-27 at 15.59.43.png]]
Priority inversion:
- Accessing critical sections should be atomic.
- Guarantee mutual exclusion using mutex lock or semaphore.
- Priority inversion is when a high priority task is indirectly blokced by a low priority task, due to the lower priority task has obtained a variable lock the higer priority needs.

Priority inheritance:
- A solution to priority inversion problem.
- when a high-priority task has to wait for some resource shared with a running low-priority task, the priority of the low-priority task is temporarily promoted to the highest priority level of the pending tasks.


Energy efficient scheduling:
- A lot of embedded systems are battery powered, and energy consumption is one of the main design concerns.
- Linux kernel; energy aware scheduling.
- Energy consumption: E = P \* T
- Power consumpton: P = P_dynamic + P_static
- Dynamic voltage and frequency scaling (DVFS): Ised to slow down processor by scaling down frequency and voltage. Use voltage and frequency just enouch for tasks to meet deadlines. ![[Screenshot 2026-10-27 at 16.52.01.png|482]]
- Turn of cpu to reduce static power consumption, or enter lower power modes. 

Governors:
- A CPU governor controls the processors frequency and voltage to balance performance and power consumption.
- Governors on linux:
	- Performance: Runs CPU at max frequency.
	- powersave: runs CPU at min frequency.
	- ondemand: adjusts CPU frequency based on workload.
	- schedutil: Integrates with the scheduler for better responsiveness.
- Ondemand was popular, and still used on Rpi os and other embedded OS.
- Schedutil has become default CPU governor in many OS.



### 10 - Compilers and Code optimisations


What is a compiler:
- A program that takes a program as input (often human readable), creates a executable program as output.
- Gives you meaningful error message when something goes wrong.

#### Flavours/shades of compilers

Traditional compiler:
- A program that takes a program as input (often human readable), creates a executable program as output (assembly or binary).

Source-to-source compiler (Source code):
- Output is the same language as the source code, useful for rewriting with humans-in-the-loop.
- AKA: S2S compiler, transpiler.

Binary-to-binary compiler (source code):
- Like S2S but with binary.
- Used for translating architecture, improve performance or add monitoring.
- AKA: Static binary transformation.

Just-in-time compiler (execution):
- Generates executable code on-demand only for portion of program.
- AKA: JIT compiler, Lazy compiler.

Interpreter, technically not compiler (execution):
- Works instruction-by-instruction.

Cross compiler (code gen):
- Produces binary output for a different computer architecture.
- AKA x-compiler.

High level synthesis compiler (code gen):
- Produces output hardware description, eg. for reconfigruable hardware.
- AKA HLS compiler.


#### Compilation units in C/C++

Translation units / compilation units:
- Large programs are divided into compilation units.
- Provids modularity.
- Each CU has a file (c/cpp) to define source code, ideally only one.
- Header files (.h) declare public interfaces of units.
- Declaration: Informs the compiler of the existence of a symbol (variable or function), that can be shared with other CU.
- Definition: Provide function body, allocate memory for local variables and so on. Must exist in one and only one CU.

![[{7ABF08DD-C68D-4C06-9AA2-901D0500DA71}.png]]
#### Compiling stages:

Preprocessor:
- Inserts header contents where you write `#include`.
- Expands macros defined with `#define`.
- Outputs preprocessed C/C++ code.

Compiler:
- Compiles each unit from source code (.c) to human readable assembly code (.s). 
- Three stages.

- **Compiler Frontend (Understanding the language):**
	- Lexing: Split the text into meaningful pieces, such as keywords, variable names, numbers, operators.
	![[{B95CA7B8-2587-429F-9480-EDE2C33411D9}.png|523]]
	- Parsing: works out how those pieces fit together—for example, which expressions belong inside an if statement.
	![[{298D0563-CDEF-4A98-8149-FF60104ACE92}.png]]
	- Semantic analysis: Checks languages rules, and checks whether variables are declared and types are used correctly.
	- Result: A checked program represented as IR (intermediate representation).
	
- **Compiler Middle-End/optimizer (Optimize concepts):**
	- The IR is a compiler language that is easy to optimize, machine-idependent and language independent.
	- Analyses the IR to find calculations that can be simplified or removed.
	- Applies optimizations without changing the programs observable behaviour.
	- This includes:
		- Constant propagation: Replacing a variable with its known value.
		- Constant folding: calculate constant expressions during compilation.
		- Dead-code elimination: remove unused calculations or unreachable code.
		- Loop optimizations: avoid repeating unnecessary work.
	- Result: optimized IR.
	
- **Compiler backend (Generating a language):**
	- Converts the optimized IR into assembly for a particular instruction set, such as ARM or RISC-V.
	- Instruction selection: chooses suitable processor instructions for each operation.
	- Register allocation: decides which values go into which registers, reusing registers when possible. Values that do not fit may need memory.
	- Symbol table: Keep track of what symbols are (func, var etc.) and their memory location.
	- Result: target assembly code.

Assembler (Compiler Backend?):
- Assembles the assembly code into relocatable binary object code (.o).
- Converts instructions in assembly to instructions in binary encodings for the processor.
- Addresses can still be adjusted when combined with other object files.
- Output is binary object file. Contains: Symbol table, .text, .data, information about relocatable parts, and debug data.
- Two pass approach:
	- First pass: Generate symbol table.
	- Second pass: Resolve labels in this CU (replace with address of instruction/variable) and generate machine instructions.
![[{2C719B1D-3F07-4E86-A674-C327C0FDE3E9}.png]]

Linker (Compiler Backend?):
- If a the compiled main.c has a declared function swap, the linker combines main.o and swap.o, connects the call to the definition, and adjusts references between the files. It also handles required libraries.
- Combines these into an executable (.exe/.out), resolving references between units.
- Combines object files and required library code.
- Connects references between files—for example, a function call in one file to its definition in another.
- Adjusts addresses as the pieces are combined.
- Static libraries are copied into the executable, while dynamic/shared libraries are in a seperate file. The executable has information about whoch library functions it needs.
- Output: an executable.

Loader:
- Sets up executable program in memory and initialises data ares prior to running.
- The executable is initially a file on disk. The loader, which the lecture identifies as part of the operating system, sets it up in memory.
- It places the program’s code and data into memory, initializes the required data areas, and arranges for execution to begin.



#### Misc

Intermediate Representation (IR):
- The IR is a compiler language that is easy to optimize, machine-idependent and language independent.
- Basic block: a straight sequence of instructions, with one entry and one exit. Branching can happen at the end.
- Data-flow graph (DFG): shows which operations depend on results from other operations. Represent flow of data inside basic block.
- Control-flow graph: shows which basic block can execute next, including branches and loops.

Register allocation:
- Allocate registers to variables to minimise memory accesses.
- Liveness analysis: Compilers analyse lifetime of variables for register allocation.
- Volatile keyword: Some variables always need to be in memory, but still brought to register for using the value.

Dynamic linking:
- Most operating systems can link modules (shared libraries, .so on linux platforms) at load time.
- Saves storage compared to static linking.
#### Optimisations in action

A compiler pass:
- A pass is the elementary unit of work. Can be:
- Analysis pass: Does not change the code, only extract info.
- Transformation pass: Make some code more efficient or make some code easier to be optimised later (normalisation passes).
- By repeating these simple passes, compilers achieve big optimisations.

Dead code elimination:
- Eliminate assignments to variables that are never used, or basic blocks that arent reached.

Constant propagation:
- Identify variables that are constant, substitue the constant elsewhere. 

Constant Folding:
- Compute and substitue constant expressions.

![[{D88F93C9-7ADF-42CA-910B-566FC777F1AE}.png]]
![[{73A73833-BBD9-4F8D-90F5-0BF2BFC3B585}.png]]![[{9247E8BE-240C-45DB-A0D9-23AD1C62744F}.png]]![[{B77361D6-7756-458E-8D26-AE909FEBCFA5}.png]]

Loop optimizations:
- One of compilers most powerful weapons.
- Examples: Loop unrolling and Loop-invariant Code Motion.

Loop invariant code motion:
- move a calculation outside a loop when its result stays the same.

Loop unrolling:
- put several iterations’ work into one loop iteration.
- Faster counter makes for less loop checks.
- Bigger loop body creates opportiunities to bundle/reorder instructions and reduce stalls.




### 11 - Guest Lecture on edge AI


Edge AI:
- Runs a neural network close to where the data is collected.
- Local processing can reduce communication, reduce cloud response delay, and support realtime decisions. 
- Challenge is doing useful inference with limited resources.
- Specialized neural networks on real-time embedded systems.
- Some common metrics:
	- Energy per inference.
	- Low + predictable lantency.
	- Throughput.
	- Power consumption (cooling).


Execution time formula:
- T_exec = W / (E * P)
- \[Seconds] = \[Operations] / (\[0-1] * \[Operations per second])
- Where T is execution time, W is workload, E is efficiency and P is peak performance.
- Decreasing workload: Better algorithms.
- Increasing efficieny: Mapping optimizations.
	- Examples: Reuse buffers, remove unecessary operations, improve data reuse and more. 
- Increasing peak performance: Hardware optimizations.


Quantization:
- Use fewer bits for weights and/or activations. This reduces storage and memory traffic, and can improve computation speed if the hardware supports the precision efficiently. The tradeoff is possible accuracy loss. FP32 → INT8 gives approximately 4× smaller raw storage.


Sparsity:
- Many weights are zero, so their multiplications can potentially be skipped. However, storing nonzero locations adds overhead, and irregular accesses and work distribution can limit parallelism. More zeros do not automatically mean faster execution.


Topology:
- The network’s structure, layers, channels, and connections. A smaller network generally requires fewer parameters and operations, potentially at the expense of accuracy.






### 12 - Programming accellerators and course wrap up

RPI OS:
- Variants: Lite version without GUI, with  GUI and apps, 32vs64 bit.
- Memory: 80-600 MB.
- DISK: 1-4GB.
- Desktop OS, not typical embedded OS.

CFS: 
- Completely fair scheduler.
- Indirectly takes priority into acocount with "nice" values.
- Higer priority = larger share of CPU time.



 Coprocessors:
- Tightly coupled to the CPU.
- Executes certain opcodes/instructions.
- CPU can supsend execution and wait for result, or;
- Continue executing while waiting (superscalar processor).
- If coprocessor not available, the instruction is emulated in hardware (illegal instruction trap / interrupt). Much slower.
- ARM architecture can support up to 16 coprocessor.

Accelerators:
- Connected to CPU bus.
- Cooperates with CPU via shared memory.
- Data and control registers.
- Specialized ISA (if programmed), or hardwired logic (pure hardware), or FPGA, or combination.
- Shared memory (SoC).
- FPGA:
	- Field programmable gate array.
	- Writeable-hardware.
	- Can act as user programmable accelerator.
	- Lower efficiency but higher flexibility.

![[{DC45051D-03FA-4BAF-BAC3-3DC555F5997E}.png]]
Accelerators examples:
- DMA: Direct memory access. Moves data to/from memory to offload CPU:
- DPS: Digital signal processor. Measure, filter or compress continuous analogs signals.
- ISP: Image signal processor.
- GPU: Graphics processing unit. Used for calculations with high degree of parallelism, such as graphics, matrix/vector operations, AI related calculations and more. Programmed in CUDA or OpenCL.
- GP-GPU: General purpose gpu.
- NPU: Neural Processing Unit, used for AI and deep learning. Either train new AI models or inference. Uses smaller bit operators.
- TPU: Tensor processing unit. Programmed with tensorflow, also used for neural network / machine learning.


Wirths law:
- Software is getting slower faster than hardware becomes faster.


Good software design decisions:
- Choose the best algorithm for the problem, and that fits the hardware.
- Minimize memory size and access.
- Optimize performance of the application, making use of availabe parallelism.
- Take advantage of hardware support for memory management.

Race-to-halt:
- Finish work quickly, then enter low power state.

DVFS:
- Dynamic voltage and Frequency scaling.
- Can reduce clock frequency to reduce dynamic power, but does nothing for total energy.
- Can reduce voltage to reduce both static and dynamic power consumption.
- Need to adjust both of these so that the CPU is running just fast enough.

Multicore CPU:
- Each core has its own execution hardware and can run a separate thread.

MPSoC:
- Multiprocessor System-on-Chip.
- An entire system containing multiple processing units on one chip, together with interconnections, memory controllers, and other components.
- Can contain several CPU cores, interconnects, memory controllers, GPU, other accelerators.

Hyperthreading:
- Intel’s name for simultaneous multithreading (SMT).
- A physical core maintains separate execution state, such as registers and a program counter, for multiple threads.

Internet of things:
- Interconnected smart devices.
- Sensor, processing, communcation, database.
- Three main types: Implanted, wearable, environmental.
- Can used environment to harvest energy.


