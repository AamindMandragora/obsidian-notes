A signal allows one process to asynchronously send an event or message to another process, which can choose to accept it and decide what to do with it.

A signal disposition is a per-process attribute that determines how a signal is handled after it's delivered, like a table of signal-action pairs. The actions are `TERM`, `IGN`, `CORE` (dump), `STOP`, `CONT`, or execute a custom function. A signal mask determines whether a particular signal is delivered or not.

The process by which the kernel sends a signal is as follows:

1. If no signals have arrived, the process can install its own signal handlers.
2. A signal starts in a generated state and moves to a pending state while waiting for the kernel.
3. The kernel checks the process' signal mask, and if it says that all the threads in a process are blocking the signal, then the signal is blocked until unblocked by a thread.
4. If a single thread can accept the signal, then the kernel executes the action in the disposition table. If the action is a default action then no threads need to be paused.
5. Otherwise, the kernel delivers the signal by stopping whatever a certain thread is doing and jumps it to the signal handler, putting the signal in the delivered phase. More signals can be generated but none can be delivered until this signal leaves the delivered phase.
6. If the process remains intact after delivery, the signal is considered caught, and killed otherwise.

Here's a table of common signals

| Name      | Portable Number | Default Action       | Usual Use                        |
| --------- | --------------- | -------------------- | -------------------------------- |
| `SIGINT`  | 2               | `TERM` (catchable)   | Stop a process nicely            |
| `SIGQUIT` | 3               | `TERM` (catchable)   | Stop a process harshly           |
| `SIGTERM` | 15              | `TERM`               | Stop a process even more harshly |
| `SIGSTOP` | N/A             | `STOP` (uncatchable) | Suspends a process               |
| `SIGCONT` | N/A             | `CONT`               | Starts a suspended process       |
| `SIGKILL` | 9               | `TERM` (uncatchable) | Eviscerate process               |

`SIGKILL` is typically overkill for most cases, and we should only ever use it when we need the process gone and we can't afford to clean its resources up.