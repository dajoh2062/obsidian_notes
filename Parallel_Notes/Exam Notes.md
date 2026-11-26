
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


# 8 - Concepts of MPI


What:
- Multiple copies of the program are started through a launcher program that tracks how to make connections between them
- Data sent from one process to another, gets its own copy.
- Single program multiple data (SPMD). Not flynn category, but programming style.

Communicators:
- Communcation and parallel work is coordinated based on "Rank" which starts at 0 -> p-1, and world communicator "size".
- Communication must have a matching statement. For example a send must have a recieve.
- Can also divide world communicator into sub-communicators, with its own "rank" and "size.
- Sub-Communicators can have virtual topologies such as cartesian coordinates.


Communication modes (blocking):
- Standard: Whatver mode is default. 
- Synchronized: Send-function will not return until reception is  acknowledged.
- Buffered: Explicitly manage the memory thats used for sending/receiving.
- Ready: Assume the receiver has already initiated the receive.


Collective operations:
- Operations every rank in communicator must call before completing.
- Broadcast: Distributing a value from one rank to all other.
- Scatter: Splitting large data sets into equal parts and distribute.
- Gather: Receiving a number of equal parts, and putting them back together into a large data set.
- Barrier: Makes all processes wait at the barrier.


Derived data types:
- MPI has a handful of built in data types: Integers, f loats, chars and so on.
- Can be combined into more elaborate structures.
- Lets you send structs, columns/rows, sub array and so on.

Parallel I/O:
- Common way to write/read is to have a master thread who organizes contents and writes/read the file.
- MPI has a feature to let ranks open and write to the same file in different parts, but only works if OS allows it.

Misc:
- MPI can do some one-sided communications as long as the receiver has a buffer for it.
- Its possible to receive from "anywhere".
- There are non-blocking collective operations.
- There is a performance profiling interface.


# 9 - Six function MPI

The six functions:
- `MPI_Init ( &argc, &argv );`
- `MPI_Comm_rank(MPI_COMM_WORLD, &rank);`
- `MPI_Comm_size(MPI_COMM_WORLD, &size);`
- `int MPI_Send(const void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm);`
- `MPI_Recv(const void *buf, int count, MPI_Datatype datatype, int src, int tag, MPI_Comm comm, MPI_Status *status)`
- `MPI_Finalize();`


# 10 - Advection equation using six-function MPI


How to split domain:
- Divide problem size by rank count, then allocat seperate parts. Then you dont need to allocate full domain in each process, which saves memory.
- Later concatenate the domain.
- Can add extra boundary values if necessary.
- Every rank can get an array with everyones domain size in it. Local_size containg \[5, 5, 4 ,4]
- With this array can also obtain global index for each process.
![[{0D8CBFE2-54C9-4EB7-BA32-F8CA2325954E}.png]]

Concatenating results:
- Different ways:
	- Allocating an array with size of rank 0 array in rank 0, then writing the results from each rank.
	- Can let the ranks take turns opening and writing to the same file. Still sequential.
	- Can make each rank save its own file and then concatenate. 
	- Can let everyone write at the same time to different  areas of the file.

Border exchange:
- Halo: A subdomain + its "ghost points".
![[{C0835FC7-B58A-4B3F-9243-E0610F961FCC}.png]]

# 11 - Collective operations (MPI)


Barrier:
- `MPI_Barrier(MPI_COMM_WORLD);`
- Synchronization feature of sorts, but not memory fence.

Broadcast:
- `int MPI_Bcast (void *buffer, int count, MPI_Datatype datatype, int root, MPI_Comm communicator)`

Reduce:
- `MPI_Reduce(&x, &total, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);`
- Other operations: MPI_MAX, MPI_MIN, MPI_PROD.

All reduce:
- `MPI_Allreduce(&x, &total, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);`


Scan:
- `MPI_Scan(&x, &running_total, 1, MPI_INT, MPI_SUM, MPI_COMM_WORLD);`
- Each rank gets the sum from rank 0 through itself, including its own value.
- Other operations: `MPI_MAX`, `MPI_MIN`, `MPI_PROD`.

Scatter:

- `MPI_Scatter(data, 1, MPI_INT, &x, 1, MPI_INT, 0, MPI_COMM_WORLD);`
- Rank 0 distributes its array `data`, giving one integer to each rank, stored in `x`.

Gather:

- `MPI_Gather(&x, 1, MPI_INT, results, 1, MPI_INT, 0, MPI_COMM_WORLD);`
- Collects one integer from each rank into `results` on rank 0, ordered by rank.

All gather:

- `MPI_Allgather(&x, 1, MPI_INT, results, 1, MPI_INT, MPI_COMM_WORLD);`
- Like gather, but every rank receives the complete array. No root argument.

All to all:

- `MPI_Alltoall(sendbuf, 1, MPI_INT, recvbuf, 1, MPI_INT, MPI_COMM_WORLD);`
- Each rank sends one integer to every rank: `sendbuf[j]` goes to rank `j`.
- `recvbuf[j]` holds the integer received from rank `j`. No root argument.


# 12 - Point to point communcation modes (MPI)



Send and receive:
- For mutual exchanges, avoids deadlock.
```c
int MPI_Sendrecv (
const void *sbuf, int scount, MPI_Datatype stype, int dest,
void *rbuf, int rcount, MPI_Datatype rtype, int source,
MPI_Comm communicator, MPI_Status *status
);
```


Communication modes:
- All blocking modes, meaning control doesnt return to caller before transmission can be guaranteed.
	- Standard mode.
	- Synchronized mode.
	- Buffered mode.
	- Ready mode.

Standard mode:
- `MPI_Send`
- At liberty to do what is best on this machine and return control as soon as it can.

Synchronized mode:
- `MPI_Ssend`
- Same argument list.
- A bit slower, as call will not return to sender before receiver starts receiving.

Buffered mode:
- `MPI_Bsend`
- Lets you allocate the buffer yourself, so you can make it a one long contiguous memory range for sending many tiny messages.
```C
int buffer_size = n*sizeof(msgsize) + MPI_BSEND_OVERHEAD;
int *my_buffer = malloc ( buffer_size );
MPI_Buffer_attach ( my_buffer, buffer_size );

// ... MPI_Bsend

MPI_Buffer_detach ( &my_buffer, &buffer_size );
```

Ready mode:
- `MPI_Rsend`
- Has to be 100% sure the recveiving call has been made.


Non blocking communication:
- Very useful for overlapping communication with computation.
- An extra argument `MPI_Request *request`
- Returns immediatly.
- Frees program to do other stuff, then you can make sure the transfer has completed later.
- The modes are `MPI_Isend`, `MPI_Issend`, `MPI_Ibsend` and `MPI_Irsend`.
- Also: `MPI_Irecv`

```C
int MPI_Isend (
	const void *buffer,
	int count,
	MPI_Datatype type,
	int destination,
	int tag,
	MPI_Comm communicator,
	MPI_Request *request
);


// ... program
int MPI_Wait ( MPI_Request *req, MPI_Status *stat);
// can aslo put MPI_STATUS_IGNORE if it is not needed


// Can also make array to save these request
MPI_Request my_reqs[42];
// and wait for all
MPI_Waitall ( 42, my_reqs, MPI_STATUSES_IGNORE );

```


Persistent sending and receiving:

```C
int x;
MPI_Request req;

// Prepare once — on the sending rank
MPI_Send_init(&x, 1, MPI_INT, 1, 7, MPI_COMM_WORLD, &req);

for (int i = 0; i < 10; i++) {
    x = i;
    MPI_Start(&req);
    // Other work that does not modify x
    MPI_Wait(&req, MPI_STATUS_IGNORE);
}

MPI_Request_free(&req);

// On the receiving rank, use MPI_Recv_init instead:
// MPI_Recv_init(&x, 1, MPI_INT, 0, 7, MPI_COMM_WORLD, &req);
// Then use the same Start → Wait cycle before reading x.
```

```c
int x;
MPI_Request req;

// Prepare: receive one integer from rank 0, with tag 7
MPI_Recv_init(&x, 1, MPI_INT, 0, 7, MPI_COMM_WORLD, &req);

for (int i = 0; i < 10; i++) {
    MPI_Start(&req);                    // Start one receive
    MPI_Wait(&req, MPI_STATUS_IGNORE);  // Wait for its message
    printf("Received %d\n", x);         // Now safe to read x
}

MPI_Request_free(&req);
```


# 13 - Timing, scatter and gather


Timing:
```C
// Optionally you can use barrier here to isolate the section you want to time
//MPI_Barrier(MPI_COMM_WORLD);


double t_start = MPI_Wtime();

do_something_useful();

double t_end = MPI_Wtime();

printf ( “Something useful took %lf seconds!\n”, t_end – t_start );

// You will get p different timings
```

Approximate communication time:
- Sending size n message will be latency + inverse of bandwidth (seconds per byte).
- "Hockney model" or pingpong model.
- Hockneys experiment: Start clock, repeat a lot of iterations of; sending message a to b, and b to a, then stop clock, and divide time difference by 2, and number of messages. 
- Can extract latency and bandwidth by varying size of message and number of messages.
![[{EC37E60F-90B4-410B-A138-296791659F75}.png|275]]
Scatter:
- `MPI_Scatter` distributes domain to processes.

```c
// the four first elements are only needed in root.
// sendbuf must be p times the size of sendcount
int MPI_Scatter(
	const void *sendbuf, int sendcount, MPI_Datatype sendtype,
	void *recvbuf, int recvcount, MPI_Datatype recvtype,
	int root, MPI_Comm comm
);
```

Gather:
- `MPI_Gather` collects domain parts to root process.
- recvbuf has to be p times size of recvcount.

```C
int MPI_Gather(
const void *sendbuf, int sendcount, MPI_Datatype sendtype,
void *recvbuf, int recvcount, MPI recvtype, // only for root
int root, MPI_Comm comm
);

```



# 14 - Derived data types

Derived types:
- Can use derived types to be more elegant than packing.
- Combines some other types in a structure that describes their layout in memory.
- Derived types must be constructed and comitted to MPI for compiling and efficient representation, and then be used just as regular types.
- Consists of type signature tn and displacement dn.
- Displacement is memory offset compared to lower bound.
![[Screenshot 2026-10-09 at 17.45.49.png|606]]

Memory locations:
- MPI_Aint: A special integer type provided by MPI that is large enough to represent memory addresses.
- Displacements have this type.
- Wrapper for pointers.


Size of derived type:
- Size: Sum bytes required for the structure (example 6 bytes for 1 float and 2 chars).
- Extent: Sum bytes required for the structure if including spacing and gaps (example will be 9 bytes).
- Can increase extent manually and add padding. Example of two float in each struct with 4 bytes padding between.
![[Screenshot 2026-10-09 at 18.06.51.png]]

Resizing types:
```C
int MPI_Type_create_resized (
	MPI_Datatype old_type, // Type to start with
	MPI_Aint lower_bound, // New value for lower bound
	MPI_Aint extent, // New value for extent
	MIP_Datatype *new_type // Result comes out here
);

MPI_Datatype new_type;

MPI_Type_create_resized(
    MPI_INT64_T,
    0,
    8 * sizeof(int64_t),
    &new_type
);

MPI_Type_commit(&new_type);

// Sends data[0], data[8], data[16]
MPI_Send(data, 3, new_type, 1, 0, MPI_COMM_WORLD);

MPI_Type_free(&new_type);
```

MPI extent:
- Extent defines the distance in bytes between the starting positions of two consecutive instances of a datatype.
- Displacement defines where data is located within one instance of the datatype.
- Size is the number of bytes of actual data being sent, while extent can include gaps between elements.
- Example: A datatype contains two floats at offsets 0 and 8. Its default extent is 12 bytes (assuming 4-byte floats).
- Resizing the extent: Using MPI_Type_create_resized(), we can change the extent from 12 to 16 bytes without changing which floats belong to the datatype.
- Why is this useful? When sending multiple instances (count > 1), MPI uses the extent to determine where the next instance starts.
  - Extent = 12 → reads offsets 0, 8, 12, 20
  - Extent = 16 → reads offsets 0, 8, 16, 24
- Important: Changing extent does not change how much data is sent per instance or add padding to memory. It only changes the spacing between repeated instances.

Creating structured types:
```C
int MPI_Type_struct (
	int count, // How many parts do we have?
	int * array_of_blocklengths, // What are their block lengths?
	MPI_Aint *array_of_displacements, // What are their displacements?
	MPI_Datatype *array_of_types, // What are their types?
	MPI_Datatype *new_type // Output: a brand new type
);

int MPI_Commit_type ( MPI_Datatype *t );
// make the translation from MPI types into the native
//addressing mechanism of your computer


// in practise 
MPI_Datatype my_type;
MPI_Type_struct ( foo, bar, baz, &my_type);
MPI_Commit_type ( &my_type );
MPI_Send ( ptr, 2, my_type, dst, tag, MPI_COMM_WORLD )

struct Person {
    int age;
    float height;
};

struct Person people[2] = {
    {20, 1.80},
    {25, 1.75}
};

// Define the datatype
int block_lengths[2] = {1, 1};

MPI_Aint displacements[2] = {
    offsetof(struct Person, age),
    offsetof(struct Person, height)
};

MPI_Datatype types[2] = {MPI_INT, MPI_FLOAT};

MPI_Datatype person_type;

MPI_Type_struct(
    2,
    block_lengths,
    displacements,
    types,
    &person_type
);

MPI_Type_commit(&person_type);

MPI_Send(people, 2, person_type, dst, tag, MPI_COMM_WORLD);

```


Vector types:
- Useful when dealing with multidimetional arrays and more.
- Useful when collecting values with a common stride between.
- Consists of count, block length, common stride. 

```C
MPI_Datatype my_type;

// 5 blocks, 2 doubles per block, stride of 5 doubles
MPI_Type_vector(5, 2, 5, MPI_DOUBLE, &my_type);
MPI_Type_commit(&my_type);

// Send columns 0 and 1
MPI_Send(&ARRAY(0,0), 1, my_type, dst, tag, MPI_COMM_WORLD);

// Send columns 2 and 3
MPI_Send(&ARRAY(0,2), 1, my_type, dst, tag, MPI_COMM_WORLD);

MPI_Type_free(&my_type);
```

![[Screenshot 2026-10-12 at 14.07.55.png|221]]

Subarrays:

```C
double matrix[4][5] = {
    { 1,  2,  3,  4,  5},
    { 6,  7,  8,  9, 10},
    {11, 12, 13, 14, 15},
    {16, 17, 18, 19, 20}
};

MPI_Datatype my_type;

int sizes[2]    = {4, 5};  // Entire matrix: 4 rows, 5 columns
int subsizes[2] = {2, 3};  // Select 2 rows, 3 columns
int starts[2]   = {1, 1};  // Start at row 1, column 1

MPI_Type_create_subarray(
    2,
    sizes,
    subsizes,
    starts,
    MPI_ORDER_C, // C style order (row major), MPI_ORDER_FORTRAN for cmo
    MPI_DOUBLE,
    &my_type
);

MPI_Type_commit(&my_type);

// Send selected subarray
MPI_Send(&matrix[0][0], 1, my_type, dst, tag, MPI_COMM_WORLD);

MPI_Type_free(&my_type);
```


Other types:
```C
int MPI_Type_contiguous (int count, MPI_Datatype oldtype, MPI_Datatype *newtype);
// Block of memory

int MPI_Type_indexed (
int count, // Nr. of parts
int *block_lengths, // List of blocklengths for the parts
int *displacements, // List of their displacements
MPI_Datatype oldtype, // What kind of elements?
MPI_Datatype *newtype
);
// Similar to MPI_Type_struct just with all elements same type.

```

# 15 - Communicators (MPI)

Communicator:
- A set of ranks, with a size.
- Can be different structures.

Group:
- Set of ranks used to create a communicator.
```c
// int MPI_Comm_group ( MPI_Comm comm, MPI_Group *group );
//fetches the group of ranks in the communicator

// This gets us a group that contains all the ranks
MPI_Group everyone;
MPI_Comm_group ( MPI_COMM_WORLD, &everyone );

// Include
int keep[3] = {0, 2, 4};

MPI_Group A;
MPI_Group_incl(everyone, 3, keep, &A);

// Exclude
int remove[3] = {1, 3, 5};

MPI_Group evens;
MPI_Group_excl(everyone, 3, remove, &evens);

// Create another group example
int keep_B[3] = {2, 3, 5};

MPI_Group B;
MPI_Group_incl(everyone, 3, keep_B, &B);

// Now we have:
// A = [0, 2, 4]
// B = [2, 3, 5]

// Union
MPI_Group united;
MPI_Group_union(A, B, &united);

// Intersection
MPI_Group shared;
MPI_Group_intersection(A, B, &shared);

// Size
int size;
MPI_Group_size(A, &size);

// Create new communicator with the group
MPI_Comm subcomm;
MPI_Comm_create(MPI_COMM_WORLD, A, &subcomm);

```

Graphs:
- Can arrange ranks in a communicator into a directed graph, where each rank points at its neighbours.
- Heap: Parent = n/2, left = n\*2, right = n\*2 +1

Heap structure example :
```C
// Neighbours for ranks 0, 1, 2, 3, 4, 5, 6:
int edges[] = {1, 2,  0, 3, 4,  0, 5, 6,  1,  1,  2,  2};

// Cumulative neighbour counts:
int index[] = {2, 5, 8, 9, 10, 11, 12};

MPI_Comm tree;
MPI_Graph_create(MPI_COMM_WORLD, 7, index, edges, 0, &tree);


int rank, count;
MPI_Comm_rank(tree, &rank);

int neighbours[3]; // At most 3 neighbours in our binary tree

MPI_Graph_neighbors_count(tree, rank, &count);
MPI_Graph_neighbors(tree, rank, count, neighbours);
```



# 16 - Cartesian communicators and Parallel I/O (MPI)


### Cartesian communicators

- Special case of graph communicator.
- Left/right (1D), Up/Down(2D), in/out (3D).

```C
#include <mpi.h>
int main(int argc, char **argv)
{
    MPI_Init(&argc, &argv);

    // Get the total number of processes
    int size;
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    // Choose a 2D process grid automatically: dims[0] = x, dims[1] = y
    int dims[2] = {0, 0};
    MPI_Dims_create(size, 2, dims);

    // No wraparound along either axis
    int periods[2] = {0, 0};

    // Allow MPI to assign different ranks in the new communicator
    int reorder = 1;

    // Create the Cartesian communicator
    MPI_Comm cart_comm;
    MPI_Cart_create(
        MPI_COMM_WORLD,  // Source communicator
        2,               // Number of dimensions
        dims,            // Number of processes along x and y
        periods,         // Wraparound settings for x and y
        reorder,         // Whether MPI may reorder ranks
        &cart_comm       // Output communicator
    );

    // Find this process's rank and coordinates in the Cartesian grid
    int rank;
    int coords[2];
    MPI_Comm_rank(cart_comm, &rank);
    MPI_Cart_coords(cart_comm, rank, 2, coords);
    // coords[0] = x, coords[1] = y

    // Find neighbouring ranks in cart_comm
    int left, right, down, up;

    //communicator, dimension, distance, source, destination
    MPI_Cart_shift(cart_comm, 0, 1, &left, &right);
    MPI_Cart_shift(cart_comm, 1, 1, &down, &up);

    // Use cart_comm and the neighbour ranks for communication here

    MPI_Comm_free(&cart_comm);
    MPI_Finalize();
    return 0;
}
```
    
### I/O

Sequential file I/O:
- Usually operating systems only provide system calls for:
	- Open files for reading/writing at the start.
	- Read/write the next n bytes.
	- Append n bytes.
	- <stdio.h> offers little random access.
- Even with random access, simultaneous writes to a shared storage device can become a bottleneck because the device has limited capacity to process them in parallel.

Parallel file systems:
- Data: the actual contents of the file, such as numbers from your simulation.
- Metadata: information describing the file, such as its name, directory, size, permissions, and how to locate its contents.
- The central idea is one logical file, with its contents distributed across storage devices for parallel access.

MPI_IO:
- All ranks can open the file simultaneously.
- View: Window where a rank can write without touching other ranks data.
- Wont actually get much speedup unless running on a machine with parallel disk arrays and the right file system.

Example use with indexing:
```c
#include <mpi.h>

int main(int argc, char **argv)
{
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    // Declare an MPI file handle
    MPI_File file;

    // All processes open the same file
    MPI_File_open(
        MPI_COMM_WORLD,                     // Participating processes
        "results.bin",                      // File name
        MPI_MODE_CREATE | MPI_MODE_WRONLY,  // Create if missing; write only
        MPI_INFO_NULL,                      // No extra optimization hints
        &file                               // Store the file handle here
    );

    // Each process writes to a separate position
    MPI_Offset offset = (MPI_Offset)rank * sizeof(int);

    MPI_File_write_at_all(
        file,               // Open file handle
        offset,             // Position in bytes with the default file view
        &rank,              // Address of the value to write
        1,                  // Write one value
        MPI_INT,            // Value is an integer
        MPI_STATUS_IGNORE   // Ignore the returned status
    );

    // All processes close the file
    MPI_File_close(&file);

    MPI_Finalize();
    return 0;
}
```

Example with view:
```c
#include <mpi.h>

int main(int argc, char **argv)
{
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    MPI_File file;
    MPI_File_open(
        MPI_COMM_WORLD,
        "results.bin",
        MPI_MODE_CREATE | MPI_MODE_WRONLY,
        MPI_INFO_NULL,
        &file
    );

    // Each process's view starts at a different byte position
    MPI_Offset displacement = (MPI_Offset)rank * sizeof(int);

    MPI_File_set_view(
        file,           // File handle
        displacement,   // Starting position in BYTES from file beginning
        MPI_INT,        // etype: unit used for offsets within this view
        MPI_INT,        // filetype: contiguous integers, with no gaps
        "native",       // Use the machine's native data representation
        MPI_INFO_NULL   // No extra optimization hints
    );

    // Write at the beginning of this process's view
    MPI_File_write_all(
        file,
        &rank,
        1,
        MPI_INT,
        MPI_STATUS_IGNORE
    );

    MPI_File_close(&file);
    MPI_Finalize();
    return 0;
}
```



# 17 - Pthreads introduction

Stack:
- Stores local variables and function calls; memory is automatically managed.


Heap:
- Heap: Stores dynamically allocated memory (`malloc()`), which must be manually released (`free()`).

Data:
- Data segment: Stores global and static variables that exist throughout the program.

![[{3BBB9BF9-641B-4DFF-9BCB-A4BF37A6AC52}.png]]
![[{567EE948-E9A2-40A3-93A7-C96517B62B6E}.png]]
![[{BD5C477E-8165-4CCE-B605-12F2BA4DDAE4}.png|507]]
Concept of concurrency:
- Can improve programs by relaxing the restriction that the statements must only be completed in one specific order.
- Examples: database transactions, computations, web pages served, image rendered.

The PRAM model:
- Parallel random access machine, simplfied theoretical computer.
- Four conflict write policies:
	- Common value: Only allows asignment if values are the same.
	- Arbitrary (Actually used): Random value among the attempts.
	- Priority: Gets value from highest priority thread.
	- Reduction: Assigns result by doing a operation on the attemps, such as sum, prodcut, AND.

Shared memory parallelism:
- Pricate work space: local variables in stack.
- Shared global works space: data and heap can be used for collaboration and communication.

Race Condition:
- Occurs when multiple threads access shared data concurrently (with at least one modifying it), and the result depends on the order or timing of execution.
- The code segment that must not overlap is called a critical section.

Mechanisms to achieve exclusive access in critial sections:
- Atomic operations
- Load Linked & Store Conditional instructions
- Mutex (lock) and semaphore data structures
- Higher-level programming model constructs
- All need hardware support for underlying operations. Dijkstras implementation in purely software leads to very slow performance.

Pthreads operations:
- Start.
- Stop.
- Set and release a memory lock.
- Wait until some other thread wakes them up.
- Wait for every other thread at a barrier.

Pthread limitations:
- Slow implemetation and lots of code required for tasks.
- Alternative: OpenMP.


# 18 - Creating and removing pthreads

![[Screenshot 2026-10-12 at 18.07.06.png|641]]
Using pthreads:
- Posix: Portable Operating System interface.
- Requires OS support, which most do.
- Need a function to call.
- `#include <pthreads.h>`
- `-pthread` compiler flag
- "Threadable" functions have this signature ```void *my_function ( void *argument ){}```. 
- The function body will have to cast the argument into the type it expects.
- The caller of the function will have to cast the return value to what it expects.
- `pthread_t mythread;`
- `pthread_create(&mythread, settings, &function, &argument);`
- `pthread_join(thread, &result);
- Settings: can set priority, lifetime, stacksize and more.

Example:
```c
#include <pthread.h>
#include <stdio.h>

void *double_number(void *argument) {
    int *number = (int *)argument;  // We know the argument points to an int

    *number = *number * 2;   // Modify that int
    return argument;        // Return a pointer to the same int
}

int main(void) {
    int value = 21;
    pthread_t thread;

    // Pass the address of value to double_number:
    pthread_create(&thread, NULL, double_number, &value);
    // (thread address, settings, function, argument)

    void *result;
    pthread_join(thread, &result); // Wait and collect its returned pointer

    int *answer = (int *)result;
    printf("%d\n", *answer);       // Prints 42

    return 0;
}
```


HPC - style pthreads:
- Usually dont want more threads than cores.
- All threads tend to do same thing.
- Perform on large amount of shared data, making it pointless to pass and recieve values from function calls.
- Works by giving each thread unique index and calculating its parts from of the global problem from the index and total number.
- Common to switch the void* argument for an integer instead, an have the amount of threads as a global variable (because memory addresses are intergers anyway).


# 19 - Pthread operations and synchronization

### Lock/unlock

01_pi_seq:
- Sequentially estimating pi using integration.

02_pi_nolock.c:
- Parallelizing it badly.
- Making pi global and everyone contributes to the same variable.
- Get wrong answer because of race condition, and bad performance.

03_pi_diy_lock.c:
- Attempt at mutex.
- Using a shared integer "flag" to say whose turn it is given by ``
  `flag = (flag + 1) % n_threads` and `while (flag != tid)`
- Each thread busy waits for their turn.
- Serialized but only without certain compiler optimizations. With optimizations it might cause a deadlock.
- Volatile keyword is not memory fence or prevent race conditions, but it stops the compiler from moving read and write instructions that access it around in the code.

04_pi_mutex.c:
- shared variable `pthread_mutex_t lock;`
- Initialize with `pthread_mutex_init (&lock, NULL);`
- Destroy with ``pthread_mutex_destory (&lock);`
- Still bad performance.
- Can use the lock with the following sequence:
```C
pthread_mutex_lock ( &lock );
pi += H / (1.0 + x*x);
pthread_mutex_unlock ( &lock );
```


05_pi_mutex_fast.c:
- Make local partial sums then update shared variable safely in the end.
```C
pthread_mutex_lock ( &lock );
pi += pi_local;
pthread_mutex_unlock ( &lock );
```


Synchronized iterations:
- Many scientific applications use this kind of data-parallel computation seperated by synchronization. Lots of parallel programs use this pattern aswell.


### Condition variables

06_pi_cond_signal.c:

- Uses a mutex, condition variable, and counter.
- Early arrivals lock, increment the counter, and sleep.
- Waiting releases the mutex so other threads can arrive.
- The final arrival sees that everyone has arrived and skips sleeping.
- It decrements the counter, signals one waiting thread, and unlocks.
- Each awakened thread regains the lock, decrements the counter, and signals another.
- The final departing thread skips signaling because nobody remains.
- Threads are released through a chain of wake-ups.

Two barriers are used:

- First: ensure everyone has finished contributing to `pi`.
- Then one thread uses the result and resets `pi`.
- Second: ensure the reset finishes before anyone starts adding the next round's contributions.
- The slides use separate arrival/departure counters and condition variables, plus a mutex for `pi`.



07_pi_cond_broadcast.c:

- Early arrivals add their local sums, increment the arrival counter, and sleep.
- The final arrival adds its contribution and gets the completed sum.
- It prints the result, resets `pi` and the arrival counter, then broadcasts.
- It unlocks so awakened threads can continue.
- Only one mutex and one condition variable are needed in this design.
- No second barrier is needed because the final arrival completes the reset before releasing everyone.


### Built-in barrier

08_pi_barrier.c:

- Uses `pthread_barrier_t`.
- Initialize with `pthread_barrier_init(&barrier, NULL, n_threads)`.
- Wait with `pthread_barrier_wait(&barrier)`.
- Threads wait until `n_threads` have arrived.
- The barrier resets automatically for reuse.
- Destroy with `pthread_barrier_destroy(&barrier)`.
- This pi example uses two barrier waits: before and after processing/resetting the result.




# 20 - OpenMP introduction


Open multi-processing:
- Unit of parallelism is threads.
- Used for scientific number crunching.
- First released in 1997.
- Not to be confused with OpenMPI.
- More covenient way to use pthreads, and often implemented with ptreads.
- Its core is a set of language extensions that must be supported by the compiler.
- It also has a runtime library of functions that you can call to inspect the state of what the compiler has generated.

Language extentions:
- Phtreads code is repetitive:
	- Declare, initialize, use, and destroy a mutex for every thing that needs protection. 
	- Declare, initialize, use, and destroy a cond for every signal.
	- Declare, initialize, use, and destroy an object for every barrier.
	- Simple sets of operations make for lots of repetitive typing.
- Can make the compiler generate the repetitive code, using a tiny languiage embedded in \#pragma directives. Used in OpenMP

Basic block:
```C
int main () {
	int a = 42, b = 32, c = 0;
	{
		int a = 64;
		c = a – b;
	}
	printf ( “a = %d, b = %d, c = %d\n”, a, b, c );
	return 0;
}
// will print "a = 42, b = 32, c = 32"
```


Using openMP:
- `-fopenmp` compile flag.
- Number of threads; OS recognized corecount or environment variable "OMP_NUM_THREADS". Or use `#pragma omp parallel num_threads(4)`
- `#include <omp.h>`

Thread count and thread-id:
- `int tid = omp_get_thread_num();`
- `int n_threads = omp_get_num_threads();`


Mutex:
- `omp_lock_t lock;` called outside of functions (global).
- `omp_init_lock ( &lock );` called before parallel segment.
- `omp_set_lock ( &lock );` called before critial code.
- `omp_unset_lock ( &lock );` called after critial code.
- `omp_destroy_lock ( &lock );` called after parallel section.


Barrier:
- `#pragma omp barrier`  Wait until every entry is ready


Other common OpenMP directives:
- `#pragma omp parallel` parallel basic block.
- `#pragma omp for` divides for loop iterations among threads.
- `#pragma omp parallel for` Combines the above: starts a parallel region and divides the loop among its threads.
- `#pragma omp single` One thread executes the block; the others skip it.
- `#pragma omp critical` Allows only one thread at a time into the protected block. Every thread that reaches it gets a turn.
- `#pragma omp atomic` Protects a supported operation on one memory location, such as counter++.





Condition variables: 
- No direct equivalent of pthread_cond_t for thread signaling.

Performance: 
- Keep critical sections short; busy waiting and using more threads than cores can hurt performance.

Correctness: 
- Easier than pthreads, but you must still handle shared/private variables and choose safe parallel work.

Examples: 
- Manually using thread IDs and counts demonstrates the connection to pthreads; typical OpenMP code uses higher-level constructs.

Why learn pthreads: 
- Helps you understand what OpenMP handles behind the scenes.




# 21 - Atomicity in OpenMP


![[{251191C3-6CBF-4952-8C5D-752E7C700864}.png]]

History of shared memory:
- Dancehall architectures: 
	- 1 core per chip, no cache memory and comparable clock rates for cpu and memory bus. 
	- This gave use a architecture of multiple cpus and memory banks connected. All memory was shared by every CPU, Uniform memory access (UMA), therefore any cpu can contact any other cpu. 
	- Another name is "Symmetric Multiprocessor" (SMP). Came with race condition.
- The interconnect fabric+cpu design therefore supported atomic operations such as (fetch-and-phi): 
	- Test-and-set : sets flag to 1, and returns previous
	- Fetch-and-increment : returns x, and does x+1
	- Fetch-and-add : returns x, and does x+n
	- Compare-and-swap : changes x to new if x == value
- 21st century: As the memory wall emerged, the memory hierarchy and therefore NUMA became common. SMP went from meaning "Symmetric Multiprocessor" to "Shared memory processor". Modern computers are multicore and therefore SMP. Cache-coherent NUMA (ccNUMA); cache attempts to bridge the memory performance gap, but only works for 1 processor at a time.


Alternate atomic solutions 1:
- If more than one copy in seperate CPU caches, invalidate the ones you arent targeted, and then lock access in the memory bank with value in it.
- Used in Intel family and its compatible competitors (CISC).

Alternate atomic solutions 2:
- Load linked: LL is instruction that will fetch value to register, and tags the memory bank. Can then be manipulated with entire instruction set.
- Store conditional: Attempts to write back to memory bank, and returns a fail if someone else altered it in the mean time.
- Comes from MIPS line of processor designs, more RISC way of using explicit load/store.

Alternate solution 3 (atomic reservoir memory):
- CPUs access it directly, bypassing their caches, so there are no stale cached copies.
- The special hardware provides atomic-access support.
- Programs use special allocation functions to put variables there.
- Trade-off: accesses are slower because caching is bypassed.
- comes from the Stanford DASH line of SMP

Atomics in x86_64 assembly:
- CISC style: large set of complicated operations and multiple addressing modes, some instructions require multiple CPU cycles.
- Atomic ops in x86 using "lock":
	- lock incq (%rax) ← Atomically increment nr. at (%rax)
	- lock addq $14,(%rax) ← Atomically add 14 to nr. at (%rax)
	- lock xchg %rbx,(%rax) ← Atomically swap %rbx for (%rax
- Therefore solution #1 of the variants. Grans exclusive access to either cache line, or the whole memory bus if necessary.

Atomic ops in GCC:
- GCC has built in functions that mirror fetch-and-phi ops, that arent directly part of c/c++ or other language:
	- \__atomic_test_and_set
	- \__atomic_fetch_add
	- \__atomic_compare_exchange
	- ...and so on
- Used in C++ atomics, and maybe supported by C.


Difficult part:
- Atomics are not standardized at CPU architecture/assembly level, and will differ.
- Atomics are not standardized at OS compiler level either; Functions of gcc are just gcc particular. 

Atomicity in OpenMP:
- For portability, openMP assumes your architecture and compiler supports some range of atomic instructions that can use these statements:
- `#pragma omp atomic` for single line operation.
- `#pragma omp critcal` for multiple lines. 
- Makes it so we dont have to use lock.
- After using `#pragma omp parallel` , a  collective of threads is called a "team", and the spawining thread is called a teams "master". `#pragma omp master`. Need to use barrier if other threads should wait after (difference from `#pragma omp single`).
- `omp_get_max_threads();` return max number of possible threads outside of parallel region, `omp_get_num_threads();` returns how many threads in team.
![[{A212FC87-B4F9-4A19-B3F0-40A3AAA35FCF}.png]]

# 22 - OpenMP worksharing directives


Partitioning work:
- Can be done manually based of index:
- `int tid = omp_get_thread_num();` to get id.
- `int n_threads = omp_get_num_threads();` to get team number.
- `for ( int x=tid; x<x_max; x+=n_threads )` round robin.
- `for ( int x=bottom[tid]; x<top[tid]; x++ )` consecutive partition.
- A bit of a hassle, can use OpenMP directives instead.

Decomposition modes:
- Functional: Divide work into subtasks.
- Data: Dividide domain of work into subtask.



Sections:
- Used for functional decomposition.
- Implicit barrier at the end of `sections`.
- Can also use `#pragma omp parallel sections`

```c
#pragma omp parallel
{
	#pragma omp sections
	{
		#pragma omp section
		{ /* Section #1 */ }
		
		#pragma omp section
		{ /* Section #2 */ }
		
		#pragma omp section
		{ /* Section #3 */ }
		
	}
}
```


Clauses:
- Optional set of terms that can control details of directives.
- Eg. `num_threads(4)` for parallel directive, and `nowait` for all worksharing directives.
- `private(x, tid, ...)`: Each thread gets own value of x even if x is defined in the outer scope.
- `shared(y, pi_array, sum, ...)`: Threads get same value for y, but still need to use atomic.
- `firstprivate(x)`: same as private but inherets startvalue from outer scope.
- `reduction(+:pi, ...)`: gives each thread private pi but performs operation in the end. In this case plus.
- `lastprivate(x)`: Variable receives the last logical iteration’s value.

Loops:
- Can use `#pragma omp parallel for` , or multiple `#pramga omp for` inside a `#pragma parallel` when you have a loop with a induction variable (i/j).

Single:
- `#pragma omp single`
- Comes with a barrier as it is a worksharing directive.
- Only one thread will execute, usually first arriving.

# 23 - Loopscheduling, threads vs. caches


Schedule:
- clause that the for directive can use to direct how to partition the loop/iteration. `schedule(kind, blocksize)`
- Blocksize is optional, but decides the minimum amount of iterations to distribute to a thread at a time. Too small blocksize causes unecessary scheduling but greater flexibility, large blocksizes allows for more efficient scheduling/distribution but can limit parallelism if too large.
- Kind:
	- Automatic: Default, lets compiler/runtime choose a way to distribute. Good for nested loops / multidim arrays.
	- Runtime: Will search for shells environment variable "OMP_SCHEDULE" to choose what to use eg. "static, 4", "dynamic, 16" or "guided, 8".
	- Static: Predetermined partitioning, eg. roundrobin with chosen blocksize.
	- Dynamic: The next available thread gets next available work. Good pattern for load balancing the work, for web servers, transactional databases and other systems where there is an "infinite" stream of work. Can also be used with finiste streams if wanting load balancing.
	- Guided: Starts with big blocks abd gradually shrink down to given block size, to attempt making the different threads reach the end barrier at similar times.


Timing in OMP:
- `double start/end = omp_get_wtime(void);`
- Works like mpi timing.



Experiment with advection equation:
- Three loops: time iteration, y axis (rows) then x axis(columns).
- Use omp for the y-axis: 
- Using omp for the x-axis: Slows down quite a bit due to repeated starting and stopping the parallel region each time we move down a row. Moving the `#pragma omp parallel` to before the y-axis loop helps as then we are not repeatedly starting and stopping the team. Adding `nowait` helps even more as then faster threads can move onto the next row. The barrier at the end of the parallel region still makes sure all calculations are complete before arrays are swapped for next time step.
- Using `#pragma omp parallel for` before the time iteration loop doesnt work as they are dependent on each other. 
- False sharing: 
	- schedule(static, 1) assigns one iteration at a time, round-robin. With two threads:
	- Several adjacent elements fit in one cache line.
	- A and B write different elements, but need ownership of the same cache line.
	- Ownership repeatedly moves between cores, causing delays. This is false sharing. Delays are almost equal to full cache miss.
	- The calculation can remain correct but slow. Larger contiguous blocks usually reduce the problem


# 24 - Cache coherence


Cache recap:
- Write-through: immediatly write back to memory from cache after update. Simple, inexpensive but creates memory traffic.
- Write-back: Delay operations in memory, and continure working with the cached copy. More complex implementation. Must mark dirty bit for modified lines. 
- Both work fine when dealing with one cpu/cache hierarchy.
- False sharing can happen with either policy when multiple cores: it concerns multiple cores writing different elements of the same cache line. Write-back does not eliminate the need to coordinate ownership.


Cache coherence:
- The issue of keeping multiple cpu caches up to date with each others modified values.
- Solution falls into two categories:
	- Snooping: CPU listens to each others memory traffic via shared branches of the interconnect. Issue with snooping: cost of broadcast operations can be costly if it is a large multilevel structure, and many participants. 
	- Directory: Maintaining a centralized registry of cache lines and the various states they are in. 


Snooping with write-through:
- CPU2 listens to the interconnect for changes to relevant cache lines, and copies the changes from the interconnect traffic, or when needed from memory depending on implementation protocol.

Snooping with write-back:
- CPU2 listens to dirty-bit broadcasts from CPU1, and either waits until CPU1 writes back, or requests the updated values from CPU1.


Directory:
- Table to track cache lines.
- Needs: entry for each memory block we want to track, a few bits to record its state (exclusive or shared, modified, uncached), and a bit vector with a bit for each processor.
- Directory requires memory: Either fixed part of general system RAM or additional memory bank.
- Coarse bit vector format: Can divide the cpus into n groups, then we need n bits for modified copies instead of cpuCount bits. Then use a seperate mechanism to figure out the coherence in the n group. 
![[{2CDA8F22-FC07-49D9-AC74-0A2737BB4088}.png]]












# 25 - Cache optimizations: Loop tiling and vector operations

### Loop tiling
- Loop tiling changes the order of the calculations so small blocks of data are reused while they are still in cache. The mathematical result stays the same.

Matrix calculation:
- For large matrices, data may be evicted from cache before it is needed again. Reloading it costs time.
```c
// C is initially zero.
for (int i = 0; i < N; i++)
    for (int j = 0; j < N; j++)
        for (int k = 0; k < N; k++)
            C[i][j] += A[i][k] * B[k][j];
```

Loop tiling for matrix caluulation:
- Use the naive algorithm for smaller parts of the matrix.
- This should be great for cache, as long as the tiles fit
	- Stage 1: All sections in matrix C receive calculation Ax0 column \* B0x row
	- Stage 2: All sections in matrix C receive calculation Ax1 column \* B1x row.
	- Stage 3: All sections in matrix C receive calculation Ax2 column \* B2x row.
![[{4420F1C0-F4A1-485D-A311-166EA0C63D2E}.png]]![[{ED6234A4-B14F-47A6-9229-F686A2B72D30}.png]]![[{3FA40D42-6401-4D0F-A880-9C716A717FA0}.png]]
### Vectorization


Vector registers:
- SIMD (Same instruction multiple data).
- Vector registers can hold more than 1 value at a time, and the cpu can apply the same operation to all of the values.
- Vector registers get a little architecture specific, so they are really best left to the compiler to generate. 
- It can fail or be inconistent, so possible to program  Intel vector registers by hand.
- Could program directly in assembly, but also supported by intrinsics in higher level languages. Intrinsics are special functions the compiler recognizes and translates into particular machine operations.




SSE2:
- Set of SIMD instructions supported by x86.
```
#include <emmintrin.h>
```
- `__m128d` holds **two doubles** in 128 bits.
- Aligned loads/stores require **16-byte-aligned addresses**.
- Allocate with `_mm_malloc(size, 16)`; release with `_mm_free()`.

Common intrinsics:
- `_mm_load_pd(ptr)` → load two doubles.
- `_mm_load_pd1(ptr)` → duplicate one double.
- `_mm_add_pd(a,b)` → elementwise addition.
- `_mm_mul_pd(a,b)` → elementwise multiplication.
- `_mm_store_pd(ptr,v)` → store two doubles.

Matrix multiplication:
- Vectorize **2×2 tiles** by calculating two adjacent output elements together.
- Duplicate an `A` value and multiply it by two neighboring `B` values.
![[{7954A049-EFDC-48ED-8722-478BE04864A6}.png|502]]
Important conclusions:
- Tiling improves data reuse; vectorization processes multiple values per instruction.
- Tiling gave the larger improvement in the lecture example.
- Floating-point results may differ slightly; compare using a tolerance.
- Multiple tiling levels can target different cache levels.
- Optimized libraries such as OpenBLAS and MKL implement these techniques.









# 26 - OpenMP tasks


Task-based programming:
- Alternative view of how to express parallelism.
- Nested parallelism inside two for loops or hidden with a function call is invalid.
- Task-based programs can be displayed with dependenciy graphs.
- can use `#pragma omp task { task (); }`
- can also use `#pragma omp taskwait` to wait for all spawned tasks to finish.
- You can also make tasks out of things without having a parallel region, but they will just be added to a list and run in sequence. 
- When there is a live team of threads active, they’ll pick up tasks from the task graph in parallel.
- A task can create more tasks; and the arguments/return values can infer dependencies. Can also use taskwait directives.
- Common pattern / use case: problems where the parallelizable work comes out of each nesting level in a recursive function call, like quicksort.
```c
#pragma omp parallel
{
	#pragma omp single
	start_the_circus(); // one thread starts the recursive function, which will create enough parallel work/tasks
}
```

```c
// calling a function into a task
#pragma omp task
some_useful_function ( arg1, arg2, arg3 );

// can also define the function with omp task and then it will be ran as a task
#pragma omp task
void some_useful_function ( int arg1, int arg2, int arg3) { … }
```
![[{B2691313-59CF-42B6-8AB5-92A84038D660}.png]]
```c
#pragma omp parallel
{
    #pragma omp single
    {
        for (int start = 0; start < 1200; start += 100) {
            #pragma omp task firstprivate(start)
            {
                // Ordinary function with a sequential inner loop.
                do_something(data + start, 100);
            }
        }

        #pragma omp taskwait
    }
}
```



Quicksort example:
```c++
mot_qsort( int64_t *buffer, int64_t lo, int64_t hi ){
// ...

#pragma omp task
mot_qsort(buffer, lo, split);

#pragma omp task
mot_qsort(buffer, split + 1, hi);

// ...

}

#pragma omp parallel
{
    #pragma omp single
    mot_qsort(buffer, 0, count - 1);
}
```







# 27 - Roofline analysis

![[{7913F028-31BE-4F05-90E6-3B7088719170}.png]]
![[{08D1E3E2-D327-4A72-BDD4-3A740A776839}.png]]

Amdahls law and speedup:
- Amdahls law: T(n) = T_s + T_p / n
- Speedup: S(n) = T_1 / T_n = (T_s + T_p) / (T_s + T_p/n) 

Gustafsons law and scaled speedup:
- Gustafson’s law: \(T_1 = T_s + nT_p\)
- Scaled speedup: \(S = T_1/T_n = (T_s + nT_p)/(T_s + T_p)\)

Hockneys communication model:
- T(m) = α + βm = α + m/B
- Where m is message size, α is fixed startup latency per message, β (seconds/byte) is transfertime per byte, and B is 1/ β.
- ignores the fact that message latency is affected by the communication library call, the operating system, the microcontroller in the network interface, the condition of the network cable. Also ignores existence of other message on the same network capacity.


Popular processor metrics (not necessarly useful):
- MIPS (Millions of Instructions Per Second).
- FLOPS (floating-point operations per second).
- GHz / MHz: would only be useful if program speeds were proportional to clock rate.
- Core-count, also different types of cores on the same chip.

Problems with benchmarks:
- Only really measuring the performance and speed of that particular benchmark.
- In order to estimate how fast your particular program can run on a given computer, it’s helpful to analyze what kind of work the code does most of.

Data movement and opertions:
- A programs work can be usefully divided into:
	- The parts that move numbers in and out of the processor (load store).
	- The parts that combine numbers in the processors (operations).


Roofline model:
- Performance memory = Memory Bandwidth \* Arithmetic intensity (I)
- \[Flops / second] = \[Bytes / second] \* \[Flops / byte]
- I_ridge = Performance Limit / Memory bandwidth
![[{1FFAC0AA-F9AB-4882-96C2-999F4640AC7A}.png|473]]
![[{A33974AC-8373-4E4F-844E-01D401EC566F}.png]]






# 28 - Application types


7 computation classes (Seven dwarves of Berkeley From the berkely report):
- Dense linear algebra.
- Sparse linear algebra.
- Structured grids.
- Unstructured grids.
- N-Body problems.
- Monte carlo methods.
- Spectral methods.

Usefulness:
- Using a program of type n-dwarf on a computer X, we know whether similar applications can be adapted well for the computer.

Dense linear algebra:
- Typically large/repeated matrix/vector calculations, where tthe operator often causes many instructions per element, such as matric multiplication (dot product).
- Use cases: Quantum mechanics, Eigenvalue problems, data analytics (dimensionality reduction).
![[{C3F048AC-9986-4874-8352-486DA7B4166B}.png]]
Sparse linear algebra:
- Similar to dense linear algebra, however many of the elements in matri are 0. 
- This makes it more useful to turn it into a list of non-zero indices (CSR: Compressed Sparse Row).
- More likely to be memory bound as operations have less work available per element.
- Use cases: Physics of everyday things, inhomogeneous systems of linear equations, search engines, social networks, machine learning.
![[{7DD8562F-303D-4859-A165-3F039EB998FB}.png]]

Structured grids:
- Similar to the advection equation problems.
- Dividing the problem into equally sized pieces, where the near neighbour points affect eachother.
- Data structures typically 2D, 3D, 4D, ... arrays.
- Application areas and performance characteristics are similar to sparse linear algebra.

Unstructured grids:
- Same as structured grids just without the assumption that the neighbours are evenly spaced.

N-body problems:
- Problems consist of a list of coordinates for things that push eachother around. 
- If every obdy can affect eachother, we get N\*(N-1) / 2 pairs


Monte carlo methods:
- Calculations that approach a solution by accumulating random numbers.
- Dice, pi approximation and so on.
- Well suited for parallel computation as each random iteration is independent.
- Challenges: Hard to source random numbers.

Spectral methods:
- Most popularly used for the Fast Fourier Transforms (FFT), also laplace and wavelet.
- Our familiar difference/volume/element methods develop values in neighbor points through combining them incrementally:
![[{308ACB6A-4C4E-4B0C-AED1-1BB365AF5C51}.png]]
![[{217035F4-4E5D-4DAA-8891-B0B7C7F6C883}.png]]


# 29 - Loose ends

### Simultaneous MultiThreading (SMT)

- Lets one physical CPU core execute instructions from multiple threads during the same clock cycle.
- Each thread has its own architectural state, such as its instruction pointer and registers, while sharing the core’s execution resources.
- The operating system sees each hardware thread as a logical processor. With two-way SMT, 4 physical cores expose 8 logical processors.
- The goal is to fill unused execution capacity. If one thread cannot use all available resources, instructions from another thread can use the remaining capacity.
- SMT can also keep a core busy when one thread stalls, provided the other thread has ready instructions and available resources.


### Superlinear speedup

- When S(p) > p happens, we call it superlinear Speedup.
- We expect S(p) = p at best, because of overhead.
- This happens, when more of the total domain fits into lower level memory in the hierarchy, such that operations involving load/store become faster more often. When using p processes in n cores, we get n times the amount of cache L1. 
![[{60AEE2F1-7BC2-455E-80B6-738BA0416B1A}.png]]
### Load balancing

- Strategies to mitigate unbalanced workloads:
	- Static: Embed the partitioning of the problem directly in the source code.
	- Semi-static: Examine the workload when the program starts, divide it then, and run with the inital partitioning until finished. 
	- Dynamic: Adapt to the workload by shifting work around between participants during program execution.

Recursive orthogonal bisection (Semi-static):
- Start by finding 50% mark of cells with work/relevance along one axis.
- Repeat until you have the number of equal weighted sections you want (2^n sections where n is the number of iterations you split the domain in half).

Master/worker pattern (dynamic technique):
- One thread becomes the master, rest of threads are workers.
- Then the master either assigns threads tasks or they take tasks from a queue, and inform the master. 

Work stealing (dynamic technique):
- Similar to master/worker.
- Each participant has own queue of tasks it has been assigned.
- Other unemployed workes can reassign or take tasks from neighbours.
- Each participant is both a "master" and a worker to its close neighbours.
- Pro: Scales well. Cons: If the work is very unbalanced, it can take a while to even out. 


### Hybrid programming

- Combining multiple parallel programming models.
- For example using MPI to communicate between several multicore SMP systems. Then we can have several cores either running pthreads or OpenMP. Inside each SMP system we can also have one or more GPUs running CUDA kernels.

Example:
- 4 nodes with 2 CPU sockets with 8 cores on each can run:
	- 64 MPI ranks.
	- 4 MPI ranks with 16 threads in each rank.
	- 8 MPI ranks with 8 threads in each rank.
	- 32 MPI ranks with 2 threads in each....
- Best combination depends on how the program uses memory.
- When using threads inside a MPI rank. you can:
	- Make 1 thread handle communication (usually the best choice).
	- Let all threads make MPI calls, even though some MPI calls arent thread-safe.
	- Let all the threads use MPI, but enforce mutual exclusion with locks.

Multiple GPUs:
- Communication:
	- 1 process that controls all GPUs-
	- 1 process per GPU, and then each process talk via MPI.
	- With the right kind of GPU you can get them to talk without involving hosting processes.
	- With the right kind of MPI, you can send and recv messages directly in device memory.
















 

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
- 2048 threads per sm, 32  registers per thread.  16 threads per core, 16 warps per partition.
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

## Performance pitfalls:

### Memory 

Memory (coalesced memory reads):
- Cache lines in GPU: usually 128 bytes per line (32 threads 4 bytes each).
- Memory allocated with cudaMalloc() is always aligned so byte 0 is the start of a cache line.
- If needing a single byte from VRAM, the 128 byte region is loaded into L2/L1 cache.
- Coalesced memory reads: Threads read sequential memory so that we require less fetched cache lines, better bandwidth utilization and more optimized cache use. 


Good cache use:
![[{EE2B55A2-FE68-48BC-80E1-F5421947CDFD}.png]]
Bad cache use:
![[{8B401431-3094-430B-ACEE-1500018A9BA9}.png]]

Example: 
- Answer is 2 - 1 - 2
![[{B4D96804-DE0B-4A30-8F44-22BA44CCE52A}.png]]

Memory (atomic write contention):
- When multiple threads are working with the same atomic variable, their operations become serialized. 
- Solution: Do seperate calculations then a single thread do the atomic operation (reduction).


Struct of arrays:
- Instead of storing each struct sequentially in memory in an array, store an array of each struct property, for better read sequences.

![[{2B6545ED-F97A-4647-ACCD-D13E568F29EC}.png]]
![[{F0886535-9A2F-4F69-9D7A-2F5CC257E6AF}.png]]

Shared memory:
- User managed L1 cache.
- 100x faster than VRAM.
- Can be used for inter-block communication.
- Use for: Storing data you will reuse many times, and exchanging values between warps in a block.

![[{097DB1C4-DE38-44BA-B985-D625B88F8253}.png]]



Memory tips:
- Vectorized loads: If struct/datatype is exactly 4, 8 or 16 bytes, the memory system guarantees your data is loaded in one operation. So if your struct is 4 floats and you need all 4 properties per struct, you dont need struct of arrays.
- Using smaller data types: Less data to through the memory system reduces load on it, eg. short instead of int.




### Suboptimal launch parameters
- If you have to few blocks some SMs get no work.
- Blocks that are too large can make register, thread limits or shared-memory limits prevent enough blocks to fit on sm. Can leave thread slots unused.
- Blocks that are too small (less than 32 threads, or not multiple of 32), can make a lot of unactive threads/lanes. Can also hit the max blocks/SM residence limit before the threads in SM are full.


### Thread divergence
- This happens when threads inside the same warp take different execution paths, such as loops or if statements. 
- This leads to a lot of waiting / inactive threads as they have to follow the same instruction path. 
- Solution: Try to arrange warps so that each thread in a warp perform the same work/path.
### Register spilling 
- Temporarily write som register values to memory.
- Upside: More threads can run simultaneously.
- Downside: More memory transactions.
- The compiler will spill registers when it believes it will improve overall performance.
- How to resolve: Consider recomputing values if doing so is cheaper than memory fetch. Also can cut fields from structs that are not realted to the kernel.

### PCIe bandwidth
- PCIe is bus for moving data between RAM and VRAM (cpu / gpu).
- Very slow compared to other memory buses.
- Avoid passing to much data back and forth.
![[{5A5260CF-6DD2-409D-8B6B-E8CF4900E8F3}.png]]


### Misc performance pitfalls:
- Dont use double (8 bytes), use float (4 bytes) instead.
- All double operations are emulated in software on most cards, only supported in hardware on enterprise GPUs.


Profiling
- Measure how the GPU kernels run to track occupancy, memory access, divergence, spilling and more.
- Can use NVIDIA Nsight systems to track application timeline, and NVIDIA Nsight compute to track induvidual kernels.



# 34 - Shuffle shenanigans part 2

Collective instructions.

### Shuffle 

Shuffle shenanigans:
- Exchanges of values within a single warp.
- Each thread provides one value.
- Each thread reads a value provied by one of the threads.
- Threads must be in same block.
- They must also be in same warp or cooperative group with max 32 threads.
- Cheap instruction (1 cycle).

```c++
T __shfl_sync(unsigned mask, T var, int srcLane);

// mask is 32 bit hex mask of what lanes participating. Eg. 0xFFFFFFFF or FULL_MASK for all threads.

// var is the calling threads contribution.

// which lanes contribution this thread wants


// --- Four variants ---

//Read from any thread (index specified by srcLane):
T __shfl_sync(unsigned mask, T var, int srcLane);

//Read from thread (laneid – delta):
T __shfl_up_sync(unsigned mask, T var, unsigned int delta);

//Read from thread (laneid + delta):
T __shfl_down_sync(unsigned mask, T var, unsigned int delta);

//Read from thread (laneid XOR laneMask):
T __shfl_xor_sync(unsigned mask, T var, int laneMask);

```

Shuffle instructions with lanemask = 16, 8, 4, 2 ,1:
![[{0E041D4D-C1EA-494C-AF7F-A81D268A76FB} 1.png]]
![[{3AE9BA68-7FEF-45C0-B58D-F72D4B1AE220}.png]]
Sum off all threads value in a warp, sum in each thread:
![[{610B516A-7923-405B-9219-ACC8CA062275}.png]]

Barriers:
- `__syncwarp()` syncs threads within one warp.
- `__syncthreads()` sync all threads in one block.


Shuffle misc:
- If the mask says a thread will call that shuffle, it must call it either in same or different branch.
- If a thread executes shuffle but not in the mask, the results are undefined.



### Shuffle applications

Warp-level reductions:

Broadcasting:
![[{BBA94FBD-0B90-416B-B249-EB4348A93462}.png]]
Bandwidth saving:
![[{9C9C01E3-C12C-4631-BAEC-4BBA7C90AB1C}.png]]
![[{B468F9CC-0BD1-4DED-AE12-68BB7E0D3F76}.png]]

Broadcast + bandwidth saving:
![[{F6BC8559-B3DB-4D5C-8610-51A892A7B1C5}.png]] 


### Warp voting

![[{0D6C8522-4FC6-478F-9CA0-78A53726B152}.png]]

### Warp voting example use cases 
![[{F5C15E0F-A27F-427B-BE7C-5A181AD974B1}.png]]
![[{F6AE56C8-98DC-4EB2-921C-5030446C76E5}.png]]
### Warp reductions
- Reduction support in hardware, added in RTX 3000 series cards.
- Integers only, if float use XOR reduction mode from before.

![[{41FCA0D1-04F8-4BE7-B8D8-24D56904C354}.png]]

### Thread cooperation:
![[{987BE944-18B0-48CA-BAB4-3AF71DFECF33}.png]]

### Cuda built in types:

- Also supports 16-bit float, half precision.
```c++
#include <cuda_fp16.h>

__half x = __float2half(1.5f); // Convert float to half
float y = __half2float(x);    // Convert back to float

// Pack two floats into each half2.
__half2 a = __floats2half2_rn(1.0f, 2.0f);
__half2 b = __floats2half2_rn(10.0f, 20.0f);

// Add corresponding components.
__half2 c = __hadd2(a, b);

// Convert the result to two ordinary floats.
float2 result = __half22float2(c);

// result.x == 11.0f
// result.y == 22.0f
```
![[{FDE73759-6D2F-4B80-9641-5387BE5C2839}.png]]
```c++
float3 position = make_float3(1.5f, 2.0f, 7.0f);

position.x;  // 1.5
position.y;  // 2.0
position.z;  // 7.0
```


### Misc Functions:
- Either very efficient of hardware implemented.
```c++
// Compute the sum of absolute differences: |x – y| + z
unsigned int __sad(int x, int y, int z);


// Compute the average of two integers
// Guaranteed to not overflow
unsigned int __uhadd(unsigned int x, unsigned int y);

// --float--

// The remainder of a/b
float remainder(float a, float b);

// Computes sqrt(a*a + b*b + c*c)
float norm3df(float a, float b, float c);

// Calculate a x y + z
// Useful because it only rounds at the end
float ___fmaf_rz(float a, float y, float z);



// --- bit ops ---

// Find the index with the first bit set to 1
int __ffs(int x);


// Count leading zeroes (starting from the least significant bit)
int __clz(int x);


// Find the position of the {offset}th bit set to 1
// Counting starts at index base
unsigned int __fns(unsigned int x, unsigned int base, int offset);

```