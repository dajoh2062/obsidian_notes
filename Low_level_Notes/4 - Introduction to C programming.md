
About:
- Both high and low level language.
- Fast, unsecure and simple.
- Compiled language.
- Procedural (Imperative) language: What operations and what data.
- Runtime error are noe caught, causes crashes.
- Data type sizes are machine dependent.
- Normally signed values but unsigned exists.


Data types:
- char - 8 bits
- short - 16 bit
- int - 32 bit (16/32/64)
- long - 32/64 bit
- float - 32 bit
- double - 64 bit
- bool - 8 bit

Pointer in assembly:

```asm

ldr r1, [r2] @r2 is a pointer to 32 bit memory address
```

Pointers in c:
- & gives address of operator, and * dereferences operater.
- Needs type to know size.

```C

int *p; // p points to an int

p = &i // p points to i now

*p = 5 // changes the value of i to 5

```


Java vs C:
- Java: primitive types is passed by value in functions, an argument with class type passes reference.
- C: Most arguments are passed by the value. Pass by reference needs to use argument of pointer type.


Memory mapping:
- Int a = 65: 00000000 01000001
- Int b = -65: 11111111 10111111
- Twos compliment: Flip all bits, add one if going from positive to negative. Automatically sign extends. 


Memory map larger number to a short:
![[{B82E4A9B-581E-4857-8DDF-7527165CA065}.png|515]]
Memory mapping int to short turning it negative:

![[{5FE34839-F3A2-4365-94D6-21F83AEC0FD9}.png|577]]

Casting (Big endianness):

![[{A674EA45-8908-4943-A6D9-15C63616233A}.png|500]]

![[{4573BD29-DB91-40D8-9911-07298B69B5B5}.png|504]]


Arrays:
- Fixed size by default.
- No lenght knowledge.
- Close relationship between arrays and pointers: Pointers used to pass arrays between functions.

```C

int arr[] = {5, 8, 19}; // 3 integer sized array

int m[2][10]; // matrix, 2d array. 2 rows and 10 columns

point p[4]; // array p of 4 structs point


int arr2[10];

arr[0] = 4; // assinges a value in the array

arr[11] = 6; // assings a value after the array in memory, undefined behaviour 

arr[-1] = 1; // assigns a value before the array in memory, undefined behaviour

```

Equivalents:
![[Pasted image 20260920202251.png|452]]

Pointe arithmetic:

```C
int arr[] = {10, 20, 30}; 
int *p = arr; // p points to arr[0], which contains 10


int x = *p++; // x points to arr[0], which contains 10,
              // but p points to arr[1], containing 20

int x = *++p; // p now points to arr[1], containing 20 // x = 20

int x = (*p)++;
// x = 10, because postfix ++ produces the old value 
// arr becomes {11, 20, 30} 
// p still points to arr[0], which now contains 11


```

Structs:
- Like objects but no methods.

```C

struct point{
	int x, y;
	// can also include structs
}
```




![[{87344DC0-96B5-4305-9349-0FCF6DEAFFD8}.png]]
Structs:

- A `struct` groups related variables into one type. Members can have different types, including other structs.
- In C, structs contain data members and have no built-in methods.
- Use `.` to access a member of a struct variable.
- Write `struct point` unless a `typedef` has introduced `point` as a type alias.

```c
struct point {
    int x;
    int y;
};

// Example statements inside a function:
struct point p1 = {2, 5};
struct point p2 = {0, 0};

p1.x = 10; // p1 now contains {10, 5}
```

Pointers to structs:

- A struct pointer stores the address of an existing struct.
- Use `->` to access a member through a pointer.
- `p->x` means `(*p).x`.
- Changing a member through the pointer changes the original struct.

```c
struct point p1 = {0, 0};
struct point *p2 = &p1;

p1.x = 10;
p2->x = 20; // Changes p1.x to 20
p2->y = 30; // Changes p1.y to 30

// p1 now contains {20, 30}
```

Struct layout and assembly:

- The compiler locates a member using the struct's starting address plus the member's byte offset.
- The slides assume 4-byte integers, with `x` at offset 0 and `y` at offset 4.
- In this layout, the struct occupies 8 bytes.
- Structs can contain padding for alignment, so actual sizes and offsets depend on the implementation.
- Use `sizeof(struct point)` for the size and `offsetof(struct point, y)` for the offset of `y`.

```c
#include <stddef.h>

// Inside a function, with p1 already declared:
size_t size = sizeof(struct point);          // 8 in the slides
size_t offset = offsetof(struct point, y);   // 4 in the slides

int value = p1.y;

// Example ARM translation, assuming r1 contains &p1:
// ldr r0, [r1, #4]
//
// Loads the value at address r1 + 4 into r0.
```

Unsafe struct pointer casts:

- A pointer cast changes the type used to interpret an address.
- It does not create a struct at that address or allocate additional memory.
- The slides cast the address of `p1.y` to a pointer to an entire struct.
- In the illustrated layout, the supposed new `x` overlaps `p1.y`.
- The supposed new `y` lies beyond the original two-member struct.
- This is undefined behavior. The diagrams illustrate possible memory corruption, not guaranteed C results.

```c
struct point p1 = {10, 20};

// Unsafe examples from the slides; do not execute:
//
// ((struct point *)&p1.y)->x = 30;
// Illustrated write overlaps p1.y.
//
// ((struct point *)&p1.y)->y = 40;
// Illustrated write goes beyond p1.

p1.y = 30; // Correct way to change y
```

Wrong swap function:

- C passes function arguments by value: the function receives copies.
- Swapping the local parameters `a` and `b` leaves the caller's variables unchanged.

```c
void swap_wrong(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}

int main(void) {
    int x = 10;
    int y = 20;

    swap_wrong(x, y);

    // Still x = 10, y = 20
    return 0;
}
```

Correct swap using pointers:

- Pass the addresses of the variables using `&`.
- The function receives copies of those addresses.
- Dereferencing the pointers with `*` accesses the caller's original variables.
- A temporary variable preserves the first value while the values are exchanged.

```c
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main(void) {
    int x = 10;
    int y = 20;

    swap(&x, &y);

    // Now x = 20, y = 10
    return 0;
}
```

Generic swap:

- `void *` can hold the address of an object of any type.
- The function also needs the object's size in bytes.
- `memcpy(destination, source, size)` copies the specified number of bytes.
- Save the first object's bytes, copy the second into the first, then copy the saved bytes into the second.
- Use writable objects of the same type and pass their correct size.
- Distinct objects must not overlap.
- The temporary array below is a variable-length array and requires compiler support for that C feature.

```c
#include <string.h>

void swap_bytes(void *a, void *b, size_t size) {
    if (size == 0 || a == b) {
        return;
    }

    unsigned char buffer[size];

    memcpy(buffer, a, size);
    memcpy(a, b, size);
    memcpy(b, buffer, size);
}

int main(void) {
    int x = 10;
    int y = 20;

    swap_bytes(&x, &y, sizeof x);

    // Now x = 20, y = 10
    return 0;
}
```

Swapping pointers:

- If a program accesses large objects through pointers, exchanging the pointers can avoid copying the objects.
- This changes which object each pointer refers to.
- The original objects retain their values.

```c
struct point first = {10, 20};
struct point second = {30, 40};

struct point *a = &first;
struct point *b = &second;

struct point *temp = a;
a = b;
b = temp;

// a now points to second.
// b now points to first.
// first and second retain their original values.
```

Memory regions:

- Stack: typically holds automatic local variables and function-call information.
- Compiler-generated code manages the stack. Some variables may instead remain in registers or be optimized away.
- Heap: holds dynamically allocated objects. In C, allocate using functions such as `malloc` and release using `free`.
- Static data: holds globals and `static` variables, which exist throughout program execution.
- Instructions: contains the program's machine code.
- SP is the stack pointer.
- PC is the program counter associated with instruction execution.
- The slide shows a simplified layout with stack and heap growing toward each other. Actual layouts depend on the system.

```c
#include <stdlib.h>

int global_value = 5; // Static storage duration

int main(void) {
    int local = 10; // Automatic local variable

    // p is local; the allocated int is on the heap.
    int *p = malloc(sizeof *p);

    if (p == NULL) {
        return 1; // Allocation failed
    }

    *p = local;

    free(p); // Release the allocated object
    p = NULL;

    return 0;
}
```

Variable scope and lifetime:

- Scope determines where a variable's name can be used.
- Lifetime determines how long its object exists.
- Global variables are defined outside functions and exist for the entire program.
- A global's name is visible from its declaration to the end of the file, unless hidden by another declaration.
- An `extern` declaration lets code refer to a global defined elsewhere, including another source file when it has external linkage.
- A file-level `static` variable has internal linkage: other source files cannot refer to it by that name.
- Automatic local variables belong to their enclosing block and have a new instance on each entry.
- Separate function calls have separate instances of their automatic local variables.
- A local `static` variable keeps its value between calls while its name stays local to its block.

```c
int global_count = 0;
static int file_only = 0;

void count_calls(void) {
    int local_count = 0;  // Starts at 0 on every call
    static int calls = 0; // Initialized once; retains its value

    local_count++;
    calls++;
    global_count++;
    file_only++;

    // After the second call:
    // local_count = 1
    // calls = 2
}

// In another source file, declare the existing global with:
// extern int global_count;
```

Endianness and reading a smaller type:

- Endianness describes the byte order of a multi-byte value in memory.
- Assume 8-bit bytes, a 4-byte `int`, and a 2-byte `short`.
- Little-endian stores the integer 8 as `08 00 00 00`, listed in increasing address order.
- Big-endian stores the integer 8 as `00 00 00 08`.
- In the slides' byte-level demonstration, interpreting the first two bytes as a short gives 8 on little-endian and 0 on big-endian.
- In actual C, accessing an `int` through a `short *` violates type-access rules and causes undefined behavior. Those results are therefore not guaranteed.
- A normal numeric conversion preserves the value 8 on either byte order.
- Writing or displaying `0x00000008` is numeric notation. It does not mean a register uses big-endian memory ordering.

```c
int a = 8;

// Unsafe pointer reinterpretation from the slides:
// short b = *(short *)&a;

short b = (short)a; // Correct numeric conversion: b = 8
```

Reading a larger type and masking:

- Casting the address of a 2-byte `short` to `int *` does not turn it into a 4-byte object.
- Dereferencing that pointer attempts to read beyond the short.
- It also violates type-access rules and may violate alignment requirements: undefined behavior.
- The slides show two unknown neighboring bytes whose contents can change the apparent result.
- On the illustrated little-endian layout, `& 0xFFFF` keeps the lower 16 bits.
- Masking afterward does not fix the invalid read.
- Use a normal assignment to convert a short's numeric value to an int.

```c
short a = 8;

// Unsafe: attempts a 4-byte read from a 2-byte object.
// int b = *(int *)&a;

int b = a; // Correct conversion: b = 8

// Separate, valid example of masking an existing integer:
// Assume unsigned int is 32 bits.
unsigned int value = 0xABCD0008u;
unsigned int low16 = value & 0xFFFFu;

// low16 = 8
```