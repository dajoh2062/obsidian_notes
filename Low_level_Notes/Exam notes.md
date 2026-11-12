# 0 - Course introduction


What:
- Understanding hardware.
- Memory, processor, buses and input/output devices
- Software development.
- Assembly, c programming, compilation and optimizations, OS basics.
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

C cheatsheet:
```C

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
- Big endianness: Most significant byte first.
- Little endianness: Least significant byte first.
- Little endianness is most common.


Cache:
- Block/ line: Unit of data stored in cache. Typically 32-128 bytes.
- Hit: Data found in cache level.
- Miss: Data not found in cache, must search and copied from higher level of hierarchy.
- Accessed either direct mapped, fully associative, or n-way set associative.


Direct-mapped:
- tag | index | offset
- The index selects one cache location, which holds one line.
- Compare the address’s tag with that line’s stored tag to check for a hit.
- The byte offset selects the required byte within the line.
- Problem: Several addresses might repeatedly compete for a few/ the same cache location

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











# 5 - More C programming, Cache and virtual memory



Cache replacement:
- Random.
- LRU (least recently used): Evict cache block that hasnt been accessed longest. Complex implementation.
- FIFO (first in first out): Same order as fill order.


Writing modified data:
- Write-back: When block is evicted, need to keep track of modified blocks ("D" - dirty 1/0). Saves resources.
- Write-through: When writing to cache we also write to memory. Simpler implementation and more coherence, but slower and resource intensive.

Cache performance:
- AMAT = (hit rate \* hit-time + missrate \* miss-latency)
- In cycles.

![[{B074A89F-C379-4805-88BD-EA168DBD548A}.png|579]]





### Virtual memory

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
- Corresponds to a cache line or block of virtual memory.
- "Page"/"virtual page" for virtual memory, and "page frame"/"physical page" for physical.
- 4-8 kb (4096-8192 bytes). Can be MB or GB in servers.
- Translation: Per program page tables. 
- Different programs can use the sam virtual address.

![[{7E7C6161-AB71-4034-A03B-E037DC32CA6C}.png]]![[{46DEBEE6-C298-4279-8D9E-6A5CBD3313ED}.png]]![[{0E0CF499-571E-443E-B192-FE2ED426F734}.png]]

Translation Lookaside Buffer (TLB):
- TLB is a cache (typically fully assosiative) for page table.
- Close to CPU and fast.
- TLB miss: Access page table and save translation in TLB.

![[{48B0D3B7-8F18-4A89-BF17-1B9289FA9CDD}.png]]


# 6 - Small microcontrollers and Energy efficiency | Microchip


Microchip:
- Microchip Technology is an American company that designs and manufactures semiconductors.
-  Its products include microcontrollers (small computers that control functions inside devices), chips for power management and connectivity, memory, and security.

### Basic electronics

- Voltage  = Resistance \* current.
- Power = Voltage \* current
- Power Consumption \[W]  = \[J] / \[S]
- Watt Hours = \[W] \* \[Hr]
- Ampere hours = \[A]\*\[Hr]

![[{0307895D-010F-4604-8E4B-94755187F617}.png|700]]


### Microcontroller architecture

![[{EFD2CBAD-5AB2-4EF5-8AA3-379BFFED9A3D}.png]]
AVR Architecture:

![[{6E85CB18-E3F9-4FF9-836E-0212485D2788}.png]]

PIC Architecture:
![[{EF5B55B4-8530-46F5-824D-B5D4AAE83DF7}.png]]
The main differences are:
- CPU design: AVR and PIC use different basic instructions and ways of doing calculations.
- Registers—tiny storage spots inside the CPU: AVR usually has 32 working registers; common 8-bit PICs do many calculations through one main working register, called W.
- Programming: The program must be built for the chosen chip. An AVR program cannot simply be loaded onto a PIC.
- Features: Memory, speed, pins, and built-in tools depend on the specific model, not just whether it’s AVR or PIC.

Microcontroller typical applications:
- Low power applications: Battery powered devices, IoT applications.
- Distributed logic and coexistence with larger processors: real-time control tasks, reading sensors, controlling simple user interfaces.

Microcontrollers concrete examples:
- Smartwatch, kitchen applicances, digital gadgets, vechicles.


### Low power:

![[{008E7344-8E4A-4453-89E3-CA55D502437C}.png]]
![[{16D197BC-4C47-46EE-B879-BE4FA1893B45}.png]]

Battery capacity:
- Battery time \[Hrs]  = Battery capacity \[mAh] / Current consumption \[A]


### Low power techniques

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


### Programming microcontrollers:


Languages:
- C, Embedded C, C++, python, java and more.

IDE:
- Arduino IDE, MPLAB X (VS extension).
- 1: Create project with main.c file.
- 2: Compile and build project.
- 3: Upload code (hex file) to MCU.

Machine language:
- Binary 0 and 1s. 

![[{A06433C7-1051-4803-9B29-FDFF8802BA57}.png]]![[{0963E8DF-B706-44F8-BAAA-10BD3A25CD63}.png]]

Bitmasks and bitwise operations:
- AND: &=
- OR: |=
- XOR: ^=
- NOT: ~(bits)


Analog comparator (AC):
- Output 1 when difference in volage is positive and 0 otherwise. 

![[{DCD865F8-58F5-4C71-9D1D-7CA98CE01817}.png|503]]


# 7 - Energy efficient computing

Energy efficient computing, motivation, theory, technology trends and accellerators.


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
- Turning of clock signal to circuit that is currently unused.
- Keeps store value as long as EN = 0, which sets clock at 0. Makes frequency (f) 0.
![[{DC72D1F9-D119-4EEB-8324-A0E74F480F42}.png]]

Power gating:
- Turns off power to the circuits not in use.
- An ordinary register loses its stored value when its supply is disconnected. After power returns, its value must be initialized or restored.
- Leakage is reduced way more than clock gating.
- Energy Overhead: require some energy to power down and power up a unit, so the energy saved should out-weigh.
- Performance overhead: Extra time required in relation to activating the unit.

OS power managment:
- Hardware provides multiple power modes, which Os can use to reduce consumption.
- Sleep modes provide maximum power saving. Similar to power gating but with software. Overhead on entering and exiting. Multiple levels to adapt the tradeoff.


Dynamic voltage and frequency scaling (DVFS):
- Lets OS choose between multiple voltages and clock frequencies.
- Reducing frequency reduces dynamic power consumption, but energy remains the same.
- Reducing voltage reduces both dymaic and static power consumption.
- OS needs to adjust voltage and frequecy such that the CPU runs just fast enough for task.



### Bare metal, runtime systems, and operating systems

Bare metal programming:
- Software runs directly on hardware. No system calls, libraries or drivers.
- Start from instruction at address 0.
- Reset handler must set up the stack pointer and call main.
- Minimal portability, no hardware abstraction and harder debugging.
- Max performance and control. Predictable timing, ideal for Realtime systems.

Runtime environment (RTE) and runtime system (RTS):
- Memory management (stack, heap/allocated memory, garbage collection).
- Aspects of programming languages that needs to be handled during run time, such as communcation between processes/threads, signals, dynamic loading and more.
- Distinction between runtim system and operating system is not clear.
- Examples:
	- FreeRTOS (from 1-2 kB RAM, from 5 – 10 kB ROM (e.g. FLASH))
	- Zephyr RTOS (from, 6-8 kB RAM, from 3-8 kB ROM)

Operating system (Os) and Real time Operating system (RTOS):
- File system, process management, security, networking and more.

![[{1FC1BD7E-8DEB-4E0A-9A13-6B107FB8BD99}.png]]
![[{EE06A4DE-5D99-4BD3-89DD-BB87925E91AA}.png]]![[{F69E6FC6-A8CC-45B4-A7FF-1269B9A543D0}.png]]
![[{4CC57407-AD57-4F28-AC38-F9396A9A23A1}.png]]
### Measuring performance

Performance monitoring unit (PMU):
- Counts hardware events that help explain execution time.
- Such as clock cycles, instructions completed, cache references, cache miss, branch mispredicts.
- Derived metrics: , instructions per completed clock cycle, cache miss rate, branch misses per instruction executed.

![[{FADC75DB-6457-4C50-83EF-66C89D2D8FD1}.png]]
For reliable measurements:
- Repeat experiments to reduce uncertainty.
- Distinguish cold-cache execution from execution after warm-up.
- Control interference from other tasks.
- Consider strided access: stepping through memory can use cache lines less efficiently than accessing adjacent elements.

Why use PMU:
- Big-O describes growth with input size; it does not capture instruction counts, memory accesses, or pipeline behavior. The counters show differences that execution time alone cannot explain.


### Sustainable computing
![[{462FEA5E-78CE-44DC-B552-C9001B11D0E0}.png]]
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
![[{16C343C3-08A3-42C0-BFCB-29FC8E741431}.png|593]]
### Technology trends and the power wall

Moore’s law (slowing/stopped?):
 - Historical trend of increasing transistor counts per chip.

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

![[{E4915225-947B-4350-8D3B-BE20479A0CFD}.png]]
### Power, energy, and gating

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
- Turning of power supply to circuits not in use, mainly reducing P_static by decreasing voltage. 
- More overhead than clock gating.

![[Screenshot 2026-10-26 at 14.43.00.png]]
![[Screenshot 2026-10-26 at 14.46.18.png]]


### Thermal limits and measurement

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
![[Screenshot 2026-10-26 at 15.05.40.png]]

Difference electrical enery and Power:
- Electrical energy: Amount of energy transferred to other forms (heat) in an electrical circuit during some amount of time. Measured in Joules ( J = W \* S)
- Power: Work done” (electrical energy transferred) per unit time. Measured in Watt (W = J / s). Also: P (watt) = U (Volt) \* I (Ampere).

Example 1: Raspberry Pi4:
- 3-6 watt.
- Runs at 5.1 volts, current depends on peripherals.
- With 6000 mAh (three AA batteries in series), it lasts only 3 h 20 min in idle.
![[Screenshot 2026-10-26 at 15.44.03.png|462]]

Example 2: AVR128DB48 Curiosity Nano:
- 13μ Watt – 39m Watt.
- Runs at 1.7-5.1 volts.
- With a 240mAh battery it lasts 20 years in idle!
![[Screenshot 2026-10-26 at 15.44.32.png|593]]

Thermal design power (TDP):
- Represents the average power the processor dissipates when operating at base frequency with all cores active under a high-complexity workload.

Hyperthreading:
- Allows one physical core to appear as two logical cores, to the OS, and therefore handle double the amount of threads. This makes it possible to better multiplexing multiple lightweight threads or processes.
- Seperate registers for each hyperthreaded core is what gives the performance boost, in addition to OS waiting times.
- HT is Intels marketing name for Simultaneous Multithreading (SMT).

Turbo boost:
- Dynamically increase clock to enhance performance when needed (within limits of TDP).
- Dynamic overclocking.

Cooling solutions:
- Heat sink: component that absorbs heat from a CPU or other electronic component and transfers it to the surrounding air to prevent overheating.
- Thermal interface materials (TIM): Thermal paste placed between the chip and the heat sink to improve thermal conductivity and ensure efficient heat transfer.
- Active cooling with fan.
- Water cooling.

Relevance to low-level programming:
- Reading temperature, fan speeds, controlling fan speeds.
- Controlled temperature is necessary for precise energy measurements.
- Temperature affects on-chip energy measurements.
- Made possible by Intel RAPL (Running Average Power Limit)

![[Screenshot 2026-10-26 at 16.58.47.png|651]]



### The vertical approach and efficiency metrics
![[Screenshot 2026-10-26 at 17.00.06.png]]
Computer architecture:
- the way a computer’s hardware components interact with each other and with the software.

Vertical Approach to Energy Efficiency:
- Main idea: Improve energy efficiency across all levels of a computer system, from software to hardware.
- Applications: Optimize algorithms, data structures, programming languages and parallelization.
- System software: Optimize operating systems, compilers, task scheduling, resource management and DVFS.
- Computer architecture: Improve multicore systems, accelerators, caches, memory and communication.
- Holistic approach: Consider how hardware and software interact rather than optimizing each layer independently.

Energy Efficiency Metrics:
- Energy: \(E=P\cdot t\), measured in Joules (J).
- Optimization goals: Maximize performance, minimize energy consumption, or balance both.
- Performance per Watt: FLOPS/Watt, used by Green500 to rank energy-efficient supercomputers.
- Energy Delay Product (EDP): \(E\cdot t\), measured in Joule-seconds (Js). Lower is better.
- Energy Delay Delay (EDD): \(E\cdot t^2\), measured in \(Js^2\). Penalizes slow execution more heavily.
- Benchmarks: SPEC evaluates CPU performance; EEMBC focuses on embedded processors.
![[Screenshot 2026-10-26 at 17.09.44.png|453]]

### Parallelism and specialization


Vectorization (SIMD):
- Vectorization: Performs the same operation on multiple data elements simultaneously (SIMD = Single Instruction, Multiple Data).
- History: Popular in Cray supercomputers (1976–1990), later returned in mainstream CPUs.
- Intel SIMD: MMX (64-bit), SSE (128-bit), AVX (256-bit), AVX-512 (512-bit).
- ARM NEON: SIMD extension used for audio, video, graphics and gaming.

Energy-Efficient Computing:
- Hardware techniques: Low-power design, DVFS, sleep modes, multicore and specialized hardware.
- Software techniques: Efficient algorithms, data structures, compilers and operating systems.
- End of Dennard scaling: Forced architects to exploit parallelism for better energy efficiency.
- Architecture-level optimization: Offers greater potential power savings than low-level circuit optimizations.

 Chip Multiprocessors (CMP):
- CMP: Multiple processor cores on the same chip, potentially sharing cache.
- Throughput: Multiple slower cores can achieve similar throughput to one fast core for parallel workloads.
- Energy efficiency: Lower frequency allows lower voltage, significantly reducing dynamic power.
- Example: Under ideal assumptions, 2 cores at half frequency and voltage consume only 25% of the original dynamic power.
- Limitation: Parallelization introduces overhead, and not all applications can be parallelized efficiently.

Multicore and MPSoC:
- Multicore: Modern name for Chip Multiprocessors (CMP).
- Cost: Several smaller processors can be cheaper than one very powerful processor.
- Real-time systems: Dedicated cores make timing guarantees easier.
- SoC (System on Chip): Integrates multiple components onto one chip, reducing cost.
- MPSoC: SoC containing multiple processors; physically separated processors form a distributed system.
![[Screenshot 2026-10-26 at 18.34.29.png]]
Accelerators and Parallelism:
- Accelerators: Specialized hardware for specific workloads, such as GPUs, DSPs and NPUs.
- BLP: Bit-Level Parallelism.
- ILP: Instruction-Level Parallelism.
- TLP: Thread-Level Parallelism.
- DLP: Data-Level Parallelism (e.g., SIMD).
- ALP: Accelerator-Level Parallelism — multiple accelerators execute different workload components concurrently.
- Example: Samsung Exynos 9810 combines CPU cores and an ARM Mali GPU in one SoC.

![[Screenshot 2026-10-26 at 18.31.02.png]]


# 8 - Tools for embedded systems programming and examples


### Warnings (compiler)

![[{1F801C08-943B-431C-AD1A-F31A13BB7517}.png]]![[{C092798F-0DBD-4993-843E-36F7538B2CBE}.png]]![[{B9E185C4-60DE-4EAC-AA06-F4985FC3353C}.png]]

Compiler warning:
- The warning flags can be combined.
- -Wall: only enables some warnings.
- -Wextra: More warnings.
- -Wpedantic: Catches use of extensions that does not conform to the standard.
- -Werror: Turns warnings into errors, causing compilation to fail.


### Static analyzers (compiler)

How:
- "-fanalyzer" flag.

What:
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

### Runtime sanitizers (compiler)

ASan - Address Sanitizer:
- "-fsanitize=address"
- Catches:
	- Out of bounds read/write
	- Use after free
	- Double free or invalid free
	- Use after return
![[{FCC01796-2A67-490C-AE80-A78800DE4647}.png]]
UBSan - Undefined Bheavior Sanitizer:
- C lets you write programs with undefined behavior.
- That can produce inconsistent results or errors.
- "-fsanitize = undefined"
- Can catch:
	- Signed integer overflow.
	- Integer divison by 0.
	- Invalid bit shifts.
	- Null pointer dereferences.
	- Misalligned memory access.
	- Som array out-of-bounds access.

LSan - Leak sanitizer:
- Reports leaked allocations, sizes and where they are allocated.
- "-fsanitize = leak"
- Detects memory allocations that werent freed and no longer reachable.


### Fuzzing (not really the compiler)

- A kind of automated testing
- Automatically feed a program millions of "random" inputs to discover bugs
- I say "random" because fuzzers can be quite intelligent in how they generate input and explore the space of possbile inputs.
- Combine fuzzing with the aforementioned sanitizers for a powerful bug finding combo
### Runtime analyzers

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

### Debugger

You can:
- Set a breakpoint: pause when execution reaches a particular line.
- Step through code: execute one line at a time.
- Inspect variables and memory.
- Inspect the call stack: see which functions called the current function.
- Investigate a crash: see where execution failed and inspect the available state.



# 9 - Embedded Operating Systems

Operating systems:
- Dekstop: Windows, linux, macos.
- Mobile: android, ios.
- Embedded os: Blackberry QNX, GreenHills software, arm mbed os.

Why are desktop os suitable for embedded devices:
- Usually monolithic or hybrid, providing numerous services for diverse hardware.
- Too large memory usage and disc space requirements.
- Dont provide special features needed: Real time guarantee, reliability by certain standards.

DO-178C:
- Software Considerations in Airborne Systems and Equipment Certification.
- Applications divided into 5 levels:
	- Catastrophic: Failure may cause deaths, and loss of airplane.
	- Hazardous: Failure has a large negative impact on safety, performance, or reduces the crews ability to operate the aircraft safely, and may cause fatal injuries among passengers.
	- Major: Failure significantly reduces safety margin or increases work load, may cause injuries.
	- Minor: Failure slightly reduces safety margin, or increases work load, such as rerouting or other inconveniences.
	- No effect: Failure has no impact on safety, aircraft operation, or crew workload.
![[Screenshot 2026-10-27 at 14.26.46.png]]
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

### Real-time Operating Systems (RTOS)

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

![[Screenshot 2026-10-27 at 16.10.47.png]]

Earliest Deadline First (EDF):
- Closest absolute deadline gets the highest priority.
- Dynamic priority.
- Preemptive.
- Cons: Less common, complicated to implement.
- Assumptions: Deadline = period, uniprocessors, no context switching overhead, no resource sharing.
- Schedulability test: 
![[Screenshot 2026-10-27 at 16.32.17.png|476]]


![[Screenshot 2026-10-27 at 15.55.30.png|677]]
![[Screenshot 2026-10-27 at 15.59.43.png]]
Priority inversion:
- Accessing critical sections should be atomic.
- Guarantee mutual exclusion using mutex lock or semaphore.
- Priority inversion is when a high priority task is indirectly blokced by a low priority task, due to the lower priority task has obtained a variable lock the higer priority needs.

![[Screenshot 2026-10-27 at 16.44.44.png]]
Priority inheritance:
- A solution to priority inversion problem.
- when a high-priority task has to wait for some resource shared with a running low-priority task, the priority of the low-priority task is temporarily promoted to the highest priority level of the pending tasks.
![[Screenshot 2026-10-27 at 16.45.55.png|456]]

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



# 10 - Compilers and Code optimisations




What is a compiler:
- A program that takes a program as input (often human readable), creates a executable program as output.
- Gives you meaningful error message when something goes wrong.



### Flavours/shades of compilers

Traditional compiler:
-  A program that takes a program as input (often human readable), creates a executable program as output (assembly or binary).

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



### Compilation units in C/C++

Translation units / compilation units:
- Large programs are divided into compilation units.
- Provids modularity.
- Each CU has a file (c/cpp) to define source code, ideally only one.
- Header files (.h) declare public interfaces of units.
- Declaration: Informs the compiler of the existence of a symbol (variable or function), that can be shared with other CU.
- Definition: Provide function body, allocate memory for local variables and so on. Must exist in one and only one CU.

![[{7ABF08DD-C68D-4C06-9AA2-901D0500DA71}.png]]
### Compiling stages:
- Compiler: Compiles each unit from source code (.c) to human readable assembly code (.s). 
- Assembler: Assembles the assembly code into relocatable binary object code (.o).
- Linker: Combines these into an executable, resolving references between units.
- Loader: Sets up executable program in memory and initialises data ares prior to running.



















# 11 - Guest Lecture on edge AI


# 12 - Programming accellerators and course wrap up