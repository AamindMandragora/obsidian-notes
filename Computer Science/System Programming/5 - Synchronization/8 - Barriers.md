If we want to perform a multithreaded calculation with two distinct stages such that we don't want any thread to reach stage two while another thread is still in stage one, we can use a **barrier**, forcing all threads to wait for the rest. `pthread` includes a `pthread_barrier_t`, which is initialized with the number of threads that must be blocked by the barrier.

To implement our own barrier, let's first figure out where the barrier goes. In the thread function, each thread does the first stage of calculations, then waits on the barrier, then does the second stage of calculations. We can try making a global variable initialized to the number of threads, then decrement it and broadcast to a condition variable if the number of remaining threads is zero, waking up the other threads.

The above implementation isn't reusable as it forces the code to assume that, if the number of remaining threads is zero before the decrement, it's the beginning of a new iteration. However, if a single thread leaves the wait, finishes its calculation, and resets the number of remaining threads before any other threads even wake up, it deadlocks. We might be able to fix this by noticing that multiple threads calling `barrier_wait` in a loop must be in the same iteration.
# Reader-Writer Problem

Imagine we have a key-value data structure being used by many threads. Since reading doesn't affect the values in any way, we should allow multiple threads to read at the same time. However, to avoid data corruption, we can only perform a write operation when no other operation, read or write, is being performed. If we simply tried wrapping read and write in a mutex lock, we prevent the latter but force readers to wait on other readers. However, if we try checking boolean flags `reading` and `writing` at the beginning of both calls, we'd suffer from a race condition where two threads could read the same values of flags and write at the same time. Also, we could have more than one reading thread and one writing thread, requiring us to keep track of their numbers. We can do this using a condition variable, which will first atomically unlock the mutex, sleep until woken, then wait until the awoken thread regains the mutex lock before returning If we use condition variables, our implementation will look like this:

```c
read() {
	lock(&m);
	while (writing) cond_wait(&cv, &m);
	reading++;
	unlock(&m);
	/* READ */
	lock(&m);
	reading--;
	cond_broadcast(&cv);
	unlock(&m);
}

write() {
	lock(&m);
	while (reading || writing) cond_wait(&cv, &m);
	writing++;
	/* WRITE */
	writing--;
	cond_broadcast(&cv);
	unlock(&m);
}
```

Even though we unlock the mutex before reading, we know that once the writer returns from `cond_wait`, it'll see that a reader is working in the while loop and sleep again.
# Starving Writers

The above solution overwhelmingly favors readers to writers, so constant read requests will force a writer to wait, as `reading` will never equal zero. This is a phenomenon known as **starvation**, which we can fix by bounding the wait for the writer. Once a writer arrives, all future readers will wait until the writer has finished, then only continue through the rest of the loop. Below is a complete, albeit unoptimized, solution:

```c
read() {
	lock(&m);
	while (writers) cond_wait(&cv, &m);
	reading++;
	unlock(&m);
	/* READ */
	lock(&m);
	reading--;
	cond_broadcast(&cv);
	unlock(&m);
}

write() {
	lock(&m);
	writers++;
	while (reading || writing) cond_wait(&cv, &m);
	writing++;
	unlock(&m);
	/* WRITE */
	lock(&m);
	writing--;
	writers--;
	cond_broadcast(&cv);
	unlock(&m);
}
```