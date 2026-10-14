![[Screenshot 2026-10-12 at 18.07.06.png|641]]Misc:
- ```#include <pthread.h>```.
- Posix: Portable Operating System interface.
- Requires OS support, which most do.
- Need a function to call.
- "Threadable" functions have this signature ```void *my_function ( void *argument ){}```. 
- The function body will have to cast the argument into the type it expects.
- The caller of the function will have to cast the return value to what it expects.
- Threads can’t actually prevent each other from entering a critical section, you have to program them so that they all respect the state of a locking mechanism everyone can see.
- Threads can’t forcibly exclude each other from overwriting shared values, you have to program them so that they don’t try to.


Pointer types:
- ```void *x``` pointer to something but not knowing the type.
- ```int *x``` pointer to int. Writing the type makes C compiler check and give warning.
- ```ten_sixteen_bit_ints[7]``` and ```*(ten_sixteen_bit_ints+7)``` will both address the array variable plus 7 times size of the sixteen bit int ```int16_t```. Cant do the same with a void pointer, because we dont know what +1 address space amounts to. Removes all protection from C type checking.
![[Screenshot 2026-10-12 at 18.01.16.png]]
```C
#include <pthread.h>
// and use -pthread with compiling

// threadable function signature.
void * my_function ( void *arg );

thread_t my_thread;

pthread_create ( &my_thread, NULL, &my_function, NULL );
// thread handle, thread attributes, pointer to function, (void *) to the argument
// Thread attributes: pointer to struct phtread_attr_t. 
// can be created with pthread_attr_init and destroyed with pthread_attr_destroy 
// lets you control OS info about the thread like max liftetime, stack size, sheduling
// priority and more. Null will give default values.

// waiting for thread to finish
pthread_join ( my_thread, NULL );
// thread handle, output

```

HPC - style pthreads:
- Usually dont want more threads than cores.
- All threads tend to do same thing.
- Perform on large amount of shared data, making it pointless to pass and recieve values from function calls.
- Works by giving each thread unique index and calculating its parts from of the global problem from the index and total number.
- Common to switch the void* argument for an integer instead, an have the amount of threads as a global variable (because memory addresses are intergers anyway).



argument_and_return.c:

```C
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>


// Shared location to store return values in
int result[2];


// Function we can make a thread out of:
//  * expects the argument to point at an array of 4 integers
//  * expects the result pointer to have space for 2 integers
//  * calculates the 2 output-integers as pairwise sum-of-squares for the input
void *
do_this ( void *arg )
{
    int *values = (int *)arg;
    printf ( "%d %d\n", values[0], values[1] );
    result[0] = values[0]*values[0] + values[1]*values[1];
    result[1] = values[2]*values[2] + values[3]*values[3];
    printf ( "%d %d\n", result[0], result[1] );
    return result;
}


// Main function: sets up arguments, spawns and joins a thread
int
main ( int argc, char **argv )
{
    // Make a handle for our extra thread
    pthread_t worker;

    // Set up our arguments and return value
    int
        *inputs = malloc ( 4*sizeof(int) ), // Passed into pthread_create
        *output = NULL;                     // Will be written by pthread_join

    // Fill in some argument values
    inputs[0] = 1;
    inputs[1] = 2;
    inputs[2] = 3;
    inputs[3] = 4;

    // Launch the thread
    pthread_create ( &worker, NULL, &do_this, inputs );
    printf ( "Started thread\n" );

    // Wait for it to complete
    pthread_join ( worker, (void **) &output );
    printf ( "Joined thread\n" );

    // Print the result
    printf ( "%d %d\n", output[0], output[1] );
    exit ( EXIT_SUCCESS );
}

```

hello_pthreads.c:

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <pthread.h>


typedef int64_t int_t;


// Default number of threads, can be overwritten with command line argument
int_t n_threads = 2;


// Trivial function to make threads out of
void *
say_hi ( void *arg )
{
    // Cast my thread-id
    int_t tid = (int_t) arg;
    // Say hello
    printf ( "Hello world, I am thread %ld out of %ld\n", tid, n_threads );
    // Stop
    pthread_exit ( EXIT_SUCCESS );
}


// Main function
int
main ( int argc, char **argv )
{
    // Check if we have a request for a non-default number of threads
    if ( argc > 1 )
        n_threads = strtol ( argv[1], NULL, 10 );

    // Make space for that many threads
    pthread_t threads[n_threads];

    // Spawn all the threads, and give them their 'rank' (thread-id)
    for ( int_t i=0; i<n_threads; i++ )
        pthread_create ( &threads[i], NULL, &say_hi, (void *)i );

    // Wait for all the threads to finish
    for ( int_t t=0; t<n_threads; t++ )
        pthread_join ( threads[t], NULL );

    // Quit
    exit ( EXIT_SUCCESS );
}

```