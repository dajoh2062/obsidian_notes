
The memory wall:
- Memory itself is too slow for the CPU, so have to try mitigate this.

Memory hierarchy goal:
- Hide latency of memory.
- Illusion of large capacity.
- Cost efficiency.

Hierarchy:
![[{C568EE9D-3DFE-4F02-B519-EF5087782F36}.png|517]]

Memory locality:
- The hierarchy exploits locality.
- Temporal locality: Accesses to same location is likely to occur close in time.
- Spatial locality: Accesses to nearby locations are likely to occur.
- Both for data and instructions.

Exploiting locality:
- Frequently used data and instructions are stored close to the processor.
- Either in registers (by compiler) or in cache L1/L2 (done by hardware).
- Rest is stored in memory or disk.

Memory layout:
![[{0D991A8D-4076-42B3-8FB5-35CA5433922A}.png]]

### Cache architecture:


What is cache:
- Number of equally sized cache locations.
- Each location holds a block size amount of data.
- 3 ways to organize locations:
	- Direct mapped.
	- set assosiative.
	- Fully assosiative.


Cache block:
- The memory address is made up of the block address and the block offset.
- Block address specifies which block in memory.
- Block offset specifies which bytes inside the block we are referencing.

![[{1D572269-6E78-40D6-A9D6-AD151B88E6CD}.png|618]]

Block size trade offs:
- Larger size gives better spatial locality.
- Too large blocks may result in a large part of the block is being unused.
- Larger blocks take longer to read into the cache though, which increases miss penalty.
- Also makes it fit less blocks.

Direct Mapped Cache:
- Each block address is mapped to exactly one cache location.
- index = blockAddress % blockCount
- Index: tells you which cache set to look in.
- Tag: identifies which memory block is stored there. Multiple memory blocks map to the same set, so the index alone isn’t enough.
- Offset: tells you which byte inside the block you want.

![[{20B3A054-5DAF-43D7-B7B2-0906A4AE9482}.png]]


Fully assosiative cache:
- No index, as each cache location must be searched. 
- Full block address must be stored as tag.
- Any block can be in any location.

N-Way Set assosiative cache:
- Each block address is mapped to exactly n cache locations.
- set_count = block_count / n
- index = block_addr mod set_count
- Less prone to location conflicts compared to direct mapped caches.
![[{576C184C-9A5C-4604-A7AB-ECF8D04BD1DC}.png]]

4-way assosiative cache:
![[{0F88DEB4-223A-4C85-A853-8D99BF7D01BF}.png]]

3C cache miss classification:
- Compulsary misses: misses that would occur even with a infinitly large cache, eg. first access.
- Capacity misses: misses that would occur even if the cache was fully assosiative.
- Conflict misses: are the misses that occur because the cache is not fully associative (i.e., a given block can only  be placed in n locations and n is not all blocks in the cache).


Seperate and unified caches:
- Seperate data and instuction L1 cache lets CPU fetch instruction and read data simultaniously.
- Else, unified caches most common.
- Seperate L1 cache also lets us: exploit knowledge about the data like instruction format, and exploit knowledge about accesses.

Write buffer:
- Writing to lower-level memory, such as L2, L3 or RAM—takes time because it is slower.
- Without a write buffer, the CPU may stall, meaning it pauses until the write finishes.
- A write buffer temporarily stores the data and its destination address, allowing the CPU to continue executing while the write completes in the background.
- A merging write buffer combines writes to consecutive memory locations into one larger transfer, which is usually more efficient than several small transfers.

### Cache design options


Cache design choices:
- Decide how to handle writes and when to allocate cache space.
- Decide which blocks to evict when space is needed.
- Decide whether blocks can appear in multiple cache levels.
- Decide how many cache misses can be handled simultaneously.

Write-back and write-through:
- Write-back: Update the cached copy. Write it to lower-level memory when the modified block is evicted.
- A dirty bit tracks whether a block has been modified.
- Write-back reduces writes to lower-level memory, but requires more complex hardware.
- Write-through: Every write is also sent to lower-level memory.
- Write-through is simpler, but uses more write bandwidth.

Write allocation:
- Determines what happens on a write miss.
- Write-allocate: Bring the block into the cache and update it there.
- No-write-allocate: Send the write to lower-level memory without bringing the block into this cache.
- Typical combinations are write-back + write-allocate and write-through + no-write-allocate.

Read allocation:
- Determines when a cache location is allocated for a missing block.
- Allocate-on-fill: Allocate the location when the requested block returns from lower-level memory.
- Existing blocks remain available while waiting, which may improve performance, but adds complexity.
- Allocate-on-miss: Reserve a cache location as soon as the miss occurs.

Replacement policy:
- When a new block needs space and all its possible locations are occupied, one existing block must be evicted.
- Random: Evict a randomly selected block. Simple to implement.
- FIFO: Evict the block inserted longest ago. Must track insertion order.
- LRU: Evict the least recently used block. Must track usage order on every access.
- LRU exploits temporal locality, but becomes expensive with high associativity.
- Pseudo-LRU: Approximates LRU with simpler hardware.

Inclusive and exclusive caches:
- Inclusive: Every block in a higher-level cache also exists in the next lower-level cache. For example, every L1 block is also in L2.
- Inclusivity helps track cached copies, but duplication reduces the total amount of unique data the hierarchy can hold.
- Maintaining inclusivity adds overhead, such as invalidating an L1 copy when its inclusive L2 copy is evicted.
- Exclusive: A block is kept in only one of the cache levels covered by the policy, allowing more unique data to fit.
- Another option is to enforce neither inclusion nor exclusion: copies may exist at multiple levels, but are not required.

Non-blocking caches:
- Allow cache hits to be serviced while an earlier miss is still being resolved.
- Can support multiple outstanding misses, overlapping their waiting times.
- This is called memory-level parallelism (MLP).
- Requests for different words in the same missing block can be merged into one block fetch.

MSHRs:
- Miss Status Holding Registers (MSHRs) track outstanding cache misses.
- Store the missing block’s address and information about the requests waiting for it.
- The number of MSHRs limits how many different missing blocks can be tracked simultaneously.
- When all MSHRs are occupied, a new miss requiring another entry must wait.
- Adjusting the available MSHRs can limit a core’s MLP and memory bandwidth consumption.

### Virtual memory

Virtual memory:
- Programs use virtual addresses, which are translated into physical addresses in RAM.
- Each process has its own virtual address space.
- The virtual address space can be much larger than the available physical memory.
- Only the currently needed portions must be present in RAM.

Address translation:
- Virtual memory is divided into fixed-size pages.
- Physical memory is divided into equally sized page frames.
- An address consists of a page number and a page offset.
- Translation changes the virtual page number into a physical frame number.
- The page offset stays unchanged because the byte’s position within the page stays the same.

Page tables:
- Store the mappings between virtual pages and physical frames.
- Forward page table: Look up a mapping using the virtual page number.
- Inverted page table: Has one entry per physical frame, recording which virtual page occupies it.

Translation lookaside buffer (TLB):
- A small hardware cache holding recently used address translations.
- Avoids looking up the page table in memory for every access.
- Instruction and data accesses can use separate I-TLBs and D-TLBs.
- TLB hit: The translation is available immediately from the TLB.
- TLB miss: Look up the translation in the page table, using hardware or software.
- A TLB miss does not necessarily mean a page fault: the page may already be in RAM.

Cache and TLB:
- In a straightforward physically addressed cache, the TLB first translates the virtual address.
- The resulting physical address supplies the cache’s tag, index and block offset.
- The cache then checks whether it contains the requested data.

Demand paging:
- Pages are brought into RAM when they are needed.
- Accessing a page that is not currently in RAM causes a page fault.
- The operating system handles the fault and loads the required page, potentially replacing another page.
- Disk access is slow, so the waiting process is paused while other processes can run.
- Once the page is available, the waiting process can resume.

Memory protection:
- Processes should not access or modify each other’s private memory.
- The same virtual address in two processes can map to different physical locations.
- Processes can intentionally share memory by mapping virtual pages to the same physical frames.
- Page permissions control allowed accesses, such as read-only or writable memory.

Virtually indexed, physically tagged cache (VIPT):
- Uses bits from the virtual address to select a cache set.
- Performs the cache lookup and TLB translation in parallel.
- Uses the translated physical tag to verify that the selected cache entry is the correct block.
- Typically takes the cache index from bits within the page offset, since these do not change during translation.
- Reduces access latency by overlapping cache lookup with address translation.

### Out-of-order loads and stores
Memory operation:
- Address calculation: Calculate the virtual address, often by adding a register value and an offset.
- Address translation: Translate the virtual address into a physical address using the TLB.
- Memory access: Read the value for a load, or arrange to write the value for a store.

Loads and stores in an out-of-order processor:
- A load needs the register values required to calculate its address.
- A store needs both its address and the value it will write.
- A load reads from the cache or memory and places the result in a register.
- A store first places its address and value in a store queue/buffer.
- Stores update the cache or memory later, once they are safe to make permanent.

Speculative execution:
- The processor can execute instructions before knowing whether their predicted execution path is correct.
- Speculative loads may bring data into the cache.
- A page fault from a speculative load is only acted on if that load belongs to the correct execution path.
- Speculative stores must not permanently change memory before they are confirmed as correct.
- Speculation can still change cache state, which can enable timing side-channel attacks.

Memory dependencies:
- RAW — read after write: A load needs a value produced by an earlier store.
- WAR — write after read: A later store must not overwrite a value before an earlier load reads it.
- WAW — write after write: Two stores to the same location must leave the value from the later store.
- In the processor organization described, in-order completion preserves WAR and WAW ordering.
- RAW is the main challenge for early loads: a load might read an old value before an earlier store has supplied the new one.

Why execute loads early?
- Loads often begin a chain of instructions that depend on their result.
- Executing a load earlier can allow all those dependent instructions to begin earlier.
- Memory dependencies are harder to identify than register dependencies because addresses must first be calculated.
- Two important techniques are load bypassing and load forwarding.

Load bypassing:
- Allows a load to execute before earlier stores when it does not depend on them.
- Example: an earlier store writes to address A, while a later load reads from a different address B.
- The load can proceed without waiting for the store.
- If an earlier store’s address is unknown, executing the load early requires speculation and a later correctness check.

Load forwarding:
- Supplies a load directly from an earlier store in the store queue/buffer.
- Example: a store writes A = 5, followed by a load from A.
- Even if the store has not reached the cache yet, the load can receive 5 directly from the buffered store.
- If several earlier stores write A, the load must receive the value from the most recent store before that load in program order.

Store queue and store buffer:
- Hold stores whose values have not yet been written to the cache or memory.
- In the slides, finished stores have calculated their addresses and values, but may still be speculative.
- A speculative store can be discarded if its execution path was incorrect.
- Completed stores are non-speculative and wait in the store buffer to be written to the cache or memory.
- Loads receive priority in the illustrated design because other instructions often depend on their results, while store delays can be buffered.

Searching the store queue/buffer:
- The newest value may be in a pending store rather than in the cache.
- A load therefore checks the store queue/buffer for a matching address.
- Uses associative comparisons, often implemented with content-addressable memory (CAM).
- Hardware must handle multiple matching stores and select the correct most recent older store.
- This makes the store queue/buffer complex.

Limitation of executing memory operations in order:
- Once all earlier stores have executed, their addresses and values are available for checking and forwarding.
- However, waiting for all earlier stores delays loads even when they access unrelated addresses.
- This limits instruction-level parallelism (ILP).

Problem with out-of-order loads:
- An earlier store may still be waiting to execute when a later load runs.
- Its value may not yet be in the store queue/buffer, and its address may still be unknown.
- The load could therefore read an outdated value from the cache.

Finished load buffer:
- Tracks loads that have executed but have not yet completed.
- Stores their addresses and loaded values.
- When an earlier store completes, its address is checked against younger loads in the buffer.
- If there is no matching address, those loads did not depend on that store.
- If a load ran too early and read the wrong value, it and the instructions after it must be discarded and re-executed.
- A load’s entry is removed when the load completes.

Memory dependence prediction:
- Predicts whether a load depends on an earlier store.
- Helps decide whether the load should wait or can execute early.
- Aims to preserve the performance benefit of early loads while reducing expensive re-execution.

