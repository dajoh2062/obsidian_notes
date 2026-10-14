
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

MPI extent:
- Extent defines the distance in bytes between the starting positions of two consecutive instances of a datatype.
- Displacement defines where data is located within one instance of the datatype.
- Size is the number of bytes of actual data being sent, while extent can include gaps between elements.
- Example: A datatype contains two floats at offsets 0 and 8. Its default extent is 12 bytes (assuming 4-byte floats).
- Resizing the extent: Using MPI_Type_create_resized(), we can change the extent from 12 to 16 bytes without changing which floats belong to the datatype.
- Why is this useful? When sending multiple instances (count > 1), MPI uses the extent to determine where the next instance starts.
  - Extent = 12 → reads offsets 0, 8, 12, 20
  - Extent = 16 → reads offsets 0, 8, 16, 24
- Important: Changing extent does not change how much data is sent per instance or add padding to memory. It only changes the spacing between repeated instances.

Creating structured types:
```C
int MPI_Type_struct (
	int count, // How many parts do we have?
	int * array_of_blocklengths, // What are their block lengths?
	MPI_Aint *array_of_displacements, // What are their displacements?
	MPI_Datatype *array_of_types, // What are their types?
	MPI_Datatype *new_type // Output: a brand new type
);

int MPI_Commit_type ( MPI_Datatype *t );
// make the translation from MPI types into the native
//addressing mechanism of your computer


// in practise 
MPI_Datatype my_type;
MPI_Type_struct ( foo, bar, baz, &my_type);
MPI_Commit_type ( &my_type );
MPI_Send ( ptr, 2, my_type, dst, tag, MPI_COMM_WORLD )

struct Person {
    int age;
    float height;
};

struct Person people[2] = {
    {20, 1.80},
    {25, 1.75}
};

// Define the datatype
int block_lengths[2] = {1, 1};

MPI_Aint displacements[2] = {
    offsetof(struct Person, age),
    offsetof(struct Person, height)
};

MPI_Datatype types[2] = {MPI_INT, MPI_FLOAT};

MPI_Datatype person_type;

MPI_Type_struct(
    2,
    block_lengths,
    displacements,
    types,
    &person_type
);

MPI_Type_commit(&person_type);

MPI_Send(people, 2, person_type, dst, tag, MPI_COMM_WORLD);

```


Vector types:
- Useful when dealing with multidimetional arrays and more.
- Useful when collecting values with a common stride between.
- Consists of count, block length, common stride. 

```C
MPI_Datatype my_type;

// 5 blocks, 2 doubles per block, stride of 5 doubles
MPI_Type_vector(5, 2, 5, MPI_DOUBLE, &my_type);
MPI_Type_commit(&my_type);

// Send columns 0 and 1
MPI_Send(&ARRAY(0,0), 1, my_type, dst, tag, MPI_COMM_WORLD);

// Send columns 2 and 3
MPI_Send(&ARRAY(0,2), 1, my_type, dst, tag, MPI_COMM_WORLD);

MPI_Type_free(&my_type);
```

![[Screenshot 2026-10-12 at 14.07.55.png|221]]

Subarrays:

```C
double matrix[4][5] = {
    { 1,  2,  3,  4,  5},
    { 6,  7,  8,  9, 10},
    {11, 12, 13, 14, 15},
    {16, 17, 18, 19, 20}
};

MPI_Datatype my_type;

int sizes[2]    = {4, 5};  // Entire matrix: 4 rows, 5 columns
int subsizes[2] = {2, 3};  // Select 2 rows, 3 columns
int starts[2]   = {1, 1};  // Start at row 1, column 1

MPI_Type_create_subarray(
    2,
    sizes,
    subsizes,
    starts,
    MPI_ORDER_C, // C style order (row major), MPI_ORDER_FORTRAN for cmo
    MPI_DOUBLE,
    &my_type
);

MPI_Type_commit(&my_type);

// Send selected subarray
MPI_Send(&matrix[0][0], 1, my_type, dst, tag, MPI_COMM_WORLD);

MPI_Type_free(&my_type);
```


Other types:
```C
int MPI_Type_contiguous (int count, MPI_Datatype oldtype, MPI_Datatype *newtype);
// Block of memory

int MPI_Type_indexed (
int count, // Nr. of parts
int *block_lengths, // List of blocklengths for the parts
int *displacements, // List of their displacements
MPI_Datatype oldtype, // What kind of elements?
MPI_Datatype *newtype
);
// Similar to MPI_Type_struct just with all elements same type.

```

