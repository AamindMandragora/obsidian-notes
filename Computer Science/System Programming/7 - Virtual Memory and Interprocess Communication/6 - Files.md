On Linux, there are two abstractions with files. The first is file descriptors: `open` takes a path to a file and creates a fd entry in the process table, erroring out if the file is inaccessible; `read` takes a certain number of bytes received by the kernel from the fd and attempts to read them into a buffer; `write` attempts to output a certain number of bytes to a fd and may be buffered; `close` removes a fd from the process table; `lseek` takes a fd and sets the file pointer for the next `read`/`write`; `fcntl` can set file locks and permissions.

The other abstraction is the `FILE*`, which we use for portability. The corresponding functions are `fopen`, `fread`, `fwrite`, and `fclose`. We can also `fgetc`/`fgets` to get a char or string from a file, read/write a format string from a file using `fscanf`/`fprintf`, `fflush` any buffers, get a boolean representing whether we're at the EOF using `feof`, and set buffering using `setvbuf`. We can convert between the two using `int fileno(FILE* stream)` and `FILE* fdopen(int fd)`.
# Determining File Length

For files smaller than the max value of a `long`, we can use `fseek` to move the file pointer to the end, then `ftell` to get the current position.
# Use `stat` Instead

However, that only works on some architectures and compilers. We can use `stat` as follows to get the size of a file: 

```c
struct stat buf;
if (stat(filename, &buf) == -1) return -1;
return (ssize_t)buf.st_size;
```

where `buf.st_size` is of type `off_t` (offset type).
# Gotchas with Files

Closing a file stream is unique to each process, which means child processes have a copy of the file descriptor with the old file pointer. We should always try to not trigger a cache inconsistency and close before forking.