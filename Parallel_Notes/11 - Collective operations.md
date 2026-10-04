

Collective code, examples:

``` C
if(rank == 0){
	for(int_t i = 0; i< N/2; i++){
		c[i] = a[i] * b[i];
	}
}
else if (rank == 1){
	for(int_t i = N/2; i<N; i++){
		c[i] = a[i] * b[i];
	}
}

// we can also write

bounds[2] = {rank * N/2, (rank + 1)*N/2};
for(int_t i = bounds[0]; i < bounds[1]; i++){
	c[i] = a[i] * b[i];
}
```

Collective operations:
- Involves every process in its communicator, so every process must call it or else it will hang and timeout.
- Place somwhere where all the ranks come through. 
- Can be in a conditional, but then every path must include operation.
- Example: MPI_Barrier (MPI_COMM_WORLD). Can be used to measure performance, debugging.

Broadcast:
- Root argument designates a rank that acts as the master rank for the operation.
- Takes data from one rank and passes it to everyone.
```C
int MPI_Bcast (
	void *buffer,
	int count,
	MPI_Datatype datatype,
	int root,
	MPI_Comm communicator
);
```


Reduce:
```C
int MPI_Reduce(
    const void *sendbuf,     // Pointer to this process's input data
    void *recvbuf,          // Pointer to store the result; used only on root
    int count,              // Number of elements contributed by each process
    MPI_Datatype datatype,  // Element type, e.g. MPI_INT or MPI_DOUBLE
    MPI_Op op,              // Operation, e.g. MPI_SUM, MPI_MAX, MPI_MIN
    int root,               // Rank of the process that receives the result
    MPI_Comm comm          // Communicator containing participating processes
);

MPI_Reduce(
    &value,          // This process's local value
    &result,         // Where root will store the combined result
    1,               // One element per process
    MPI_INT,         // Each element is an integer
    MPI_SUM,         // Add the values
    0,               // Store the result on rank 0
    MPI_COMM_WORLD
);


```

