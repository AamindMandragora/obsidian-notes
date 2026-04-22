We'll assume Process $1\leq k\leq 5$ takes $k$ seconds ($1000k$ milliseconds).
# Shortest Job First

At any given point when a CPU is free, this scheduler will choose the job with the shortest total CPU time. The only problem is it's hard to know how long a program will take before running it. In the real world, we use the expected burst time (number of CPU cycles needed to finish a program), which can be estimated using an exponentially decaying weighted rolling average based on previous burst time.

Shorter jobs tend to get run first, decreasing wait and response times. However, this algorithm needs to be effectively omniscient and good at estimating the burstiness of a process which is very difficult.
# Preemptive Shortest Job First

This scheduler is like SJF except at any point, if a new job enters with shorter runtime than the total runtime of any current job, it preempts that job. If they're equal, it's up to the algorithm. There's a variant of PSJF called Shortest Remaining Time First which compares the runtime of a new job to the remaining runtime of any current job.

This ensures the shorter jobs always run first, but we still need to know the runtime and we're now context switching more often, interrupting jobs.
# First Come First Served

Processes are simply scheduled in order of arrival. This algorithm is very simple, but suffers from the Convoy Effect if a long running process enters the queue right before many short processes. Context switches will be less frequent, and there's no starvation if all processes are guaranteed to terminate.
# Round Robin

This is similar to FCFS except processes can only occupy a CPU for at most a predetermined interval called the **time quanta** before being replaced by another waiting process if any exists. This ensures long-running processes don't starve all other processes. RR will be equivalent to FCFS as the quanta approaches infinity. The only real disadvantage is a large amount of context-switching if there's a lot of processes.
# Priority

We assign each process some priority value, and processes are scheduled based on that. For example, a navigation process might be more important than a logging process, so this algorithm ensures that process will be scheduled first.