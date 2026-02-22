Race conditions are whenever the outcome of a program is determined by how the OS schedules the operations, which means it's nondeterministic from the scope of the programmer and the output won't be easily reproducible.

For example, if we create two threads that take a pointer to an integer, adds it to itself, then writes that value back to the pointer, then the code could either run sequentially and quadruple the number or run in parallel and double it, or some other combination of loads, adds, and stores. With compiler optimizations, the assembly instruction turns out to be a single line, but the hardware itself may experience a race condition. We can avoid that by using the `lock` prefix, but we want a solution in C.
# A Day at the Races

Another small race condition can occur when we're making threads inside a loop and passing in the reference to the index variable (like `i` or `j`) as an argument, since we could be moving to the next iteration and updating the variable before the thread finishes. To overcome this race condition, we can either create a `struct` holding the necessary information for the thread and pass that in, or just cast `i` as a pointer, then recase as an integer inside the thread.

Race conditions don't have to be explicitly in our code, but instead in functions that aren't thread-safe, like `asctime`, `getenv`, `strtok`, or `strerror`. Functions that create static buffers and don't recognize that they're stored in global memory once aren't thread-safe as two threads shouldn't use it at the same time.
# Don't Cross the Streams

Programs with multiple threads can still `fork`, but the child process will only have a single thread that's a copy of the thread that called fork, terminating all the other threads and potentially leaving resources in an unusable state (permanently locked mutexes, etc). If we really wanted to do this, we could set handlers for `fork` by using `pthread_atfork`.
# Embarrassingly Parallel Problems

An embarrassingly parallel problem is one where there's very little effort involved to turn the algorithm parallel. A lot of them require some synchronization concepts, but not all. Usually, embarrassingly parallel problems are those where the operations or even the data structures themselves are split into independent chunks for some portion of time. For example, mergesort can be turned parallel by creating threads for each half of the input array that call mergesort again, turning it into an $O(\log^3(n))$ operation assuming effectively infinite CPU cores. We can do this complexity analysis by using Amdahl's law, which states that the speedups gained from parallelizing are proportional to the non-parallel section. To speed up mergesort in practice, we'd often ditch the thread call if the array gets small enough to make use of the cache, and make a worker pool of threads as CPUs don't have infinite cores.

Another embarrassingly parallel problem is applying a function to each element in an array, which we can solve by creating a number of threads less than the number of CPU cores such that each thread is allotted a nonempty subarray and is tasked with applying the function to every element in that.
# Other Problems

Other embarrassingly parallel problems are serving static files on a web server to multiple users at once, calculating points in the Mandelbrot set or Perlin noise, rendering computer graphics, cryptographic brute-force searches, cryptocurrency proof-of-work systems, multiple search queries, large scale facial recognition systems, simulations of multiple independent scenarios, computing meta-heuristics for evolutionary algorithms, weather prediction calculations, the marching squares algorithm, the sieving step of the quadratic or number field sieve, the tree growth step of random forests, and the discrete Fourier transform.
# Lightweight Processes

We can create threads like processes as follows:

```c
#define STACK_SIZE (8 * 1024 * 1024)

int thread_start(void *arg) {
	puts("Hello Clone!");
	return 0;
}

int main() {
	char *child_stack = malloc(STACK_SIZE);
	char *stack_top = child_stack + STACK_SIZE;
	
	pid_t pid = clone(thread_start, stack_top, SIGCHLD, NULL);
	
	if (pid == -1) {
		perror("clone");
		exit(1);
	}
	
	printf("Child pid %ld\n", (long) pid);
	
	if (waitpid(pid, NULL, 0) == -1) {
		perror("waitpid");
		exit(1);
	}
	
	return 0;
}
```

It seems simple, but we did need a lot more boilerplate code and it isn't bound to the POSIX standard like `pthread` is, so there'd be a decent amount of undefined behavior, plus `pthreads` let us customize our thread with options. The only real reason to use `clone` is to gain the small amount of functionality lost through the `pthread` abstraction.