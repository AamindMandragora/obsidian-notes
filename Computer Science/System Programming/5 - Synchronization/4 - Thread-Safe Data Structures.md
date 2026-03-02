We can use mutexes and synchronization primitives to make our data structures thread-safe as well. We'll mostly use mutexes because they're more semantically meaningful, but writing high-performance thread-safe data structures can be a lot more complicated.

For example, if we take the following standard stack:

```c
#define STACK_SIZE 100
int count;
double values[STACK_SIZE];

void push(double v) {
	values[count++] = v;
}

double pop() {
	return values[--count];
}

int empty() {
	return count == 0;
}
```

This is clearly thread-unsafe as two threads calling `push` or `pop` at the same time can mess up `count` or the last element of `values`. Therefore, we need to place a mutex around any call that accesses `count` or `value`. Here's a new, thread-safe stack:

```c
#define STACK_SIZE 100
int count;
double values[STACK_SIZE];

pthread_mutex_t mtx = PTHREAD_MUTEX_INITIALIZER;

void push(double v) {
	pthread_mutex_lock(&mtx);
	values[count++] = v;
	pthread_mutex_unlock(&mtx);
}

double pop() {
	pthread_mutex_lock(&mtx);
	double v = values[--count];
	pthread_mutex_unlock(&mtx);
	return v;
}

int is_empty() {
	pthread_mutex_lock(&mtx);
	int c = (count == 0);
	pthread_mutex_unlock(&mtx);
	return c;
}
```

There are a couple problems with this implementation. While `is_empty` is thread safe, there's no guarantee that it returns before another `push` or `pop` operation, which is why functions that return sizes are deprecated or removed in thread-safe data structures. Also, there's no protection from over or underflow, which can be fixed using semaphores. Finally, the above implementation doesn't support the use of more than one stack, so we might create a `struct` like this:

```c
typedef struct {
	int count;
	pthread_mutex_t mtx;
	double *values;
} struct stack;
```

and define the member functions accordingly, calling `malloc` and `free` in the constructor and destructor.

We could also try using condition variables to solve the over and underflow problems, like this:

```c
void push(stack_t *s, double v) {
	pthread_mutex_lock(&s->mtx);
	while (s->count == s->capacity) { pthread_cond_wait(&s->cfull, &s->mtx); }
	s->values[(s->count)++] = v;
	pthread_cond_signal(&s->cfull);
	pthread_mutex_unlock(&s->mtx);
}

double pop(stack_t *s) {
	pthread_mutex_lock(&s->mtx);
	while (s->count == 0) { pthread_cond_wait(&s->cempty, &s->mtx); }
	double v = s->values[--(s->count)];
	pthread_cond_signal(&s->cempty);
	pthread_mutex_unlock(&s->mtx);
	return v;
}
```