Here's a naive version of `malloc` and the corresponding `free`:

```c
void* malloc(size_t size) {
	void *p = sbrk(size);
	if (p == (void*)(-1)) return NULL;
	return p;
}

void free() {}
```

However, each `malloc` becomes a system call, which are much slower than library calls, which means it'd be ideal to reserve a large amount of memory initially and occasionally ask the system for more. Also, we never reuse the heap memory after we free it, which means a typical program would quickly exhaust all the available memory. We need an allocator that can efficiently use heap space for the general case.
# Placement Strategies

During program execution, memory is allocated and deallocated, but not necessarily in order, so there will be a gap in the heap memory we can reuse. If a new `malloc` request is executed, we can choose a freed block that's as small as possible while still being large enough (best-fit), a block as large as possible (worst-fit), or the first available large enough block (first-fit). If the block chosen is too large, we can choose to split it, which may lead to external fragmentation, where our freed blocks are too small to fit a `malloc` request despite that much memory being free in total. We could also choose not to break the block, leading to internal fragmentation, where there's unused allocated memory.
# Placement Strategy Pros and Cons

Heap allocators need to minimize fragmentation and run fast. There will also usually be lots of pointer manipulation and arithmetic. We can also only optimize for the general case, so we'll end up sacrificing specific edge cases, and even if we knew the requests in advance, choosing the best implementation is a known NP-hard problem.

It's not obvious how different strategies affect fragmentation. A mathematical one-shot approach finds that for first-fit, actual memory usage over ideal usage is about 1.7, and more than that for best-fit. In practice, it was found that the worst-case for best-fit is when we select a block that's almost the right size, but the remaining space after splitting is so small that it'll never get used. To avoid this, we could set a threshold for splitting, but the worst-case is very rare. When talking about first-fit, it's important to define first: first freed, first address, least recently used, etc. Address-ordered and LRU lists perform better than MRU. Under random workloads and in practice, both best and address-ordered first do equally well with a splitting threshold and coalescing.

Best-fit may take less time than a full heap scan if we find a perfect block early, and so does worst fit if we use a regular max-heap to hold memory blocks. First-fit needs to have an ordering in blocks, and we mostly default to linked lists. If we're doing address-ordering, we can use a randomized skip-list as well to speed up insertion from $O(n)$ to $O(\log n)$. Other placement strategies, like next-fit, which stores a global pointer to the last-placed memory and runs first-fit from there, exist.