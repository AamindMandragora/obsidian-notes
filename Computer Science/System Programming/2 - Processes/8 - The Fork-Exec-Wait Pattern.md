A common programming pattern is to call `fork` followed by `exec` in the child process and `wait` in the parent process, so we can have a process monitoring the child that can do other things after the `exec` call, like reading the output or executing another function.
# Environment Variables

The system keeps some variables for all processes to use, and we can use a `$` to access them. In C, we can call `getenv` and `setenv` to read and write to the environment. Environment variables are inherited between processes and can't be read from outside processes, which makes them safer.