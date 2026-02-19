The man (manual) pages are the documentation for C system calls (s2) and library functions (s3).
# Handling Errors

C handles errors through return instead of creating an exception like C++ or Java as they're easier to understand, don't require stack traces or jump tables, and aren't complex objects. However, exceptions can come from several layers deep, reduce global state, and differentiate business logic and control flow, so they have their uses. C uses return errors to have backwards compatibility with older languages like FORTRAN.

Each thread in a process gets a copy of `errno`, stored on the top of each thread's stack. If a called function could return an error, the programmer has to manually check `errno`, or print out the description using `perror`. Functions can also return the error code
# Input / Output

Every process can read and write from three streams of data: standard input (with file descriptor 0), standard output (with file descriptor 1), and standard error (with file descriptor 2). Usually, standard input and output are sourced from and to the terminal running the program, but a programmer can alter that behavior to send and receive input and output to and from files or other programs.

Standard output (`stdout`) oriented streams can only write to `stdout`, and the most common function that does that is `printf`, which takes a format string including placeholders, then a number of arguments that correspond to each placeholder. We can specify string pointers (`%s`), integers (`%d`), or even memory addresses (`%p`), and the data will get buffered until the cache is full or a newline is printed for performance. `printf` is a library function that calls the system call `write` from the previous section.

There are three types of streams in terms of buffering: unbuffered (the contents reach the file as soon as possible), line buffered (the contents reach the file as soon as a newline is provided), and fully buffered (the contents reach the file as soon as the buffer is full). Usually, `stderr` will be unbuffered, and `stdin`/`stdout` will be line buffered in a terminal and otherwise fully buffered. These rules determine the buffering of `printf` and other related functions or system calls, but we can force a flush (emptying of the buffer into the file) at any time by calling `fflush()` on the stream. Printing single strings and chars can be done using `puts()` and `putchar()`.

Printing to other streams can be done by using `fprintf` (for `FILE` pointers) or `dprintf` (for file descriptors). To print into a C string, use `sprintf` for static or bounded numbers of characters and `snprintf` (which returns the number of bytes written to) for variable numbers of characters.

# `stdin` Oriented Functions

Standard input (`stdin`) oriented functions read from that stream directly, but most (like `gets`) have been depreciated due to poor design. Functions we can use are `fgets` (for statically allocated buffers) and `getline` (to dynamically allocate and reallocate the buffer to be large enough).

To parse input in addition to reading it, use `scanf` (or `fscanf` for non standard input streams, or `sscanf` for a string), which all return the number of parsed items. `scanf` will keep reading until the string ends, so to prevent a buffer overflow, put the max number of characters (`sizeof(buffer) - 1`) before the "s" in the format specifier. The `scanf` family is much more expensive than system calls are, so use them sparingly or not at all for high performance programs.
# `string.h`

`string.h` is a library of functions that deal with the manipulation and checking of pieces of memory, usually C strings (a series of bytes delimited by the NUL character `'\0'`), and any behavior not mentioned in the man pages is undefined.

`int strlen(const char *s)` returns the length of the string.

`int strcmp(const char *s1, const char *s2)` returns an integer denoting which of the two strings comes lexicographically first (zero if they are the same).

`char *strcpy(char *dest, const char *src)` copies the string at `src` to `dest` assuming there's enough space, otherwise undefined behavior.

`char *strcat(char *dest, const char *src)` concatenates the string at `src` to the end of `dest` assuming there's enough space.

`char *strdup(const char *dest)` duplicates the string on the heap and returns a pointer to the copy.

`char *strchr(const char *haystack, int needle)` finds the first occurrence of `needle`'s corresponding character in `haystack`.

`char *strstr(const char *haystack, const char *needle)` finds the first occurrence of the substring `needle` in `haystack`.

`char *strtok(const char *str, const char *delims)` replaces the first instance of a character in `delims` in `str` with `'\0'`, effectively splitting the original string. It will never return an empty string but will instead use a `NULL`.

`[long] long int strol[l](const char *nptr, char **endptr, int base)` takes the pointer to a string `nptr` and a `base` (binary, decimal, hex), and an optional `endptr` and returns the parsed value, or `0` on failure. In that case, we must save the previous `errno`, then call `strol` to check if `errno != 0`, in which case an error occurred.

`void *memcpy(void *dest, const void *src, size_t n)` copies the first `n` bytes of `src` to `dest`, but when the two regions overlap the behavior is undefined and Valgrind won't be able to help either.

`void *memmove(void *dest, const void *src, size_t n)` does the same thing as `memcpy` but guarantees correct byte copying even if the regions overlap.