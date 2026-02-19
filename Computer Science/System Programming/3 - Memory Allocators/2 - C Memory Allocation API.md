`malloc(size_t bytes)` is a library call that reserves a continuous block of memory that remains allocated until free is called on it. If `malloc` can't find enough free space (and sometimes even if that space exists), it will return `NULL`, which means all programs should check the return value. `malloc` also doesn't initialize the allocated memory, so the program shouldn't just read from it.

`realloc(void* space, size_t bytes)` allows a program to resize a memory allocation made by one of the `alloc` functions, and is usually used to implement dynamic arrays. Because there may not be extra space surrounding the extra pointer, `realloc` may return a pointer to a different location, and it can also fail if no space exists, and programs must always handle those two cases by calling `free` on the original pointer if `realloc` returns NULL and setting it equal to the returned pointer otherwise.

`calloc(size_t nmemb, size_t size)` takes the number of memory items and the size of each memory item, and allocates the corresponding number of bytes, then zeroes them all out. It's designed to be faster than calling `memset` after `malloc`, and while the arguments are swappable, follow the conventions.

`free(void* ptr)` makes the allocation being pointed to available for use again, and should be called whenever we're done using a piece of heap memory. Use-after-free is undefined.
# Heaps and `sbrk`

The heap is a part of the process memory that varies in size, and we allocate to it using the functions above. Initially, the program break pointer, which defines the end of the heap, points to the end of the data segment, but the program can move it based on the demand for memory using `sbrk`. The stack is the other part of the process memory that grows based on how many threads a program has and how many computations it does. Typically, the heap grows upwards, and the stack grows downwards towards the heap.

Nowadays, modern operating systems can request independent regions of virtual memory from the system, so memory allocators no longer need `sbrk`. Calling `sbrk(0)` can tell us the address at which the heap currently ends.

While memory that has already been allocated to the process has no guarantees regarding its value, memory directly obtained from the OS must be zeroes out or else the process could read the RAM of another process, a security leak. We can assume this when calling `sbrk` but never `malloc`.