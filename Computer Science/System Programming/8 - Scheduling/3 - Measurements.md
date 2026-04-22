Scheduling affects the **latency** (response/wait time) and **throughput** (bits per second, processes per unit of time) of the system. Different schedulers offer different tradeoffs between the two, and they have different use cases. There's no perfect scheduler for every possible task.

For example, some schedulers minimize total wait time across all jobs, but interactive environments prefer minimizing response time, even at the expense of throughput. Arrival time is the time at which a process first arrives at the ready queue, and is also the execution start time if there's an idle CPU.
# Preemption

Without preemption, processes run until they are unable to or have no further use for the CPU. Examples include the process being terminated by a signal, blocked in concurrency, or simply exiting. However, processes may never be scheduled if there are always more preferable jobs in the queue. With preemption, existing process may be removed immediately if there's a more preferable process waiting in the queue. If our scheduler prioritizes jobs with shorter execution time over all else, a process taking five milliseconds will always execute before a job taking ten milliseconds.
# Ready Queue

A process is placed on the ready queue when it can use a CPU. It may have just been created, or was blocked on a `read`, system call, or synchronization primitive and is able to continue now or has a signal whose handler needs to run.