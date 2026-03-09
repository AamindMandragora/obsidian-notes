A ring buffer is a simple, usually fixed-sized, array that's treated as if it was circular, with two index counters keeping track of the current beginning and end of the queue. Since array indexing isn't usually circular, the index counters must wrap around to zero. As data is enqueued and dequeued, the items in the buffer appear to circle the array. Here is a single-threaded implementation that doesn't guard against underflow or overflow:

```c
#define BUFFER_SIZE 16
void *buffer[BUFFER_SIZE];
unsigned int in = 0, out = 0;

void enqueue(void *value) {
	buffer[in++] = value;
	if (in == BUFFER_SIZE) in = 0;
}

void *dequeue() {
	void *result = buffer[out++];
	if (out == BUFFER_SIZE) out = 0;
	return result;
}
```

# Multithreaded Implementation

```c
#include <pthread.h>
#include <semaphore.h>
#define N 16

void *b[N];
int in = 0, out = 0;
mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
sem_t countsem, spacesem;

void init() {
	sem_init(&countsem, 0, 0);
	sem_init(&spacesem, 0, 16);
}

void enqueue(void *value) {
	sem_wait(&spacesem);
	mutex_lock(&lock);
	b[in++] = value;
	if (in == N) in = 0;
	mutex_unlock(&lock);
	sem_post(&countsem);
}

void *dequeue() {
	sem_wait(&countsem);
	mutex_lock(&lock);
	void* value = b[out++];
	if (out == N) out = 0;
	mutex_unlock(&lock);
	sem_post(&spacesem);
}
```