

### Flynns taxonomy:

- Way to classify parallel computers. 
- Kind of old-fashioned.

Two things to deal with:
- Instructions (single or multiple).
- Data (single or multiple).

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
- Not really interestin g or useful.
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


