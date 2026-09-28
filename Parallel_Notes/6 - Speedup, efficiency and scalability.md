
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