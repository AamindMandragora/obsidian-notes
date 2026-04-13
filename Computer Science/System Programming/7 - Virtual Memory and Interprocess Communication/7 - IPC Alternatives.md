We only keep pipes around usually to redirect `stdin`, `stdout`, and `stderr` for collecting logs and other stuff. Usually we won't have to deal with processes communicating through pipes.

We use files for IPC almost all the time, like in Hadoop, where producers write to append-only tables and consumers read from them. If we want to save the intermediate results of an operation, or if putting it in memory would cause an out of memory error, we'll use files.

For linear read-throughs of a file or for direct memory IPC, we'll use `mmap`.