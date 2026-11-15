
# MPI

Send/Recv
```C
// Blocking
MPI_Send (buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD); // Standard
MPI_Ssend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD); // Synchronous
MPI_Bsend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD); // Buffered
MPI_Rsend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD); // Ready

// Non-blocking — first declare: MPI_Request req;
MPI_Isend (buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD, &req); // Standard
MPI_Issend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD, &req); // Synchronous
MPI_Ibsend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD, &req); // Buffered
MPI_Irsend(buf, count, MPI_INT, dest, tag, MPI_COMM_WORLD, &req); // Ready

// Complete a non-blocking send before modifying/reusing buf:
MPI_Wait(&req, MPI_STATUS_IGNORE);

// Recv
int numbers[3];
MPI_Recv(numbers, 3, MPI_INT, 0, 7, MPI_COMM_WORLD, MPI_STATUS_IGNORE);

MPI_Irecv(buf, count, MPI_INT, source, tag, MPI_COMM_WORLD, &req);
// ...
MPI_Wait(&req, MPI_STATUS_IGNORE);


```


Collective operations:
```C

// --- Broadcast ---
int the_number = 0;  // Every rank starts with 0

    if (rank == 0) {
        the_number = 42;
    }

    // Every rank calls this; rank 0 supplies the value
    MPI_Bcast(&the_number, 1, MPI_INT, 0, MPI_COMM_WORLD);
    

	
// --- Scatter ---
int data[4] = {10, 20, 30, 40}; // Only rank 0's data is used
int x;                         // Each process has its own x

MPI_Scatter(data, 1, MPI_INT, &x, 1, MPI_INT, 0, MPI_COMM_WORLD);

// --- Gather ---
x = x * 2;
int results[4];

MPI_Gather(&x, 1, MPI_INT, results, 1, MPI_INT, 0, MPI_COMM_WORLD);

// --- Barrier ---	

MPI_Barrier(MPI_COMM_WORLD);
```

Datatypes:
```C
// MPI_INT
// MPI_DOUBLE
// MPI_BYTE

```

Running mpi:
```zsh
mpirun -np 3 ./my_program
```


Initialization: 
```c
int main ( int argc, char **argv ) {
	MPI_Init ( &argc, &argv );
	
	// ... Program
	
	
	MPI_Finalzize();
}
```

Collective code:
```C
if ( rank == 0 )
	for ( int_t i=0; i<N/2; i++ )
		c[i] = a[i] * b[i];
else if ( rank == 1 )
	for ( int_t i=N/2; i<N; i++ )
		c[i] = a[i] * b[i];

//we can write this instead

bounds[2] = { rank * N/2, (rank+1)*N/2 };
for ( int_t i=bounds[0]; i<bounds[1]; i++ )
	c[i] = a[i] * b[i];

```

