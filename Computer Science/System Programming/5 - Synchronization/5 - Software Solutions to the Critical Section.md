We already know that some parts of our code are classified as critical sections and should only ever be touched by one thread at a time. We implement this by wrapping each critical section in a mutex lock and unlock. How would we implement these functions? Can we do it with pure software while still assuring mutual exclusion?

Our first attempt looked something like this:

```c
lock(mutex *m) {
	while (m->lock);
	m->lock = 1;
}

unlock(mutex *m) {
	m->lock = 0;
}
```

However, even ignoring the fact that, using this implementation, threads can unlock locks they don't own, this doesn't satisfy mutual exclusion. Really, there are three main properties that we'll need for a solution to this problem: mutual exclusion, bounded waiting, and constant progress.
# Naive Solutions

Consider the pseudocode above as part of a larger program with two threads. It clearly suffers from a race condition where two threads can read the other's flag as lowered and continue. If we swap the order of raising and waiting on flag, that problem goes away, but we'd then suffer from deadlock if both threads raise their flags before checking the other's.
# Turn-Based Solutions

If we instead create a counter to denote which thread's "turn" it is before doing critical section stuff, it satisfies mutual exclusion at the cost of continuous progress, where fast threads will have to wait on slow threads even if they're not in the critical section.
# Turn and Flag Solutions

If we combine the two approaches, raising a flag then waiting until its turn if the other thread's flag is raised before doing critical section stuff, it seems to work. Analysis of solutions like these is tricky, and even peer-reviewed papers contain incorrect solutions. However, since threads don't wait on the other thread's flag, two threads can end up running at the same time.