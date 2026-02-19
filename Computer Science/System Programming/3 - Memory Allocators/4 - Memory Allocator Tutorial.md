A memory allocator needs to keep track of which bytes are currently allocated and which are available, and the simplest way to do that is to create a linked list of blocks with metadata holding information about the block and a boundary tag to denote where it ends. If we make the metadata and boundary tags structs, then we have implicit pointers from block to block. This means that we can just add the size of the block to its pointer to get the pointer of the next block. A minimal metadata struct would just be the size of the block, but even with that, we'll still need more memory from the system than the user explicitly requested.

This makes our heap memory a list of blocks where each block is either allocated or unallocated. If our metadata struct is as follows:

```c
typedef struct {
	size_t block_size;
	char data[0];
} block;

block *p = sbrk(100);
p->size = 100 - sizeof(*p) - sizeof(BTag);
```

then we can move from one block to the next by adding the block's size, like this:

```c
block *next = (char*)p + sizeof(metadata) + p->block_size + sizeof(BTag);
```

If we don't cast right, then we could end up moving many times more bytes over than we intended, since `p += 8` adds `8 * sizeof(p)`, which isn't always eight bytes.
# Implementing a Memory Allocator

The simplest implementation has us start at the first block if it exists and iterate until we find an unallocated block of sufficient size or we've checked all the blocks, at which point we call `sbrk` to extend the heap. When a free block is found, we create an entry each for the allocated block and the remaining space. We can define the following structs to form our block metadata:

```c
typedef struct {
	unsigned int block_size : 7;
	unsigned int is_free : 1;
} size_free;

typedef struct {
	size_free info;
	char data[0];
} block;
```

The first struct allocates seven bits out of the int to represent the block size, and the last bit to denote whether it's free or not.
# Alignment and Rounding Up

Many architectures expect multibyte primitives to be aligned to boundaries of the same size as them, and if they aren't, performance can be impacted in the best case and the program can crash in the worst. Since `malloc` doesn't know what the type of the pointer it's allocating for is, it needs to be aligned to the largest possible multibyte primitive. Addresses are multiples of eight on most systems and sixteen on 64-bit systems. In C, we can calculate the number of 16-byte units by doing `(requested_bytes + tag_overhead_bytes + 15) >> 4`.

If the given block is larger than the allocation size, then we'll run into internal fragmentation, which is why we often implement both block coalescing and block spitting.

# Implementing `free`

When `free` is called we need to subtract `sizeof(size_free)` from the given pointer to get back to the real start of the block, and then set `p->info.is_free = 0;` to free the block. To really optimize `free`, we need to check if the previous and next blocks are free too, in which case we can coalesce them into a single large block.

To coalesce with the previous block, we have to be able to find it, so we store the block's size on both ends of the block. These are called boundary tags and were invented by Knuth as implicit double pointers, allowing us to jump backwards and forwards at will.
# Performance

Using the above information, we can finally build a memory allocator. It'll be really simple, where `malloc` is worst-case $O(n)$ and free is constant time. We'll only need to coalesce three blocks at a time, and we only need a single pointer to track the MRU block. We can now use this allocator to start to experiment with different placement strategies, like next fit.
# Explicit Free Lists Allocators

If we implement an explicit doubly-linked free list, we can achieve better performance, immediately traversing to the previous and next free blocks, reducing the search time by ignoring all the allocated blocks. We can also control how our linked list is ordered, inserting recently freed blocks to the head instead of always in the middle. Our struct would then look something like this:

```c
typedef struct {
	size_free info;
	block* next;
	char data[0];
} struct block;
```

We can store the pointers of our linked list inside the free blocks as long as we ensure they'll always be big enough. We also have to implement boundary tags for block coalescing. We'll find the first sufficiently large free block to allocate during `malloc`, but the way we do link order determines which placement strategy we end up using.

We can either insert the newly deallocated block in the beginning or in address-order in the free list. The former case creates a LIFO policy, reusing the MRU deallocated spaces and making fragmentation worse. The latter case makes `free` take more time but reduces fragmentation.