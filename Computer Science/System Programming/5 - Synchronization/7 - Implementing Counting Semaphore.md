Now that we can make a mutex, we can start implementing other synchronization primitives, like semaphores. Since it's not easy to implement an $O(1)$ space complexity condition variable with only a mutex, we'll assume we already have one made. We also will assume the semaphore struct is already allocated, as we don't want to call `malloc` inside a primitive and risk deadlock. Our semaphore will look like this:

```c
typedef struct sem_t {
	ssize_t count;
	pthread_mutex_t m;
	pthread_condition_t cv;
} sem_t;

int sem_init(sem_t *s, int pshared, int value) {
	if (pshared) {
		errno = ENOSYS;
		return -1;
	}
	s->count = value;
	pthread_mutex_init(&s->m, NULL);
	pthread_cond_init(&s->cv, NULL);
	return 0;
}

void sem_post(sem_t *s) {
	pthread_mutex_lock(&s->m);
	s->count++;
	if (s->count == 1) pthread_cond_signal(&s->cv);
	pthread_mutex_unlock(&s->m);
}

void sem_wait(sem_t *s) {
	pthread_mutex_lock(&s->m);
	while (s->count == 0) pthread_cond_wait(&s->cv, &s->m);
	s->count--;
	pthread_mutex_unlock(&s->m);
}
```
# Other Semaphore Considerations

Usually, semaphores should strive to not keep a thread sleeping too long if possible, so they often use queues to ensure fairness. Our `pshared` argument, if implemented, would allow semaphores to be shared across processes through setting condition variable and mutex attributes.