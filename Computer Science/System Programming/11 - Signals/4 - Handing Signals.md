Most library and system calls are `async-system-unsafe`, meaning they shouldn't be used in a signal handler because they are not re-entrant. A re-entrant function will succeed no matter how many times it's frozen and resumed. Examples of functions that aren't re-entrant are those with shared buffers, synchronization primitives, or dynamic memory allocation (like `printf`).

However, say we still want to use these functions. If we can't put them in the signal handler, we can use a flag that's set in the signal handler and repeatedly checked in the main program to determine whether we should execute the normal code or the signal code. However, compiler optimization means that a flag can get optimized to a hardcoded value, which means setting it won't actually change the conditional. We can avoid this by using the `volatile` keyword. Secondly, it would be bad if our flag only got changed halfway due to caching, so we make its type `sig_atomic_t`. Reading from and writing to it are both now uninterruptible. `sig_atomic_t` can be as small as a `char` on embedded systems and as large as an `int` on modern platforms.

We can either asynchronously handle signals with `sigaction` or synchronously catch them with `sigwait`.
# `sigaction`

`sigaction` is more consistent and portable than `signal`, and also works better with threads. We can use it to get or set (as seen below) the current handler for a particular system.

```c
struct sigaction sa;
sa.sa_handler = myhandler;
sigemptyset(&sa.sa_mask);
sa.sa_flags = 0;
sigaction(SIGALRM, &sa, NULL);
```

We can also set the mask field using `sigaddset`, `sigfillset`, or `sigdelset`, and provide flags like `SA_RESTART`, which automatically restarts system calls that get interrupted. However, it's often better to have the code check for a `EINTR` to be safe.