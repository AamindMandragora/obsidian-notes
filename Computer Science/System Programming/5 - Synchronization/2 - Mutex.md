To ensure that only one thread at a time can access a global variable, we can use a mutex, short for mutual exclusion, which bars all other threads from accessing that section of their function until the thread completes it. Mutexes aren't true primitives, but they're one of the smallest that has a useful interface with threading. It's also not a data structure, but an abstract data type, and there are many ways to implement one. To use the mutex defined by `pthread`, we call:

```c
pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_lock(&m);
// critical section
pthread_mutex_unlock(&m);
```
# Mutex Lifetime

We can either create a custom mutex using `pthread_mutex_init(pthread_mutex_t *mutex, pthread_mutexattr_t, *attr)` or just use the macro `PTHREAD_MUTEX_INITIALIZER` that calls the default constructor. It's recommended to call the `init` function when allocating the mutex on the heap, but we can use either method.

When we're finished with the mutex, we can call `pthread_mutex_destroy(&m)`, which is only defined for a locked mutex. We also can't initialize an already initialized mutex or copy the bytes to a new location and use that. Global and static mutexes don't have to be destroyed, and we shouldn't initialize a mutex using more than one thread.
# Mutex Usages

To use a mutex, wrap the critical sections of the function in `pthread_mutex_lock` and `pthread_mutex_unlock`. However, these functions come with overhead, so make sure to call them as little as possible. Often, it's best to perform all operations locally and only put locks around loading from and storing to global or static variables.

We must remember that the only way mutexes can stop program flow is if one thread tries to lock an already locked mutex, which means we shouldn't ever use separate ones for separate threads. We should also never fork after a mutex has been initialized, try to lock a mutex using a thread that wasn't the one that locked it, forget to unlock the mutex before an early return, forget to call `pthread_mutex_destroy`, use an uninitialized mutex, or lock it twice on a thread without unlocking it first. While different threads shouldn't use different mutexes, it's fine and often preferred for different data structures within the thread's function to use different locks to speed up programs.
# Mutex Implementation

The naive approach to implementing a mutex is giving it a single boolean field `locked`. `lock` would then wait until it's unlocked and then lock it for other threads, and `unlock` would just set `locked` to zero. This wastes a lot of CPU resources but also introduces a race condition if two threads call `lock` at the same time and read `locked` as zero. To reduce the CPU wastage, we can call `pthread_yield()` inside `lock`, but that still leaves the race condition.
# Implementing a Mutex with Hardware

The computer's architecture defines several instructions that can't be interrupted by any other instruction called atomics. The code for a spinlock mutex is as follows:

```c
typedef struct {
	atomic_int_least8_t lock;
	pthread_t owner;
} struct mutex;

#define UNLOCKED 0
#define LOCKED 1
#define UNASSIGNED_OWNER 0

int mutex_init(mutex* mtx) {
	if (!mtx) return 0;
	atomic_init(&mtx->lock, UNLOCKED);
	mtx->owner = UNASSIGNED_OWNER;
	return 1;
}

int mutex_lock(mutex* mtx) {
	int_least8_t zero = UNLOCKED;
	while (!atomic_compare_exchange_weak_explicit(&mtx->lock, &zero, LOCKED, memory_order_seq_cst, memory_order_seq_cst)) {
		zero = UNLOCKED;
		sched_yield();
	}
	mtx->owner = pthread_self();
	return 1;
}

int mutex_unlock(mutex* mtx) {
	if (unlikely(mtx->owner != pthread_self())) return 0;
	int_least8_t one = LOCKED;
	mtx->owner = UNASSIGNED_OWNER;
	if (!atomic_compare_exchange_weak_explicit(&mtx->lock, &one, UNLOCK, memory_order_seq_cst, memory_order_seq_cst)) {
		return 0;
	}
	return 1;
}
```

When this mutex is locked, the function repeatedly performs the Compare-and-Swap operation, which is supported by most modern architectures (`lock cmpxchg` on x86). It checks if the first argument equals the second, and if so sets it to the third argument. Otherwise, it sets the second argument to the first. This is all done atomically, in one uninterruptible instruction.

However, atomic functions are prone to false alarms. There are therefore two versions of each atomic function: a strong function that guarantees success or failure, or the faster weak function that may return a false fail. We use the weak version above as we're in a loop and it doesn't matter if it fails a bit more often.

If we're inside the while loop, it means we've failed to grab the lock, so we reset `zero` to unlocked and sleep for a while, and once we exit the loop, we lock the mutex and set its owner to the current thread.

This guarantees mutual exclusion since the thread that successfully swaps the lock from `UNLOCKED` to `LOCKED` becomes the winner, and the rest keep spinning. To satisfy the API, a thread can't unlock the mutex unless it owns it, and then we unassign the thread owner and try and swap to unlock.

The `seq_cst` part of the instruction is the specified memory order, and specifies that all threads see the same global order of operations. It's super expensive on the CPU, which is why there's an opposite memory order called `relaxed`, which doesn't guarantee anything about order of operations and just ensures atomicity for the variables themselves. If we just need local synchronization, we can use `acquire` and `release` to guarantee all memory reads after and all memory writes before the load or store operation can't be reordered to happen on the opposite side, respectively.
# Semaphore

Semaphores are another synchronization primitive that holds a value, and are initialized using `int sem_init(sem_t *sem, int pshared, unsigned int value)`, where the first parameter is a pointer to the semaphore object, the second defines the scope (0 for sharing between threads and otherwise for sharing between processes), and the third is the initial count value. It has two functions, `sem_wait` and `sem_post`, that decrement and increment the held value. 

A mutex is a semaphore that always waits before it posts, and many textbooks refer to it as a binary semaphore. We can implement a mutex by initializing the semaphore with a count of one, then replacing `lock` and `unlock` with `wait` and `post`. This breaks if we add more than one to the semaphore, which is usually why we use mutexes, which can handle a double-unlock, to implement semaphores and not the other way around. Also, binary semaphores can be unlocked from a different thread.
## Signal Safety

`sem_post` is one of a handful of functions that can correctly be used inside a signal handler, while `pthread_mutex_unlock` is not, which means we can write signal handlers that unlock binary semaphores. We can also use semaphores to keep track of the empty spaces in arrays.