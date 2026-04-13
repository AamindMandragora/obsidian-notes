A **filesystem** satisfies the API of a filesystem, is backed by a storage medium (HDD, SSD, RAM, etc.), and tells the OS how to organize the bits in the storage medium to store file and directory information. Usually, operating systems abstract the filesystem so that introducing a new system call doesn't require changes to all filesystem drivers.

We must recognize the difference between \*B and \*iB. The former uses powers of ten for their prefixes (KB = 1000B, etc.) and the latter uses powers of two (KiB = 1024B, etc.). When files are displayed in the operating system, they typically use KB to mean KiB due to a clash between storage developers (who preferred iB) and network developers (who preferred B).

Recall the Unix proverb that everything is a file. File-like objects must present themselves to the file and support common filesystem operations (`open`, `read`, `write`, `close`). Filesystems deal both with local files and special devices, handle failures, scalability, indexing, encryption, compression, performance, and the abstraction between a file that contains data and the storage of data on disk.

Linux systems typically use several filesystems: `ext4` for files whose path starts with `/`; `procfs` for `/proc` files; `sysfs` for `/sys` files; `tmpfs` for `/tmp` files; `sshfs` for syncing files across `ssh`. Filesystems like `procfs` are referred to as **virtual filesystems** since they don't provide traditional data access.
# The File API

Filesystems must provide callback functions for actions like `open`, `read`, `write`, `close`, `chmod`, and `ioctl`, which interacts with parameters of devices like terminals. Some of these are omitted, and many aren't seekable and provide exclusively sequential access to files.