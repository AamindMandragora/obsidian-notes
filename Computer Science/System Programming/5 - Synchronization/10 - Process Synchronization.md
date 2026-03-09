While there aren't race conditions within a process, that isn't the case if it interacts with the surrounding system. For example, if we write to a file with a parent and child process, we normally won't know which one writes first.
# Interruption

A more pressing concern is the fact that most system calls can be `interrupted` by the operating system, so if `write` fails, we could encounter data loss in the best case and corruption after a partial write in the worst.
# Solution

A process can create a shared mutex before forking through `mmap`, then use that inside a `write` wrapper function. There are advanced options that use shared memory, but this works too. Usually we try to avoid this problem entirely by writing to separate files, but this technique is nice to have, and that it extends to semaphores, condition variables, and barriers. Process synchronization means we don't have to worry about arbitrary memory addresses becoming race condition candidates, each process is isolated and fails alone, and eases the system load when a lot of threads are created.