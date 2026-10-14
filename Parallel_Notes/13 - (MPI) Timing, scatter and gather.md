
MPI timing:
- MPI has a clock.
- One of the few mpi functions that doesnt respond with error code.
- Walltime: how much real time passes, regardless of whether the time is spent on the program, system calls, libraries, and more.
- Clock on the wall everyone can see.
```C
MPI_Barrier ( MPI_COMM_WORLD );

double t_start = MPI_Wtime();
do_something_useful();
double t_end = MPI_Wtime();

printf (
“Something useful took %ld seconds on rank %d!\n”, t_end – t_start, rank
);
```
- You will get p different timings, but can collect, find average/median/variance and so on.
- Ranks will wait unpredictably long for each other.

Latency:
- α (alfa).
- Interval time from distance between A and B.

Bandwidth / inverse bandwidth:
- β (beta).
- Bandwidth, \[bytes/second].
- β^-1 ( inverse beta).
- How much transfer time do we add by sending additional bytes. \[Second / byte].

Approximate communication time:
- postal model / hockney model.
- First published by Roger W. Hockney.
- With size n being our message, we can estimate transmission time as follows:
![[Screenshot 2026-10-09 at 15.43.18.png|623]]
Hockneys model:
- Estimate message costs on Intel Paragon machine.
- Communication links were equally fast throughout the entire machine.
- Therefore, alfa and beta could be be measured between any pair of processors.


Ping-pong test:
- Start clock.
- Repeat following lots of times: Send message from A to B (ping), send message from B to A (Pong).
- Stop clock.
- Divide time by two and number of messages.

Finding alfa and beta:
- α: Pingpong test with large amount of empty or very short messages. Makes latency dominate timing. 1 byte messages are necessary if specific cpu skips empty messages.
- β: Can find inverse beta with smaller number but huge messages. Bandwidth will dominate the time taken. Huge should reflect how many layers of the memory hierarchy you want the procedure to account for.

Modern times: 
- Uniform latency and bandwidth are long gone.
- Cost of sending messages between adjacent cores on a chip is different to sending to another computer across the room.
- Have to measure as many α/β pairs as you have types of links in platform. 
- Can still be useful if you are careful about where your ranks are running.
- Can also use statistical techniques to make measurements more reliable.

Latency lags bandwidth:
- Difficult to improve upon.
- Often the smaller part of transmission time.
- Restricted by speed of light.
- Bandwidth can be expaneded however.
- Latency masking techniques: Overlapping computation with MPI_Isend.

Collective operations so far:
- MPI_Barrier(): All processes must reach the barrier before any of them can continue. The faster processes wait for the slower ones.
- MPI_Bcast(): One process (root) sends the same data to all other processes in the communicator.
- MPI_Reduce(): One process collects values from all processes, performs operation, and stores result in root process.
- MPI_Allreduce(): Same as MPI_Reduce(), but every process receives the result, not just the root.

Scatter:
- MPI_Scatter.
- Rooted collective function.
- The root process divides data into parts and distributes one part to each process.
- The roots send buffer must contain p times as many elements as the sendcount,  for p participants. 

```C
int MPI_Scatter(
	const void *sendbuf, int sendcount, MPI_Datatype sendtype, // only relevant for root
	void *recvbuf, int recvcount, MPI_Datatype recvtype,
	int root, MPI_Comm comm
);
```
![[Screenshot 2026-10-09 at 16.34.28.png]]

Gather:
- MPI_Gather()
- Each process sends its data to the root, which collects everything into one array.
```C
int MPI_Gather(
	const void *sendbuf, int sendcount, MPI_Datatype sendtype,
	void *recvbuf, int recvcount, MPI_Datatype recvtype, // only relevant for root
	int root, MPI_Comm comm
);
```

![[Screenshot 2026-10-09 at 16.44.37.png]]

Linear scatter:
- Process 0 distributes data directly to every other process. Each process recieves \[n/p] elements.
- \[α] = Latency: Fixed overhead of sending one message.
- \[β] = Bandwidth: Data transferred per unit of time. The cost of sending \[n] elements is \[n/β].
- \[n] = Total number of elements.
- \[p] = Number of processes.

![[Screenshot 2026-10-09 at 17.05.16.png|640]]

Binary tree Scatter:
- Processes help root distribute the data. 
- At each step, processes split their data and forward part to another process.
- Only log_2(p) communication steps are required (assuming \(p\) is a power of 2).
- Lower latency but same bandwidth, better for scaling.

![[Screenshot 2026-10-09 at 17.07.35.png|462]]

Reality:
- n reality, network connections can have different speeds and latencies.
- Performance also depends on hardware, network topology, and MPI implementation.
- The same type of analysis can be applied to other collective operations, such as `MPI_Reduce()`.

