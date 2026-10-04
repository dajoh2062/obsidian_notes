

![[{34AEEE24-4059-4460-B730-5BA2D82A61E5}.png]]
![[{4845E341-F51D-43CD-8B5E-82509B6D516A}.png]]

### MPI functions:

Initialization:
```C
int main ( int argc, char **argv ) {
	MPI_Init ( &argc, &argv );
```

Finalization:
```C
// Inside main
MPI_Finalize();
// cleans up all memory allocated during init
```

Communicator:
```C
int rank, size;
// Rank and size
MPI_Comm_rank ( MPI_COMM_WORLD, &rank );
MPI_Comm_size ( MPI_COMM_WORLD, &size );
```

Sending:
```C
int MPI_Send (
	const void *buf, // Pointer to the data to send
	int count, // Number of elements to send from it
	MPI_Datatype datatype, // Datatype such as MPI_INT, MPI_DOUBLE, MPI_BYTE and so on. Message length is count*type.size
	int dest, // Rank of recipient
	int tag, // integer label, lest you distinguish different messages
	MPI_Comm comm // Communicator to send in
);
// Return value is usually the constant MPI_SUCCESS
```

Recieving:
- MPI pairls the correct send and recv by checking size, type, source, destination and so on. But tag lets you distinguish messages if necessary.
```C
int MPI_Recv (
	const void *buf, // Where to put the result
	int count, // Number of elements
	MPI_Datatype datatype, // Type of elements
	int src, // Rank of sender
	int tag,
	MPI_Comm comm, // Communicator to send in
	MPI_Status *status // Lets you get information about how the message was sent after you have recieved it
);
```

Sending and recieving:
```C
int MPI_Sendrecv(
    const void *sendbuf,     // Pointer to the data to send
    int sendcount,          // Number of elements to send
    MPI_Datatype sendtype,  // Type of elements sent, e.g. MPI_INT
    int dest,               // Rank of the process to send to
    int sendtag,            // Integer label for the outgoing message

    void *recvbuf,          // Pointer to memory where received data is stored
    int recvcount,          // Maximum number of elements to receive
    MPI_Datatype recvtype,  // Type of elements received, e.g. MPI_INT
    int source,             // Rank to receive from, or MPI_ANY_SOURCE
    int recvtag,            // Tag to receive, or MPI_ANY_TAG

    MPI_Comm comm,          // Communicator used for both send and receive
    MPI_Status *status     // Receive details, or MPI_STATUS_IGNORE
);
// Returns MPI_SUCCESS on success
```

