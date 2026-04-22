After forking, the child process inherits a copy of the parent's signal handlers and its signal mask. However, pending signals for the child aren't inherited. `exec` will carry over the signal mask and pending signals.

Each thread has its own mask, and will inherit a copy of the calling thread's mask. The kernel treats processes as a collection of threads, each of which can have a mask and receive signals.

To block a signal in a multi-threaded program, we swap `sigprocmask` out for `pthread_sigmask`. To prevent asynchronous delivery, we block it in all the threads, usually by setting the signal mask before any new threads are created. `pthread_sigmask` includes the same `how` parameter that defines how to use the set.

If two or more threads can receive the signal, the choice of thread to interrupt is arbitrary, so we'll usually make sure no signal is handled by two threads that are active at the same time.

Even though we can't send a signal to a specific thread from the terminal, we can do it from within using `pthread_kill(tid, sig)`. `SIGKILL` will still kill the entire process. Signal disposition is per-process, not per-thread, meaning `sigaction` can be called from any thread.