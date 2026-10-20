
Open Multi Processing:
- V1 released in 1997.
- Tool used for scientific number crunching and computing.
- Targets shared memory systems, unit of paralleism is threads as with pthreads.
- More covenient way to use pthreads, and often implemented with ptreads.
- Its core is a set of language extensions that must be supported by the compiler.
- It also has a runtime library of functions that you can call to inspect the state of what the compiler has generated.

Language extentions:
- Phtreads code is repetitive:
	- Declare, initialize, use, and destroy a mutex for every thing that needs protection. 
	- Declare, initialize, use, and destroy a cond for every signal.
	- Declare, initialize, use, and destroy an object for every barrier.
	- Simple sets of operations make for lots of repetitive typing.
- Can make the compiler generate the repetitive code, using a tiny languiage embedded in \#pragma directives. Used in OpenMP


Basic block:
```C
int main () {
	int a = 42, b = 32, c = 0;
	{
		int a = 64;
		c = a – b;
	}
	printf ( “a = %d, b = %d, c = %d\n”, a, b, c );
	return 0;
}
// will print "a = 42, b = 32, c = 32"
```

hello_openmp.c and using openMP:
- using flag `-fopenmp` for gcc
- we know have the most important info, thread id and thread count.
- Openmp has more more concepts and tools than this.
- default thread count is equal to number of cores.
- Can adjust it without recompiling by changing the shell enivironment variable "OMP_NUM_THREADS", and OpenMP will use it.
- Can also hardcode it into program using `#pragma omp parallel num_threads(4)`

```c
// OpenMP run time library functions
#include <omp.h>


int
main ()
{
    int n_threads = omp_get_num_threads();
    int tid = omp_get_thread_num();
    printf ( "Hello, I am thread %d of %d\n", tid, n_threads );

    printf ( "Let us go momentarily parallel...\n" );
    #pragma omp parallel
    {
        int n_threads = omp_get_num_threads();
        int tid = omp_get_thread_num();
        printf ( "Hello, I am thread %d of %d\n", tid, n_threads );
    }

    printf ( "...aaaand we're back to normal:\n" );
    n_threads = omp_get_num_threads();
    tid = omp_get_thread_num();
    printf ( "Hello, I am thread %d of %d\n", tid, n_threads );

    exit ( EXIT_SUCCESS );
}

```

pi_mutex_openmp.c:
- equal to pi_mutex fast.
- smoother ways to implement in openmp
```C
#define _XOPEN_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <math.h>
#include <omp.h>

#define STEPS (1e8)
#define H (1.0/STEPS)

// Global values, accessible by all threads
omp_lock_t lock;
double pi = 0.0;

int
main ( int argc, char **argv )
{
    // Make a lock
    omp_init_lock ( &lock );

    // Estimate pi in parallel
    #pragma omp parallel
    {
        int
            tid = omp_get_thread_num(),
            n_threads = omp_get_num_threads();
        double
            x = tid * H,
            pi_local = 0.0;
        for ( size_t i=0; i<STEPS; i+=n_threads )
        {
            x += n_threads*H;
            pi_local += H / (1.0 + x*x);
        }

        // Avoid race conditions for the global pi value
        omp_set_lock ( &lock );
        pi += pi_local;
        omp_unset_lock ( &lock );
    }

    // We're not parallel anymore, print the result and clean up
    pi *= 4.0;
    printf ( "Estimated %e, missed by %e\n", pi, fabs(pi-M_PI) );
    omp_destroy_lock ( &lock );
    exit ( EXIT_SUCCESS );
}

```

OpenMp barriers:
- `#pragma omp barrier`
- following example is similar to "pi_barrier.c", but there are smoother ways to implement this in OpenMP aswell.
- pi_barrier_openmp.c:
```C
#define _GNU_SOURCE
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <math.h>
#include <omp.h>

#define ITERATIONS 10
#define STEPS (1e8)
#define H (1.0/STEPS)


double pi = 0.0;
omp_lock_t lock;


int
main ( int argc, char **argv )
{
    omp_init_lock ( &lock );
    #pragma omp parallel
    {
        int
            tid = omp_get_thread_num(),
            n_threads = omp_get_num_threads();

        for ( size_t iter=0; iter<ITERATIONS; iter++ )
        {
            double x = tid*H, pi_local = 0.0;
            for ( size_t i=0; i<STEPS; i+=n_threads )
            {
                x += n_threads*H;
                pi_local += H / (1.0 + x*x);
            }

            omp_set_lock ( &lock );
            pi += pi_local;
            omp_unset_lock ( &lock );
            
            #pragma omp barrier

            if ( tid == 0 )
            {
                pi *= 4.0;
                printf ( "Estimated %e, missed by %e\n", pi, fabs(pi-M_PI) );
                pi = 0.0;
            }
            #pragma omp barrier
        }
    }
    omp_destroy_lock ( &lock );
    exit ( EXIT_SUCCESS );
}

```

Condition variables: 
- No direct equivalent of pthread_cond_t for thread signaling.

Performance: 
- Keep critical sections short; busy waiting and using more threads than cores can hurt performance.

Correctness: 
- Easier than pthreads, but you must still handle shared/private variables and choose safe parallel work.

Examples: 
- Manually using thread IDs and counts demonstrates the connection to pthreads; typical OpenMP code uses higher-level constructs.

Why learn pthreads: 
- Helps you understand what OpenMP handles behind the scenes.