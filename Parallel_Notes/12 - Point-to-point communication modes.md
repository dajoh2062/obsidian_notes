
Deadlocks:
- Some send and recv patterns can cause deadlock. 
- Eg.: send left, recv right, send right, recv left. Here send cannot return until everyone has received.
- One solution is to do send/recv and recv/send for every-other rank.
- Another solution is mutual exchange: "MPI_Sendrecv".

Mutual exchanges:
```C
int MPI_Sendrecv (
	const void *sbuf, int scount, MPI_Datatype stype, int dest,
	void *rbuf, int rcount, MPI_Datatype rtype, int source,
	MPI_Comm communicator, MPI_Status *status
);

int outgoing = (rank == 0) ? 100 : 200;
int incoming;
int other = 1 - rank;  // Rank 0 talks to 1; rank 1 talks to 0

MPI_Sendrecv(
    &outgoing,          // sbuf: address of the value to send
    1,                  // scount: send one element
    MPI_INT,            // stype: the element is an integer
    other,              // dest: send to the other process
    42,                 // sendtag: label the outgoing message 42

    &incoming,          // rbuf: address where received data is stored
    1,                  // rcount: space for one element
    MPI_INT,            // rtype: receive an integer
    other,              // source: receive from the other process
    42,                 // recvtag: accept a message tagged 42

    MPI_COMM_WORLD,     // communicator: participating process group
    MPI_STATUS_IGNORE  // status: we don't need receive details
);
```


### Communication modes

The sender decides how to transmit, and the recieving method stays the same.

Standard mode:
- MPI_Send
- arguments are buffer-pointer, count, type, destination, tag and communicator.
- Liberty to do what is "best on this machine".
- Returns as soon as it can.

Synchronized mode:
- MPI_Ssend
- Will not return to caller before receiving process starts receiving.
- A little slower, but more consistency between how far the communicating processes have come before returning.
- Easier to create deadlocks.

Buffered mode:
- MPI_Bsend and MPI_Buffer_attach
- Lets you allocate the buffer memory manually, so you can make it one long continuous memory range.
```C
int buffer_size = n*sizeof(msgsize) + MPI_BSEND_OVERHEAD;
int *my_buffer = malloc ( buffer_size );
MPI_Buffer_attach ( my_buffer, buffer_size );

MPI_Buffer_detach ( &my_buffer, &buffer_size );

```

Ready mode:
- MPI_Rsend
- Use when 100% confident that the corresponding recv call has been made. 
- If recv has not been made, it is an error, and the result is arbitrary.

Non-blocking send:
- MPI_Isend.
- Returns immediatly.
- Leaves program free to continue.
- Send message later at MPIs convenience.
- When you actually need to make sure that the transfer has completed, you can wait for the request to say that its finished.
```C
int MPI_Isend (
	const void *buffer,
	int count,
	MPI_Datatype type,
	int destination,
	int tag,
	MPI_Comm communicator,
	MPI_Request *request 
	// extra argument. Hands some memory to MPI, which writes there.
);


int MPI_Wait ( MPI_Request *req, MPI_Status *stat )
// or
int MPI_Wait ( MPI_Request *req, MPI_STATUS_IGNORE)

// Sending multiple 
MPI_Request my_reqs[42];
for ( int m=0; m<42; m++ )
	MPI_Isend (&msgs[m], 1, MPI_INT, dst, 0, MPI_COMM_WORLD, &my_reqs[m]);

// waits for all of them to complete
MPI_Waitall ( 42, my_reqs, MPI_STATUSES_IGNORE );

```


Communicate vs. compute:
- Communcation calls can be very expensive compared to local operations.
- Rule of thumb: "send early, recieve late".
- Compute result while messages are underway.
- Overlapping communication and computation is a popular application of MPI_Isend.


Non blocking version for other modes:
- MPI_Isend
- MPI_Issend
- MPI_Ibsend
- MPI_Irsend
- MPI_Irec

Persistent Communication in MPI:
- Purpose: Used when processes repeatedly send or receive messages using the same communication pattern.
- Initialize once: MPI prepares the communication once, reducing setup overhead in loops.
- MPI_Send_init(): Creates a persistent send request without actually sending data.
- MPI_Recv_init(): Creates a persistent receive request without actually receiving data.
- MPI_Start(): Starts a previously initialized communication request.
- MPI_Startall(): Starts multiple persistent requests at once.
- MPI_Wait(): Waits until the communication is completed before reusing the request.
- MPI_Request_free(): Frees the persistent request when no longer needed.
- Reusability: Data values can change between iterations, but communication parameters (destination, tag, count, etc.) remain fixed.
- Non-blocking: MPI_Start() returns without waiting for communication to complete.
- Benefits: Reduces setup overhead, simplifies code, and can improve performance.
- Key sequence: Initialize once → Start → Wait → Repeat Start/Wait → Free.