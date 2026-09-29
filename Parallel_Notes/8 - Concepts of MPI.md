
Message passing interface (MPI):
- Standard specifiaction for a large number of function calls, and what they are supposed to do.
- Been around since 1994, implementations come and go.

MPI how:
- Parallel processes.
- Makes copies of the program who are started through a launcher program, and tracks how to makes connections through them. Started with mpirun, mpi_exec or similar.
- Sending data from process to another gives copy. Code have to take care of consistency.

MPI is SPMD:
- Single Program multiple data, programming style not type of machine.
- P copies can do P different things due to idendity number seperating (rank).

Communication:
- Two sided, one side have to send and the other side waiting to recieve.
- Sends that dont have recieve will stop or crash the program, recieves that dont have a send will stop the program.
- Each rank starts as a member of a "communicator".
- World communicator: contains every process launched. Size is between 0 to p-1.
- Can divide world communicator into subgroups.
- Sub groups have own ranks and world size.
- Can make communicators that have structure, such as cartesion coordinates (x,y).

Communcation modes:
- Standard: Whatever your implementation has as default.
- Synchronized: Send-function will not return until reception is acknowledged.
- Buffered: Explicitly manage the memory that’s used for sending/receiving.
- Ready: Assume that the receiver has already initiated the receive.

Non-blocking communication:
- Usually send/recieve operations only resumes the program after the message comes through.
- Non-blocking sending and receiving immediately returns a request instead, so that you can continue calculating.
- In order to make sure that the message has gone/come through, you must issue a wait-for-completion call for the request later on, for example when you need the comms being complete.

Collective operations:
- Mpi offers set of operations every rank in a communcator must call before it completes.
- Such as: Broadcasting values, finding a maximum or minimum, global sum and so on.
- These dont require seperate branching. 

Scattering and gathering:
- MPI has some collective operations dedicated to: 
	- Splitting some large data set into equal parts and distributing.
	- Recieving a number of equal parts and putting them back together into a large data set.

Synchronzation:
- Barrier mechanism that waits until all ranks in a communicator have reached it.
- Can be used to measure execution time of what comes next.


Derived datatypes:
- Mpi understands a handful of built-in data types.
- Such as: Integers, floats, characters.
- Allows you send data structures with different types of variables, dealing with 2d arrays and so on.



Parallel I/O:
- File-master: One process is a file-master, that collects all the pieces and organizes the file contents. Increases portion of sequential code.
- Parallel: MPI has features to let all ranks open same file but write to different parts of it. But, unless you have a custom OS installation that allows it, it will probably still be serialized by the OS.



Misc:
- One sided communcation is possible if reciever has a buffer that allows it.
- Recieving from "anywhere" is possible, useful for searching methods.
- There exists non-blocking collective operations.
- You can inject code that is run before/after every kind of MPI operation, so that you can instrument it without changing the source.


Design philosophy:
- It makes all its data movement explicit, so that you can find the bottlenecks directly in the source code.
- It’s easier to reason about ‘invisible’ data movement when you’ve handled it manually first.





