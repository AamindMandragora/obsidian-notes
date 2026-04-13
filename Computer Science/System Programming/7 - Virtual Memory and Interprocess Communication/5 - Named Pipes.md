We can use `mkfifo` to create a named pipe by giving it the pathname and the operation mode. All it does is creates an unnamed pipe that refers to the named pipe, which means it takes up almost no space in a file system. We can then use the name to refer to it in processes that aren't connected in any way.
# Hanging Named Pipes

A named pipe created using `mkfifo` is a pipe that a program calls `open` on with read and/or write permissions. However, reads and writes hang until there is at least one reader and one writer.
# Race Condition with Named Writes

If you open a pipe under both permissions in one process, it won't wait for the second process to open it and will start performing operations. In some cases, it may even close the pipe before the second process opens it, causing an infinite hang.