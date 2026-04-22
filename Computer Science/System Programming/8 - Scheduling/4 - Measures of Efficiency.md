We define `start_time` and `end_time` as the wall-clock start time and end time of the process, `run_time` as the total CPU time required, and `arrival_time` as the time at which the process enters the scheduler.

We can measure efficiency by the turnaround time (`end_time - arrival_time`), the response time (`start_time - arrival_time`), or the wait time (`end_time - arrival_time - run_time`).
# Convoy Effect

When a process takes up a lot of the CPU time, all other processes (which may be less resource-intensive, usually i/o-bound) follow like a convoy behind it. This reduces the overall system speed and throughput (number of processes per unit of time).