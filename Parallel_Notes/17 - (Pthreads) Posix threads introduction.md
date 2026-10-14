![[Screenshot 2026-10-12 at 14.48.41.png]]

Stack:
- Stores local variables and function calls; memory is automatically managed.


Heap:
- Heap: Stores dynamically allocated memory (`malloc()`), which must be manually released (`free()`).

Data:
- Data segment: Stores global and static variables that exist throughout the program.
![[Screenshot 2026-10-12 at 14.59.16.png]]

Memory layout with one process with one thread:![[Screenshot 2026-10-12 at 15.09.30.png|652]]

Memory layout one process with multiple threads:
![[Screenshot 2026-10-12 at 15.23.14.png|626]]

Concept of concurrency:
- Can improve programs by relaxing the restriction that the statements must only be completed in one specific order.
- Examples: database transactions, computations, web pages served, image rendered.

Software that enables concurrency:
- Threads are a sotware construct, augmenting the program with information about which steps depend on fixed sequence, and which dont.
- The hardware and OS decide how to use that information. 
- For this course, we would like as much parallel operations as possible.

Thread and core count:
- Trivial mapping when thread and core count match.
- Oversubscription can be very useful, as we can keep the cores busy while some threads wait for IO or other resources. Instrumental to keeping GPUs efficiently.
- For strict computation threads, oversubscribing only leads to scheduling overhead.

Shared memory parallelism:
- Pricate work space: local variables in stack.
- Shared global works space: data and heap can be used for collaboration and communication.
- Can lead to conflicts and issues.

PRAM model:
- The PRAM model* is a minimialistic, abstract machine which proposes 4 candidate policies:
	- **Common Value** admits the assignment and allows execution to proceed if (and only if) all attempts to make it are trying to assign the same value.
	- **Arbitrary Value** assigns the value from one of the attempts, and the program must assume that it can be any of them. 
	- **Priority** requires each thread to have a ranking among its peers, and the highest ranked assignment wins.
	- **Reduction** assigns the result of applying some commutative operation to all the attempts (sum, product, logical AND, etc.).

Practically:
- Arbitrary value is the only policy that is commonly used.
- The value is simply determined by handling the assignments in the order they arrive at the memory banks.
- This makes the program act non-deterministically at best and crash in worst case.

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

