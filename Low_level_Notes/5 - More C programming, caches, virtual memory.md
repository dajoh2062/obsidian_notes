
### Cache organiation

Direct-mapped: 
- A block can only be placed in one location in the cache.

Fully associative:
- A block can be placed anywhere in the cache.

n-way set associative:
- A block can be placed at one of the predetermined n locations in the cache.

![[{80115575-723A-44D4-8ED8-B0B2A9790422}.png]]

Block or line:
- Unit size of data stored in cache.
- Typically in the ranges of 32-128 bytes.

Set: 
- A group of blocks.
- Index bits select a set.

Way: 
- A block within a set.
- 2-32 is common.
- Fully assosiative: number of ways = number of blocks.


Cache replacement:
- Random.
- LRU(least recently used).
- FIFO.

### Writing to cache

Write-back:
- When block is evicted.
- Pro: Consolidate multiple writes to same block.
- Con: Need to track modified blocks (D bit 1/0).

Write-through:
- While writing to cache, write back to memory.
- Pro: Simpler implementation and coherence.
- Con: Slow and bandwidth intensive.



### Cache performance

Formula:
- AMAT (cycles per access) = (hit rate * hit time) + (miss rate * miss latency)

![[{968F8699-EED9-4C74-AEB6-F608C3EBC96E}.png]]

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