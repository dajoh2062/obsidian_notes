
Primitives types:
- MPI_INT64_T
- MPI_DOUBLE
- MPI_CHAR
- And more.
- Fine for sending rows of consecutive array elements, but what about columns or contents of structs?

Solution 1; DIY packing:
- Manually pack/unpack contents of buffer. 
- Requires lots of extra code and effort.
- Can use MPI_Pack and MPI_Unpack to dispense some of the pointer arithmetic.

```C
struct { int i; int j; double v; } my_struct; // Structured data
uint8_t my_buffer [ 2*sizeof(int)+sizeof(double) ]; // Just some bytes
*((int *)&my_buffer[0]) = my_struct.i; // Count bytes
*((int *)&my_buffer[sizeof(int)]) = my_struct.j;
*((double *)&my_buffer[2*sizeof(int)]) = my_struct.v;
```


Derived types:
- Can use derived types to be more elegant than packing.
- Combines some other types in a structure that describes their layout in memory.
- Derived types must be constructed and comitted to MPI for compiling and efficient representation, and then be used just as regular types.
- Consists of type signature tn and displacement dn.
- Displacement is memory offset compared to lower bound.
![[Screenshot 2026-10-09 at 17.45.49.png|606]]

Memory locations:
- MPI_Aint: A special integer type provided by MPI that is large enough to represent memory addresses.
- Displacements have this type.
- Wrapper for pointers.


Size of derived type:
- Size: Sum bytes required for the structure (example 6 bytes for 1 float and 2 chars).
- Extent: Sum bytes required for the structure if including spacing and gaps (example will be 9 bytes).
- Can increase extent manually and add padding. Example of two float in each struct with 4 bytes padding between.
![[Screenshot 2026-10-09 at 18.06.51.png]]

Resizing types:
```C
int MPI_Type_create_resized (
	MPI_Datatype old_type, // Type to start with
	MPI_Aint lower_bound, // New value for lower bound
	MPI_Aint extent, // New value for extent
	MIP_Datatype *new_type // Result comes out here
);

MPI_Datatype new_type;

MPI_Type_create_resized(
    MPI_INT64_T,
    0,
    8 * sizeof(int64_t),
    &new_type
);

MPI_Type_commit(&new_type);

// Sends data[0], data[8], data[16]
MPI_Send(data, 3, new_type, 1, 0, MPI_COMM_WORLD);

MPI_Type_free(&new_type);
```



11

