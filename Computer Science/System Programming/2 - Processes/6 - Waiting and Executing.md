If the parent process wants to wait for the child to finish, it must use `wait` / `waitpid` which wait until a child changes process states (termination, stopped / resumed by a signal). `waitpid` is normally blocking, which means it stops the parent from executing commands until that state change, but it can be set to be non-blocking using the `WNOHANG` flag. `wait` takes a pointer to a status integer and waits on any child process, returning when the first one changes state. `waitpid` takes the PID of the specific process it wants to wait on, a pointer to a status integer, and an option parameter where we can set flags like `WNOHANG`, `WNOWAIT` (which leaves the child wait-able by another wait call), `WEXITED`, `WSTOPPED`, and `WCONTINUED`.
# Exit Statuses

To find the return value of the child process, we can use the `WIFEXITED` and `WEXITSTATUS` macros, which take the status integer passed in `waitpid` as their single argument. `WIFEXITED` returns a boolean value denoting whether the child process exited or returned normally, and `WEXITSTATUS` returns the integer that was returned. There are only 256 return values for a process, and convention defines an exit value of zero to mean a successful process. Beyond that, it's up to the programmer to define specific return codes for the scope of their program.
# Zombies and Orphans

If a parent doesn't wait on its child, the latter becomes a zombie process that takes up space in the kernel process table, which keeps track of the PID, status, and how it died. Too many zombies may render the parent unable to fork, and the only way to get rid of one is to wait on the children.

This doesn't mean that a program always needs to wait for its children. If it dies without waiting, those children become orphans and are adopted by `init`, the first process with PID 1 that automatically waits for all its children, so the orphans would only become a zombie for a little while after finishing and then get removed from the system.
# Asynchronously Waiting

We know parents get `SIGCHLD` when one of its children completes, so we can create a handler that catches that signal, then waits on the child, which means the parent can do stuff while the child executes and the kernel handles transferring control to the handler when signaled. However, more than one child may finish per `SIGCHLD`, and they can be sent for other reasons (like a temporary stop). Better code that asynchronously waits is written below.

```c
void cleanup(int signal) {
	while (waitpid((pid_t)(-1), 0, WNOHANG) > 0);
}
```

This continually reaps child processes without blocking until there are none left, in which case the condition returns a nonpositive number.