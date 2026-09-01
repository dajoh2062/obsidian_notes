### Cache and memory

Programming languages must (that is with memory and repetitions):
- Expressions: carry out some set of operations on given values. 
- Variables: give names to values, and recall them later. 
- Conditionals: do something only when an expression is valid. 
- Jumps: fetch next expression from a different place in the program.

Memory:
- Temporal locality: If data is accessed now, it is likely to be accessed again soon.
- Spatial locality: If data is accessed now, nearby memory locations are likely to be accessed soon.

![[{67CC496F-7BF9-44A3-9AFF-D489EF473A94}.png]]
CPU fetch sequence:
- CPU asks for array → cache miss → RAM loads array + nearby elements → cache → CPU

Cache:
- Wont save time on first fetch, but uses more bandwidth.
- Saves time when next fetch is from the same or neighboring locations.
- Each little neighborhood of values is called a cache line, there’s room for several in the cache.
- Bigger cache = slower cache.
- Fully associative cache: A fully associative cache lets any memory block be stored in any cache line, reducing placement conflicts but making lookup more complex.

![[{FCC877DB-1D97-4884-B1F2-C371465F4906}.png]]
Set associative cache:
- The number of sets are called "ways of associativity".
- Line lengths are powers of 2.
- Ways of associativity are also powers of 2.
- The number of lines in each set can be any nice integer, only affects how many tags we must search through.

### Hierarchical memory

L1:
- its close to the registers, there are often separate caches for instructions and data here.

L2:
- If something isn’t in L1 cache, a bigger L2 is searched (typically contains both instructions and data).
- may be private to one processor or shared between several.

L3:
- If something isn’t in L2 cache, L3 is searched.
- It’s typically shared by every core on a CPU socket.

L4:
- here may be an L4 cache which sits outside of the CPU chip.

### Virtual memory

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