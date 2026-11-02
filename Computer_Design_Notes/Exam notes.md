
# 0 - Course introduction


What:
- Detailed discussion of simple pipelined CPUs.
- Introduction to advanced techniques such as:
	- Out of order execution.
	- Branch prediction.
	- Speculative execution.
- Instruction level parallelism - focused processor cores.

Curriculum:
- Exercise materials (8) .
- Lecture slides (8).
- Computer Architecture,A Quantitative Approach, 6th (or 7th) Edition.
	- Chapter 1
	- Chapter 2.1, 2.2  and 2.3
	- Chapter 3 (except 3.2, 3.7, 3.11 and 3.12)
	- Appendix A
	- Appendix B
	- Appendix C (except C.6)

# 1 - Trends and performance

Chapter 1: Fundamentals of Quantatative design and analysis

### The computer revolution:

Moores law:


### Classes of computers

Embedded computers:
- Components of systems.
- Restricted power/performance/cost constraints.
- Usually specific tasks and applications, and tailored to purpose.

Personal mobile devices (PMDs):
- Wireless devices. 
- Multimedia user interface such as cell phones, tablet computers.
- Stringent power/performance/cost constratins.

Desktop computers:
- General purpose.
- Variety of software.
- Cost/performance tradeoff.

Server computers:
- Network based.
- High capacity, throughput, reliability.
- Business applications.

Clusters/Warehouse - scale computers.
- Collections of desktop computers or servers connected by network to form a larger computer system.
- Price-performance oriented.

### Parallelism

Main way of improving computer performance.

Types of parallelism:
- Data-level parallelism (DLP); same operation on different data.
- Task-level parallelism (TLP); Divide work into tasks that can operate mostly in parallel.

Sources of parallelism:
- Instruction-level parallelism (ILP): Independent instructions that can be executed in parallel. Can support DLP and TLP.
- Vector processing / Single Instruction Multiple Data (SIMD): Single instruction being applied to multiple data elements at the same time. Supports mostly DLP.
- Thread-level Parallelism (TLP): Software organized as different threads that can operate mostly in parallel. Either communcicating to eachother or dividing a domain. Can support DLP and TLP.
- Request-level Parallelism: Parallel request-response server systems. Can handle many users with multiple requests. Supports mostly TLP.
- Memory-level Parallelism: Processor memory requests (loads and stores) can be issued in parallel to hide some of the latency. Supports either by overlapping independent memory accesses

Flynns Taxonomy:
- Way of categorizing computers.
- SISD: Single instruction stream, single data stream.
- SIMD: Single instructon stream, multiple data streams. Vector architectures, Graphic processor units (GPUs). 
- MISD: Multiple instruction streams, single data streams. No commercial implementation.
- MIMD: Multiple instruction streams, multiple data streams. Tightly coupled; multi-cores. Loosely coupled: Clusters, data senters.

### What is computer architecture

### Technology trends

### Principles of quantatative design



