If we're given a list of commands to `exec`, we can schedule them using `SIGSTOP` and `SIGCONT`, making a user space scheduler. At the kernel level, the general flowchart is below:

1. The initial state is telling the operating system it needs to create a new process since a process has been requested to schedule (coming from `fork` or `clone`).
2. Once the process is created, it moves to the ready state (all structs in kernel are allocated), from which point it can go into ready suspended or running states.
3. The running state is when the process is doing useful work. From here, a process can either terminate (finish), be blocked (waiting on a mutex lock or sleeping), or be preempted (brought back to ready).
4. Within the blocked state there are blocked ready and blocked suspended states, but we don't have to worry about

We will try picking a scheme that decides when processes move between the ready and running states, without mentioning voluntarily blocked or deep slumber states.