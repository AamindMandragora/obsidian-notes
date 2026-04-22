The software filesystem models don't effectively take advantage of the current hardware, which is getting better all the time. We might even work on a filesystem in the future, so let's explore a simple filesystem model for practice. We'll base it off of the first filesystem that Linux ran on, `minixfs`. It's laid out sequentially on disk, the first section being the superblock, storing important metadata about the filesystem. We keep it in the front because we need to read it before anything else. After the superblock comes a bitmap of inodes, where each bit is on iff the corresponding inode is being used. We then have a similar bitmap for data blocks. Finally, we have an array of inodes followed by the rest of the disk, implicitly partitioned into data blocks. Our inodes have an array of data block indices that act as direct pointers to data blocks, then an indirect pointer to another array of data block indices. We will assume a data block is $4KiB$ and that a file is compact (fills up each data block before requesting a new block).
# File Size vs Space on Disk

We have to store the file size in the inode, but the filesystem isn't aware of the contents of any file as that's user data. However, we can calculate upper and lower bounds using the number of direct and indirect blocks being used. The final block being used can have anywhere from $1B$ to $4KiB$ of data, which gives us our bounds. The **overhead** of storing a file on disk is simply the size of any indirect blocks being used, since they point to direct blocks and not actual data.

We can also calculate the minimum and maximum disk usage per file in the system. A file of size $0$ takes up no space on disk (besides the size of the inode, which is ignored since they're located in a fixed-size array somewhere else on disk). The smallest non-empty file has size $1B$, but requires an allocation of an entire data block, which is $4KiB$.

We can hold $1024$ data block numbers in one indirect block, implying that the maximum file size can be $2\cdot 4KiB+1024\cdot 4KiB=4MiB+8KiB$. However, there's still $4KiB$ of overhead, which means the total disk usage will be $4MiB+12KiB$. Without indirect blocks, our file size and disk usage would be the same thing. However, indirect blocks allow us to keep our inodes small and increase the space available for user data.
# Performing Reads

Say we want to read the entirety of some file. Then we first go to the inode, loop through the direct data blocks numbers, then use them as indices on the array defined by the pointer to the start of all data blocks and read all the bytes in those blocks. Once we're done, we read the indirect block, looping through every four bytes, checking if it can be a data block number, then access the data block and read the bytes until we have read enough.

To start the read at an offset of $n$ bytes, we calculate how many blocks we have to skip, then once we're reading the first block, we offset the pointer by the remaining offset.
# Performing Writes

We can either write to a file or to a directory. In the former case, we can do pretty much the same thing as a read, making sure to maintain a count of how many bytes we've written in case we have to cross data block boundaries.

A write to a directory implies that an inode needs to be added to a directory. Since we know we'll be adding at most one dirent at a time, we can simply find the last data block we have and write to the end of it or create a new data block if it's full.
# Adding Deletes

If the inode is a file, then we simply remove the dirent in the parent directory by marking it as empty (-1) and decrementing the hard link counter. Once it reaches zero, we simply free the inode and all associated data blocks. The filesystem can only delete empty directories.