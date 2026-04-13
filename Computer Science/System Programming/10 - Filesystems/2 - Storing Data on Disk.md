A **disk block** is a portion of the disk reserved for storing the contents of a file, an **inode** *is* a file or directory containing metadata and pointers to disk blocks, and a **superblock** contains metadata about the inodes and disk blocks. Modern filesystems contain several superblocks and one supersuperblock, which helps with fragmentation.

A block can be thought of as a contiguous region on disk whose size is usually determined based on the size of a page of memory for cacheable data. The superblock is a known location on disk since otherwise the computer may fail to boot. For any usable file, we need to have the name, size, time created / modified / accessed, permissions, path, checksum, and data.
# File Contents

The superblock may store an array of index nodes, aka inodes, which store direct and indirect pointers to disk blocks. Most filesystems have a limit on how many inodes, and therefore files, can exist. We must understand that the file name is merely a field of a file (albeit usually stored in the directory), and the actual file *is* the inode. We can read or write the first few bytes of the file by following the first direct block pointer. If the size of the file isn't a multiple of the size of a direct block, then there will be some garbage data at the end. If a file is bigger than the maximum space addressable by its direct blocks, we can simply introduce **indirect blocks** that store pointers to more data blocks, exponentially increasing the address space while linearly increasing the time for a search.
# Directory Implementation

A directory is a mapping of names to inode numbers, typically a normal file but with some special bits set in the inode and a specified structure. The `ls`executable simply reads that directory file to get **dirent**s. Since inodes have unique numbers, a filename can exist in multiple directories and a file can have multiple names.
# UNIX Directory Conventions

In UNIX, `.` represents the current directory and `..` represents the parent directory, but there's no extension to this aliasing (except in `zsh`). `~` is usually expanded to the home directory, files starting with `.` are treated as hidden, and files starting with the NUL byte are usually abstract UNIX sockets and are hidden by anything not explicitly looking for them.
# Directory API

We typically use `open`, `read`/`write`, and `close` to interact with files, but directories have special calls `opendir`, `readdir`/`mkdir`, and `closedir`. We can get `dirent`s from a `DIR*` using `readdir`, but we should always make sure to call `closedir` once we're done. Either parent or child can use `readdir`, `rewinddir`, or `seekdir`, but both using them is undefined.

`readdir` will return both the parent and current directory along with all other `dirent`s, and search can take a while without excluding subdirectories. Also, `readdir` isn't thread-safe.
# Linking

We know now that a filesystem holds directory inodes, which can hold more directory inodes, and so on until we reach file inodes. This suggests a tree-like structure. However, we can create links to inodes in different directories, which means an inode can have more than one parent directory, which requires a graph. The two types of links are hard and soft. 

Hard links are simply entries in directories assigning some name to an inode with a pre-existing entry in another directory. This means that both entries point to the same file, so updating one updates the other. We can use `link` in `C` or `ln` in the shell to create a hard link.

Soft links, also known as symbolic links, are files storing a path to another file and a special bit set denoting their status. The advantages are that they can refer to files and directories that are outside the current file system or even don't exist. The tradeoff is being slower than regular files since an additional call to open and read is necessary.

POSIX forbids hard linking directories as otherwise verifying the directory structure is an acyclic tree reachable from root becomes expensive. If that assumption is broken, the system may be unrepairable.

When we remove files using `rm` or `unlink`, we're really just removing a reference to an inode. However, that inode may still be referenced by other directories, and it keeps a count of how many places reference it (not counting symlinks). Hard links can efficiently create archives of a file systems, incrementally backing it up like Apple's Time Machine.
# Pathing

A path is a sequence of directories that form a "path" in the filesystem graph. However, they can also include the special characters `.` and `..`, which makes them hard to read. C gives us `realpath` which can compress these paths, but we can also simplify by hand if we remember that `..` means parent directory and `.` means current directory.
# Metadata

Since directories are also inodes, how can we distinguish between them and regular files? We store that, along with other things like file extensions and modification times, inside fields within the inode. We can access them using the `stat` function (there's also an `fstat` for file descriptors and an `lstat` for symlinks). Here are the fields:

```c
struct stat { 
	dev_t st_dev; /* ID of device containing file */ 
	ino_t st_ino; /* Inode number */ 
	mode_t st_mode; /* File type and mode */ 
	nlink_t st_nlink; /* Number of hard links */ 
	uid_t st_uid; /* User ID of owner */ 
	gid_t st_gid; /* Group ID of owner */ 
	dev_t st_rdev; /* Device ID (if special file) */ 
	off_t st_size; /* Total size, in bytes */ 
	blksize_t st_blksize; /* Block size for filesystem I/O */ 
	blkcnt_t st_blocks; /* Number of 512B blocks allocated */ 
	struct timespec st_atim; /* Time of last access */ 
	struct timespec st_mtim; /* Time of last modification */ 
	struct timespec st_ctim; /* Time of last status change */ };
```

For example, we can use the `st_mode` field and the macro `S_ISDIR` to figure out if an inode is a directory or not.