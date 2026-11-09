
# 1 - Introduction

Parallel computing and HPC history:
- **1758**: Multiple people sit together for 5 months to approximate the orbit of Halleys comet.
- **1765**: Neville Maskelyne begins calculating nautical almanacs for 1767 using himself and 5 employees.
- **1793**: Gaspard de Prony initiates a 6-year effort to produce 19 volumes of trigonometry tables, using 96 untrained workers.
- **1822**: Difference Engine #2, meechanical calculator for the Royal Astronomical Society.
- **1842**: Augusta Ada King writes about the translation of calculations into machine states for its successor (the Analytic Engine), and inadvertently invents programming.

Discoveries during human computer era:
- Overclocking: Paying workers more to finish faster.
- Programambility and library functions: Preparing idiot-proof instructions and tables for the computes (non-mathematicians).
- Fault tolerance: Many moving parts in the first mechanical computers meant some parts broke down over time.

Computers at War:
- Early high-performance computing was expensive and not very profitable.
- WWI created demand for calculations involving maps, ballistics, and navigation.
- Women performed much of this large-scale calculation work.
- WWII led to major investment in cryptanalysis at Bletchley Park.
- Alan Turing was among the researchers there.
- Konrad Zuse developed a programmable computer in 1941.
- IBM calculator pipelines were used for Manhattan Project calculations.

Computing After the Wars:
- Electronic and electromechanical computers developed rapidly after WWII.
- Reliability remained a major problem.
- John von Neumann studied how to build reliable systems from unreliable components.
- Robert Noyce integrated multiple switches onto silicon in 1959.
- Gordon Moore predicted continued transistor shrinking in 1965.

The Clock Race:
- Processor performance increased very rapidly from about 1959 to 2003.
- Higher clock frequencies were a major source of performance improvement.
- Around 2000, this growth began to flatten.

The Computing Boom:
- Post-war investment and semiconductor improvements caused enormous growth in computing power.
- Computers became important to the global economy.
- Computing changed from a specialized resource into a widely available commodity.
- This rate of improvement cannot continue forever.

Historical Lessons:
- Increased computing power can cause major changes in society.
- More computing power often requires long-term investment.
- Parallel computing was once the main way to increase performance.
- Today, parallelism has again become essential.

Why Parallel Computing:
- There is no real point where computing becomes “fast enough.”
- New computing power creates new applications that demand even more performance.
- Major problems such as climate modeling, medicine, and energy research require huge amounts of processing power.

Limits of Single Processors
- Increasing clock speed increases heat and power consumption.
- Processors cannot keep increasing frequency indefinitely.
- Very high clock speeds would make processors too hot.
- Modern systems increase performance by adding more cores instead.

Automatic Parallelization:
- Converting serial programs into efficient parallel programs automatically is difficult.
- A compiler may preserve correctness but still produce a slower program.
- Performance is harder to reason about than functional correctness.
- Efficient parallel implementations often require different algorithms.

The Future of Computing:
- Moore’s Law once helped overcome performance limits.
- Computers have become household products rather than huge investments.
- Scientific computing is no longer the main market driving hardware development.
- HPC increasingly depends on hardware developed for larger consumer markets.

The Wal-Mart Effect:
- Mass-produced hardware becomes much cheaper.
- Only 27 Cray-2 systems were built.
- Hundreds of millions of smartphones are sold.
- HPC benefits when its hardware is also useful in large consumer markets.

HPC and Consumer Technology:
- Hardware companies use HPC systems as test environments for new technologies.
- Technologies developed for supercomputers often later appear in consumer devices.
- Vector processing moved from supercomputers into normal CPUs.
- Large parallel systems eventually became compact accelerator cards and multicore desktops.

What the Course Covers:

- The course uses small physics simulations as examples.
- The focus is on parallel programming models.
- MPI is used for message passing.
- pthreads are used for thread-based programming.
- OpenMP is used for shared-memory parallelism.
- CUDA is used for GPU programming.

Why Learn Parallel Computing?

- Modern supercomputers already rely heavily on parallelism.
- HPC techniques often become relevant to consumer computers later.
- Learning parallel programming prepares you for future hardware and software challenges.

Main Takeaway:
- Hardware became strongly parallel around the early 2000s.
- Modern systems continue adding more processors, cores, and accelerators.
- Software has not adapted as quickly as hardware.
- Efficiently using multiple processors is therefore an important programming skill.


# 2 - The von Neumann computer


Memory:
- Receive address and return content at address.
- Receive content and destination address.
- Can be seperated into program and data memory.
- Interconnect: Interface between memory and external devices.

Simplified von neumann architecture:

![[Screenshot 2026-11-08 at 11.45.21.png]]

Dynamic and allocated memory:
- Stack for function arguments and local variables.
- Program code places data of dynamic size in heap.

![[{476B2365-5D34-4665-AC5F-49F960E5856F} 1.png]]
Abstract computer, sequential processing model:
- A program to run.
- A translation mechanism that separates data and instructions.
- An instruction-interpreting unit.
- A data-manipulating unit.

Strength - The von Neumann computer is a bridging model:
- Programmers can improve performance for every computer.
- Hardware designers can improve performance for every program.
- NB: No bridging model for parallel computing.

Weaknesses:
- Memory bottleneck (Von neumann bottleneck): Programs repeatedly read from and write to memory, so performance is limited by slow memory access even when the CPU is much faster.
- Sequential execution.
- Inefficient memory/program model: All memory is treated as one large address space, with no built-in distinction between active and inactive data, and the basic model only runs one program at a time.

# 3 - Cache and virtual memory



Memory hierarchy:
- Registers: 16-64 registers (8 bytes), 1 cycle access time.
- L1 Cache: 16-64kB, 5-10 cycle access time.
- L2 Cache: 256kb-32mB, 50-100 cycle access time.
- RAM: 4gB-16gB. 500-1000 cycle access time.

Cache types:
- Fully assosiative: Tag + Offset. No index bits, because we search all cache lines. 
- N - way Set assosiative: Index bits select a set, and a memory block can be stored in any line within that set. Tag + Set Index + Offset. Index selects a set, and tags are compared within that set.
- Direct-mapped: Tag + Index + Offset. Index selects exactly one cache line.

![[Screenshot 2026-11-08 at 12.42.58.png]]
CPU fetch sequence:
- CPU asks for array → cache miss → RAM loads array + nearby elements → cache → CPU

Cache hierarchy:
- L1: Close to registers, usually divided into instructions and data.
- L2: Larger but slower, typically on each core.
- L3: Even larger but even slower, typically shared between the cores.
- L4: If used, usually sits outside the CPU chip.

Virtual memory: 
- The MMU lets each process behave as if it has its own large, continuous address space.
- Virtual memory is divided into pages, while physical RAM is divided into equally sized frames.
- The OS maps virtual pages to physical frames.


Page protection:
- The OS keeps a page table that records which virtual page maps to which physical frame.
- Pages that are not currently available can be marked as protected.

Page faults:
- Accessing a protected or unmapped page causes a page fault.
- The CPU pauses the program and lets the OS decide what should happen.
- The OS may allocate a new frame or load the needed data from disk.

Mapping pages to frames:
- A virtual page can represent new empty memory or data from a memory-mapped file.
- The OS chooses a free physical frame and fills it with the correct data.
- The program does not need to know where that frame is physically located.

TLB:
- The Translation Lookaside Buffer stores recent page-to-frame mappings.
- This makes repeated virtual-to-physical address translation faster.

Address translation:
- CPU requests virtual address.
- Check TLB:
    - Hit: Use physical address.
    - Miss: Check page table.
- Check page table:
    - Valid: Update TLB → access RAM.
    - Invalid/not present: Page fault → OS handles it.
- If resolved, update page table and TLB → retry access.


Memory isolation and sharing:
- Several programs can share the same physical RAM.
- Only memory that is actually needed must be backed by physical frames.
- The OS prevents one process from accessing another process’s private frames.


Swap space:
- Rarely used pages can be moved from RAM to disk.
- This allows programs to use more memory than the available physical RAM.
- Heavy swapping is very slow and can make the system nearly unresponsive.

Page size:
- Large pages reduce page faults but may waste more memory.
- Small pages waste less space but cause more page faults.
- A common default page size is 4096 bytes.

Why virtual memory matters:
- It allows multiple programs to run efficiently on the same computer.
- Page faults can reduce program performance.
- Large allocations may reserve virtual memory immediately, while physical memory is assigned when accessed.

Page protection tricks:
- Programs can change protection settings on their own pages.
- They can register handlers for invalid memory accesses.
- This enables advanced techniques, but it is not important for this course.

# 4 - Instruction level parallelism


### Pipelining

Why:
- Improved switching frequencies made it tricky to complete one complex instruction each clock cycle.
- Early RISC architectures decided to break each operation into 5 stages to keep up:
	- IF -> ID -> EX -> MEM -> WB
- One instruction still takes 5 steps, but two instructions take 6, three takes 7 and so on.

![[{E495AF38-A333-4E1E-9983-B75FC1D581D2}.png|501]]
Stall and flush:
- Stall = Stop and wait. Flush = Throw away and restart from the correct path.
![[{6DE86BEB-A491-4240-8678-A4CBD20FB215}.png|485]]
### Out-of-order execution

Bernstein's conditions:
- Two statements will produce the same result no matter the order if;
![[{AC90617E-6CFA-48C4-8AE0-3CD537D291A7}.png]]
Dependences:
- Data dependence, Name dependence and Control dependence.

Data dependence:
- The result of an operation is an input to a following operation.
![[{8F6B2A1D-F342-4FA7-98BC-3B796167828F}.png|381]]
Name dependence:
- When a name is re-used for a different purpose, its first use must be finished before the second can begin.
- Programmers usually dont reuse variable names, and compilers fix most of it anyway. 
- Loop iterations can create this effect.
- At the instruction level, the CPU has a limited number of registers to use, they have to be recycled every so often.
![[{2512ABCD-7CA4-413C-A4A1-75192907F21D} 1.png|275]]
Control dependence:
- Branches in the program make it impossible to start operations simultaneously.
![[{8FDF5466-4354-4205-8E0D-421CD614626F}.png|219]]
Superscalar computer:
- A processor that uses multiple execution units to execute multiple independent instructions in parallel during the same clock cycle.

### Prefetching and branch prediction

- Programs often spend a lot of CPU time inside long loops.
- Loops commonly access memory in regular patterns.
- Example: accessing array elements one after another creates predictable memory addresses.
- CPUs can use these predictable patterns to prepare data and instructions in advance.

Prefetching

- Regular memory access can cause cache misses when execution reaches the end of a cache line.
- Prefetching detects patterns such as repeatedly accessing addresses with the same stride.
- The CPU starts loading upcoming data from memory before it is actually requested.
- This can reduce waiting time caused by cache misses.
- If the prediction is wrong, the prefetched cache space can simply be reused later.

Branch Prediction

- Branches such as if statements and loop conditions make it difficult to know which instructions should be loaded next.
- The CPU predicts whether a branch will be taken or not taken.
- A simple predictor may always guess taken or always guess not taken.
- Correct predictions keep the pipeline full and improve performance.
- Wrong predictions require incorrect instructions to be flushed from the pipeline.

More Advanced Branch Prediction

- Branch predictors can use previous branch results to improve future predictions.
- A 2-state predictor can adjust its prediction based on the previous branch result.
- A 4-state predictor can use more branch history before changing its prediction.
- More states allow the CPU to respond more carefully to occasional unusual branch outcomes.
- CPUs can also store branch history and statistics associated with branch instruction addresses.
### Vectorization
- Performs the same operation on multiple values at once.
- Uses SIMD: Single Instruction, Multiple Data.
- Common with arrays, matrices, graphics, and scientific calculations.
- Improves speed when many independent values need the same operation.


# 5 - Flynn’s taxonomy, shared & distributed memory

### Flynns taxonomy:
- Way to classify parallel computers. 
- Kind of old-fashioned.


![[{70D877C1-2B32-4688-8A13-2A4065278A4B}.png]]
SISD:
- Regular von neumann machine.
- One instruction at a time.
- One set of operands affected.
- Everything is sequential.

SIMD:
- Vector machine.
- Single instruction at a time but on many sets of operands simultaneously.

MISD:
- Not really interesting or useful.
- Only one set of operands at a time but can apply different instructions to them simultaneously.

MIMD:
- Multiple instructions and multiple operands at the same time.
- Meaning independent procsessors.
- Programs will make them work towards the same goal.
- Parallellism like threads, processes etc.

Another way to classify is shared and distributed memory variants.

### Shared memory
- Partitions the memory image of a process.

Threads:
- Dispatch concurrent function call until its result was required.
- Requires additional stacks and IPs.
- Multiple cores/processors with independent IPs and SP registers, make it so they can execute threads simultaneoulsy.
- Can lead to race conditions.

![[{CB5CF4EE-C42B-4512-9504-1CB4230FA632}.png]]
Pros:
- Only one copy of shared data which all can work on modifying, as long it is coordinated.

Cons:
- Threads have to live inside the address space of a single process, so a single OS must manage their memory, and therefore theycan’t live on separate computers.
- We only get as many as we can fit into one machine.

### Distributed memory

- Two processes cant communicate directly as the page tables dont allow it. They cant overwrite each others memory locations.
- Instead we can give them each others ID numbers, and allow them to establish some shared workspace under supervision.
- Function calls can transmit data to and from it, but traffic as to be initiated by the process itself.
![[{3AC1FE8A-5525-475A-8CC9-967D104B0133}.png]]
Pros: 
- Processes cant have their memory corrupted, as the only recieve what they want and where they want.
- Extra useful part: since address spaces are already completely distinct, the processes dont even have to be on the same computer. They can be connected via a network. 
![[{7686B438-0A14-44A1-A984-A3CED6363266}.png]]

Con:
- After transmission there are two copies of the thing you want to communicate which occupies twice as much memory.

### Multi-computer paralellism

Interconnects:
- Crossbar: Connect all proccessorss to all memory modules. Efficient but impractial for large systems.
![[{4645315E-DD27-49AB-B121-433EBCB509A4}.png]]
- (Fat) trees: Associate processors with memory modules that are their responsibility. Less expensive at scale but gives non-uniform memory access effects (NUMA). Higher bandwidth near root to compensate for structure.
![[{E6A07B1E-F420-40A8-AF2A-B13C4F8D9F56}.png]]
- Mesh: Constant number of links per unit, message routed through several hops. Scalable but latency is linear with distance.
![[{32624791-F25B-40EF-9871-809A32EC5D83}.png]]
- Torus: Mesh that wraps around edges.
![[{C01BF956-89B1-4AD8-B09B-304382057019}.png]]
- Hypercube: 3D mesh. Requires log2 P linkes per processor for P processors. Good for d-cube algorithms like the Fast Fourier Transform.![[{E3DD4DC2-C170-4709-8351-18FE7740EDC4}.png]]

Interconnection fabrics:
- Several of these graph shapes can be found at various levels of granularity in a large computer.
- In combination we call them the interconnect fabric.
- Some parts are memory logic, som are network connections.
- To a suitably parallelized program, they can combine into how much it costs to send data from A to B.


# 6 - Speedup, efficiency and scalability 


Every parallel computation can be serialized:
- Steps S1-Sn need to be taken, where subset of steps si-sj can be evaluated simultaneously.
- The parallel patrs of a comutation cant be dependant on any particular order, thats what make them parallel.

Not every sequential computation can be parallelized (probably):
- Steps S1-Sn all need to be taken.
- The input data of every step Si contains an element from the ouput of its predecessor Si-1.
- This makes some computations inherently sequential.

Categories of computational problems:
- P: Can solve it efficiently. Polynomial time.
- NP: I can check the proposed solution efficiently. Easy to verify, even if not easy to find solution. P=NP? means does every problem with efficiently checkable solutions also have an efficient solving algorithm.
- NC: Can solve problem quickly using many processors.
- P-complete: Can solve it efficiently (P), but not yet found a very fast parallel solution, that would be a breakthrough. P=NC? means does every P problem have a very fast parallel solution. 

Research:
- On paper, we dont know if all computations can be parallelized or not (P=NC?).
- We know that simulating T steps of a von Neumann machine is P-Complete though. 
- Always at least one sequential dependency in any program.
- Parallelizing a program amounts to discriminating between the sequentially dependent and the parallelizable steps.

### Execution time

Total execution time:
- T is the sum of the time costs of all operations in the program.
- F is the fraction of sequential operations it requires.
- 1-f is the fraction of operations that can be parallelized.
- Ts is the time it takes to run all in sequence.
![[{C9313E53-F113-4152-BAD5-7E6651E1C0E3}.png]]
Parallel execution time:
- Assume the parallel operations are evenly distributed among p processors.
![[{02AB5AF1-FBEA-4BED-8D41-8A5CD3B248CE}.png]]
Speedup:
- Speedup of fast solution relative to the slow one. Eg. 4x.
![[{B4370E21-C833-4936-A051-ABF572E79BA2}.png]]


Comparing Ts and Tp:
![[{FC50D257-9EFF-44D0-A088-E30E46685E16}.png]]
- No sequential dependecies would lead to:
![[{72E1D5C8-0708-4BFB-A8A8-0845C0E198A8}.png]]
- Cant have this linear speedup, as f cant be exactly 0.

Amdahls law:
- If we had as many processors as we wanted, it would mean we can only speed up the program a maximum of 1/f. 
- Slight flaw: Argument is based on solving the same, fixed problem on any computer. Assumes constant serial time.
![[{EF93969B-23B3-44C8-8CE3-D3176E83DF3D}.png]]

Gustafsons law:
- If we increase processor count, and problem size, how much more work can we do (depending on fraction of parallel operations).
![[Screenshot 2026-09-28 at 13.18.38.png]]
Speedup vs scaled speedup:
- Speedup: I can do the same amount of work in 1/x time.
- Scaled speedup: I can do x times the work in the same amount of time.
- Parallel efficiency: Amount of speedup divided evenly among the processor count. Drops in practise with growing p.
![[Screenshot 2026-09-28 at 13.26.55.png|288]]

Modes of scalability:
- Strong scaling: how speedup changes with processor count.
- Weak scaling: how scaled speedup changes with processor count.

Quality vs. quantity:
- Horizontal scaling: Upgrade system with more of the same components you had from before. Improves the parallel part of execution.
- Vertical scaling: Upgrade components with more powerful units. Improves sequential part of program.

Theory vs. practise:
- We assume that the parallel part can always be evenly and easily distributed amongst any number of processor.
- Gustafson pretends we can grow the parallel workload without increasing the sequential.
- It means that sometimes, using \(p\) processors can make a program run more than \(p\) times faster than using one processor (superlinear speedup).

# 7 - The advection equation

Objectives:
- Numerically integrating various functions accounts for a lot of applications in parallel computing.

Advection and diffusion:
- Diffusion: tendency to level out.
- Advection: How to medium it is in carries it along when moving. 
- Spreading out from dense concentration to less concentrated. Eg.: Heat, gases, water pollution.
- Example: Heating apartment without a fan vs. with a fan next to heating source.

Advection terms:
- U: Amount of whatever is spreading out.
- t: time axis.
- x: space axis.
- du/dt: difference in u when time changes.
- du/dx: difference in u from left/right in space.
- v: how quickly the medium is moving, and therefore transporting some of the U in either direction. 

Equation:
- Integrating would give us a function U(t, x).
- du/dt tells us if we will find more U at this given point later/earlier in time, and du/dx tells us how the concentration moves through space.
![[Screenshot 2026-09-28 at 13.59.48.png|368]]
Taylor polynomial to solve f'(x) from f(x) and dx:
![[Screenshot 2026-09-28 at 14.27.08.png|604]]





![[Screenshot 2026-09-28 at 14.29.57.png]]

Expression for change in U in time:
![[Screenshot 2026-09-28 at 14.36.47.png|577]]
Advection equation looking forward in time, and left and right in space:
![[Screenshot 2026-09-28 at 14.57.46.png]]

Boundary conditions:
	- Dirichlet: Outside points equal a constant.
	- Neumann: Mirror inside points.
	- Periodic: Outside points connect.


Small problem:
- Making small rounding errors for every time step.
- Replace one term on the right side adds some friction instead.
- New formula:

![[Screenshot 2026-09-28 at 16.44.57.png]]
Main stages of a simulation program:
- Initialize.
- Integrate with loop.
- Finalize.

# 30 - GPU and U part 1



### GPU history

1992 – Early Graphics Hardware:
- Graphics cards were mainly display adapters that stored and displayed images.
- The CPU performed all rendering, making graphics processing slow.
- SGI introduced OpenGL in 1992, a standard for graphics programming.

1996 – Hardware-Accelerated Graphics:
- The 3dfx Voodoo graphics card introduced hardware-accelerated 3D rendering.
- Graphics cards took over drawing tasks, freeing the CPU for other work.
- Games like Quake helped drive GPU development.

1995–2003 – Fixed-Function Graphics:
- GPUs became faster and supported better textures, resolutions, polygons, and lighting.
- Rendering was mostly fixed-function, meaning developers had limited control.

2001–2005 – Programmable GPUs:
- 2001: NVIDIA GeForce 3 introduced programmable vertex and pixel shaders.
- Shaders allowed developers to customize graphics calculations.
- Early shaders had strict instruction limits and initially no loops.
- 2002–2005: Shaders became more flexible, including support for loops.

2005–2006 – General-Purpose GPU Computing (GPGPU):
- Developers realized GPUs could process non-graphics data using shaders.
- GPUs could perform many independent calculations in parallel.
- This enabled faster scientific simulations and image processing.
- 2006: GPUs evolved into general-purpose, throughput-oriented processors.

2007–Today – Modern GPUs:
- GPU programming APIs emerged, including CUDA, OpenCL, Vulkan, ROCm, and SYCL.
- GPUs gained more processing power, memory, and specialized hardware.
- GPUs became important for scientific computing and AI, not just graphics.
- Performance increased significantly, but so did power consumption.

Key Takeaway:
- Display adapter → Hardware acceleration → Programmable shaders → General-purpose GPU.
- GPUs evolved from displaying images to performing massive parallel computations.
- The need for flexible graphics programming eventually enabled modern GPU computing.


### GPU vs CPU

CPU example specs (AMD ryzen 9 9950x):
- 16 cores, 32 threads.
- Power usage: 197w. 
- 16 visible registers per thread. 

GPU example specs (Nvidia RTX 5090):
- 21760 "cores".
- Power usage 314W.
- Divided into 170 SMs (streaming multiprocessors).
- The SMs contain each (4 partitions with 32 cores) 128  Cuda cores / 32-bit floating point units.
- About 512 registers per cuda core (average), 16384 per partition, or 65536 per SM. 
- 2048 threds per sm, 32  registers per thread. 
- 4 warps per sm, switch every cycle.

GPU memory system:
- GPU has shared L2 cache.
- Each SM has its own L1 cache and seperate instruction cache.
- VRAM: Bandwidth optimized GDDR. Latency is bad and 1000s of threads trying to access all the time. Solution: Fast and often context switching.
![[Screenshot 2026-11-09 at 12.11.58.png]]
Similarities CPU / GPU:
- Superscalar architecture.
- General purpose processor cores.
- Out-of-order execution.

Main differences:
- GPU has way more cores, but less flexible.
- GPU has more threads per functional unit (upto 2048 per SM, 16 per cuda core). 
- Different functional units: Low precision matrix, Special functions unit, Ray tracing acceleration, Rasterisation Texture Unit.


### GPU architecture

Warps:
- Each induvidual thread is executed in order.
- Threads are executed in groups of 32, with the same instructions. This makes it only need one decode for 32 threads.
- Reliable restriction, with advantages.
- Subclass of SIMD / vector architecure (Flynns taxonomy), called Single Instruction Multiple Threads (SIMT).
- Thread divergence: All threads in a warp must participate in the same branch path, for example in loops or if statements. Doesnt have to execute instruction, but cant do anything else either. 

Avoiding thread divergence:
- Avoid long if/else statements where threads are likely to choose different paths.
- Avoid loops with highly variable number of iterations.
- Branches often can be replaced with math.
- Rewriting multiple nested loops as a single loop.
- Moving as much code as possible out of loops / statements. 

Context switching:
- Why: Slow memory and fast switching (many registers and threads/warps). Allows us to hide memory latency.
- Done each cycle, 4 new warps per SM (1 per partition). Therefor, upto 1 thread per Cuda core.
- Each sm can have upto 2048 threads/ 64 warps saved in registers
- Many available warps to choose from keeps throughput high.

![[Screenshot 2026-11-09 at 13.10.06.png]]

# 31 - GPU and U part 2


Occupancy:
- Useful execution slots / total execution slots.

SM hardware limits:
- Typically 1024-2048 active threads (32-64 active warps).
- Usually 65536 registers.
- Usually need more threads than this.

Thread hierarchy:
- Grid: Three dimentional grid, gridDim.x, gridDim.y and gridDim.z.
- For example one thread per pixel in an image.
- Block: Chunk of the grid that can fit within an SM; blockDim.x, blockDim.y and blockDim.z.
- Blocks should be dimentioned so that they contain multiples of 32 threads (n warps).
- Blocks should also be dimentioned so that the fit as many warps as possible on the SM. 
- For example: A sm that supports 48 warps, can have 4 block with 12 warps each (384 threads).
- Block queue: Queue of block created after kernel is lauched. Blocks run until all threads/warps within are finished.

![[Screenshot 2026-11-09 at 13.31.46.png]]
![[Screenshot 2026-11-09 at 13.31.37.png]]
Thread interaction:
- Within same warp: cheap.
- Within same block: Easy, shared memory. Allows for some cool features.
- `_syncthreads();`: Barrier for all threads/warps in block.
- You can and should not try to synchronize threads across blocks, lauch multiple kernels instead.
- Atomics: Only mechanism for avoiding race conditions is atomic instructions.

```c++
int atomicAdd(int* address, int val);
int atomicSub(int* address, int val);
int atomicExch(int* address, int val);
int atomicMin(int* address, int val);
int atomicMax(int* address, int val);
unsigned int atomicInc(..);
unsigned int atomicDec(..);
int atomicCAS(int* address, int compare, int val);
int atomicAnd(int* address, int val);
int atomicOr(int* address, int val);
int atomicXor(int* address, int val);
```

Shared memory:
- Each SM has its own L1 Cache (upto 100 KB).
- `__shared__ int blockBuffer[20];` creates array of 20 integers in shared memory in the same SM.
- Can be accessed within the block.

### Programming the GPU



```C++
#include <stdio.h>

// Host: CPU
# __host__ void cpuFunction();

// Device: GPU
# __device__ void doSomething();
# __global__ mainKernel(); 

# Host and device functions cannot call eachother


// global marks a kernel to be run on gpu. 
__global__ void runMeOnGPU() {
	// print threads x coordinate within block. this block is 32*1*1 dim
	printf("I'm thread %i!\n", threadIdx.x);
}

int main() {
	// calls kernel function with 1 block of 32 threads (1warp)
	runMeOnGPU<<<1, 32>>>();
	
	// This makes the CPU wait until previously submitted GPU work has finished.
	cudaDeviceSynchronize();
	return 0;
}
```

```c++

// kernel function that sets each element of an array to 0, 
// using one thread per element
_global__ void setToZero(int* array, int length) {
	int arrayIndex = blockIdx.x * blockDim.x + threadIdx.x;
	if(arrayIndex < length) {
		array[arrayIndex] = 0;
	}
}

int main() {
	// array in c++
	std::vector<int> numbers = {1, 5, 3, 7, 4};
	// pointer to integer
	int* device_array = nullptr;
	// size of "numbers" in bytes
	int arraySizeBytes = sizeof(int) * numbers.size();
	
	// Allocate memory in VRAM, 20 bytes with address saved in "device_array"
	cudaMalloc(&device_array, arraySizeBytes);
	
	// copy from cpu to gpu (destination address, source data, number of bytes
	// to copy, direction of copy)
	cudaMemcpy(device_array, numbers.data(), arraySizeBytes,cudaMemcpyHostToDevice);
	
	// call kernel functions with 5 blocks * 32 threads to do the array manipulation
	// unecessary as only 5 threads (1 block with 27 unactive) is needed 
	setToZero<<<numbers.size(), 32>>>(device_array, numbers.size());
	
	// wait for gpu to finished recently called work
	cudaDeviceSynchronize();
	
	// free pointer on device
	cudaFree(device_array);
}
```

# 32 Cooperative Organization

More SM limits (blackwell):
- Limit 1: Max threads per block: 1024.
- Limit 2: Max number of blocks per SM: 24.
- Limit 3: Max number of warps per SM: 48 (1536 threads).
- Limit 4: Number of registers per thread max 255, (typically 32).
- Limit 5: Available shared memory: up to 100KB.
- Limit 6: Register requirements per warp limits how many warps that fit into a SM.
- Limit 7: Blocks cannot be partially loaded into a SM.
- Limit 8: Shared memory required by each blocks can also limit amount of blocks per SM.

Block size tradeoffs:
- Larger blocks: More cooperative ability between threads, better reuse of shared memory.
- Smaller blocks: Less waiting for all warps in block to finish, may but not always improve occupancy by allowing more warps to to fit in execution.

Block size:
- Even a small increase in block size or shared memory requirements can halve active warp count and therefore also performance. 
- Nvidia has tools available to help find the optimal block dimentions based on kernel.


Cooperative groups:
- Partitions threads in blocks and warps and partitions them into groups that can work together.
- Most collaboration stems from communication between threads in the same warp being cheap.

```c++
#include <cooperative_groups.h>

// Not required, but recommended
namespace cg = cooperative_groups;


```

Cooperative group types:
- Coalsced group: the threads in the current warp, that are currently executing.
- Block group: a group with all threads in the current block.
- Grid group: A group of all threads in a grid.
- Cluster group: Union of multiple thread blocks, unavailable to consumers. 

Coalsced group example:
```cpp
__global__ void kernel() {
	if(threadIdx.x < 12) {
		return;
	}
	// 20 active threads in the warp
	cg::coalesced_group warp = cg::coalesced_threads();
	
	// printing thread id and total threads in coalesced_group
	printf(“Thread %i/%i\n”, warp.thread_rank(), warp.num_threads());
}
int main() {
	kernel<<<1, 32>>>();
	cudaDeviceSynchronize();
	return 0;
}

```

Block Group Example:
```cpp
__global__ void kernel() {
	// create block group
	cg::thread_block block = cg::this_thread_block();
	
	// thread rank here is 0-255, and num threads is 256
	printf(“Thread %i/%i\n”, block.thread_rank(),block.num_threads());
	block.sync();
}
int main() {
	kernel<<<1, 256>>>();
	cudaDeviceSynchronize();
	return 0;
}
```

Grid group example:

```cpp
__global__ void kernel() {

	cg::grid_group grid = cg::this_grid();
	
	// here rank is between 0 and 5119, and num threads is 5120
	printf(“Thread %i/%i\n”, grid.thread_rank(),grid.num_threads());
}
// Restrictions
// - Must be able to run all blocks at the same time in gpu
// - Must use special function call syntax, not regular <<>>

int main() {
	// launches cooperative kernel (function, grid dimentions, block dimentions, no
	// kernel arguments)
	cudaLaunchCooperativeKernel(kernel, {20, 1, 1},{256, 1, 1}, nullptr);
	
	cudaDeviceSynchronize();
	return 0;
} 
```

Cooperative groups:
- num_threads() and thread_rank() are thin abstractions over threadIdx, blockDim, blockIdx, and gridDim.
- Advantages of groups: group partitioning, selective synchronization and somewhat simplified warp communication.
- Must contain thread count that is power of 2.
- Members of groups do not change.
- Need groups to sync in branch.

Group partitioning:
- Make smaller subgroups from groups.
```cpp
cg::thread_block block = cg::this_thread_block();

// Use if size known at compile time:
cg::thread_block_tile<8> tile8 = cg::tiled_partition<8>(block);

// Use if size unknown at compile time:
cg::thread_group tile8_dynamic = cg::tiled_partition(block, 8)


// Each thread performs some work
tile8.sync();

```

Warp partitioning:

```cpp

cg::coalesced_group warp = cg::coalesced_threads();

// Use if partition count known at compile time:
int label = threadIdx.x % 8;
cg::coalesced_group<8, int> part = cg::labeled_partition<int>(warp, label);

// Use if partition count unknown at compile time:
cg::coalesced_group<int> part_dynamic= cg::labeled_partition(warp, label);

// binary_partition: use if you only need to split in 2 using a
boolean predicate

```



# 33 - Shuffle shenanigans 

### Performance pitfalls:

Memory:
- 