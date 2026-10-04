
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
- MPI_Bsend
- 

Ready mode:
