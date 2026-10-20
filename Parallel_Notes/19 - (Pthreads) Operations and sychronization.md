

### Lock/unlock

01_pi_seq:
- Sequentially estimating pi using integration.

02_pi_nolock.c:
- Parallelizing it badly.
- Making pi global and everyone contributes to the same variable.
- Get wrong answer because of race condition, and bad performance.

03_pi_diy_lock.c:
- Attempt at mutex.
- Using a shared integer "flag" to say whose turn it is given by ``
  `flag = (flag + 1) % n_threads` and `while (flag != tid)`
- Each thread busy waits for their turn.
- Serialized but only without certain compiler optimizations. With optimizations it might cause a deadlock.
- Volatile keyword is not memory fence or prevent race conditions, but it stops the compiler from moving read and write instructions that access it around in the code.

04_pi_mutex.c:
- shared variable `pthread_mutex_t lock;`
- Initialize with `pthread_mutex_init (&lock, NULL);`
- Destroy with ``pthread_mutex_destory (&lock);`
- Still bad performance.
- Can use the lock with the following sequence:
```C
pthread_mutex_lock ( &lock );
pi += H / (1.0 + x*x);
pthread_mutex_unlock ( &lock );
```


05_pi_mutex_fast.c:
- Make local partial sums then update shared variable safely in the end.
```C
pthread_mutex_lock ( &lock );
pi += pi_local;
pthread_mutex_unlock ( &lock );
```


Synchronized iterations:
- Many scientific applications use this kind of data-parallel computation seperated by synchronization. Lots of parallel programs use this pattern aswell.


### Condition variables

06_pi_cond_signal.c:

- Uses a mutex, condition variable, and counter.
- Early arrivals lock, increment the counter, and sleep.
- Waiting releases the mutex so other threads can arrive.
- The final arrival sees that everyone has arrived and skips sleeping.
- It decrements the counter, signals one waiting thread, and unlocks.
- Each awakened thread regains the lock, decrements the counter, and signals another.
- The final departing thread skips signaling because nobody remains.
- Threads are released through a chain of wake-ups.

Two barriers are used:

- First: ensure everyone has finished contributing to `pi`.
- Then one thread uses the result and resets `pi`.
- Second: ensure the reset finishes before anyone starts adding the next round's contributions.
- The slides use separate arrival/departure counters and condition variables, plus a mutex for `pi`.



07_pi_cond_broadcast.c:

- Early arrivals add their local sums, increment the arrival counter, and sleep.
- The final arrival adds its contribution and gets the completed sum.
- It prints the result, resets `pi` and the arrival counter, then broadcasts.
- It unlocks so awakened threads can continue.
- Only one mutex and one condition variable are needed in this design.
- No second barrier is needed because the final arrival completes the reset before releasing everyone.


### Built-in barrier

08_pi_barrier.c:

- Uses `pthread_barrier_t`.
- Initialize with `pthread_barrier_init(&barrier, NULL, n_threads)`.
- Wait with `pthread_barrier_wait(&barrier)`.
- Threads wait until `n_threads` have arrived.
- The barrier resets automatically for reuse.
- Destroy with `pthread_barrier_destroy(&barrier)`.
- This pi example uses two barrier waits: before and after processing/resetting the result.


