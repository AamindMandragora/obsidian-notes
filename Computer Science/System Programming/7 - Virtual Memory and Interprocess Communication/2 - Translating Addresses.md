Consider a $32$-bit machine, which means addresses are $32$ bits and can map to $2^{32}$ different bytes, which is equivalent to four gigabytes of memory. If we had a large table storing every possible physical address, they'd each need four bytes to hold the $32$-bits. This would require sixteen billion bytes in total, which is far too much. Instead, we can chunk memory into regions, and have a table to look up those regions.
# Terminology

A **page** is a block of virtual memory, typically 4KiB or $2^{12}$ bytes. For a $32$-bit machine, there are $2^{32}/2^{12}=2^{20}$ pages of this size. A **frame** is a block of physical memory (RAM), the same size as a page. **Page tables** map a page number (virtual memory) to a frame number (physical memory). We'd need twenty bits, or $2.5$ bytes, to be able to number every frame. Rounding up to four bytes gives us 4 MiB needed to hold the entire page table for a process.

Since our page table maps to frames and not addresses, we can calculate the exact byte we need by using the extra $1.5$ bytes we added to store an offset. Once we need to dereference a virtual address, we can get the corresponding frame using the first twenty bits of the address and looking up the corresponding frame in the page table, then add the offset to the frame's base address to get our final physical address.

Changing up the sizes of the operating system and the page will give alternate ways to split an address into frame # + offset. For a 64-bit machine with 4KiB pages, we'd need $52$ bits to number every frame, which is around 40 petabytes of storage, entirely too large. On the other hand, 64 bits can address to an astronomical number of bytes, far more than will ever be needed, so we'll only end up using a small subset of the virtual memory.
# Multi-Level Page Tables

Multi-level page tables hold a list of pointers to the next level of tables. This means that, for a 32-bit address, we'd split it into some $n$ indexes and an offset. The MMU will take the top-level page table and find the entry corresponding to the first index, which will point to a sub-table. We then find the entry corresponding to the second index, which points to a sub-sub-table, and so on for every $i\text{-th}$ index until we reach a pointer to a frame, at which point we add the offset and perform the operation.

For a two-level page table, each index is ten bits wide to map to the $2^{10}$ sub-tables. Rounding up to two bytes, we'd need 2KiB to store the first-level page table. For processes with small memory needs, they really only require a sub-table for the heap (low addresses) and the stack (high addresses). If we make the second-level page tables the size of pages themselves, we'll only need 10KiB for the entire thing. The tradeoff is only being able to access 4MiB of memory, which is why read programs usually have more sub-table entries.
# Page Table Disadvantages

Single-level page tables require two memory accesses, which makes them twice as slow. This scales linearly with every extra level added. To overcome this, the MMU has an associative cache called the Translation Lookaside Buffer, queried every time a virtual address needs to be translated into a physical one in parallel to the page table. For programs with bad cache coherence, the TLB won't be very effective.
# MMU Algorithm

Assuming a single-level page table:

1. Receive address
2. Attempt to translate address
3. On fail, report invalid address
4. On success:
	1. Get the frame from the TLB if it's contained and perform the operation
	2. Otherwise, if the page exists in memory, ensure the permissions agree with the operation
		1. If so, perform the operation and cache results
		2. If not, trigger a hardware interrupt (SIGSEGV)
	3. Otherwise, if the page doesn't exist in memory, generate an interrupt
		1. If the page fits the mapping, allocate the page and retry
		2. Otherwise, invalid access (SIGSEGV)
# Frames and Page Protections

Since frames can be shared between processes, we can use them to communicate with processes. Page tables also store whether a process can write a specific frame or just read. If a frame is read-only, like the C-library instruction code, it can safely be shared between multiple processes. If a program tries to write to one of these frames, it will SEGFAULT.

We can also use the `mmap` system call to tie virtual addresses to something other than a physical frame and allow parent processes to talk to their children. It won't work for communicating with other hardware. The permission bits for a frame are: read-only, execution (do the bytes form an executable program), dirty (data has changed since read), etc.
# Page Faults

When a program accesses an address in a frame missing in memory, a page fault occurs. If the address is valid but lacks a mapping, we call it **minor**. If the mapping is exclusively on the disk, it's called **major**, and the OS makes the page and another page in memory switch places. If the address itself isn't valid, it's called **invalid** and the OS generates a SIGSEGV.
# Link Back to IPC

Now that we know how processes are isolated, there are two ways to connect them: either ask the kernel for an interface, or ask it to map two pages of memory to the same virtual area and handle the synchronization yourself.