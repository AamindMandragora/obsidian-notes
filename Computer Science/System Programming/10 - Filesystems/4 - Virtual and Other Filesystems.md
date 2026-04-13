POSIX systems like Linux and Mac OS X include several virtual filesystems that are mounted as part of the system. Files within these virtual file systems may be stored in RAM or generated on the fly. In Linux, there are three main such virtual filesystems: `/dev`, a list of physical and virtual devices; `/proc`, a list of resources used by each process and set of system information; `/sys`, an organized list of internal kernel entries. Examples of virtual devices in dev are `/dev/zero` which generates an infinite stream of zeroes and `/dev/null` which discards everything placed in it.
# Managing Files and Filesystems

We can create a secure directory by `chmod 700`, which gives us full permissions and everyone else none. However, in between making a directory and changing the permissions, there's a small vulnerable window which can be exploited by a race condition. A better choice is to just use `mkdir -m 700`.
# Obtaining Random Data

`/dev/random` contains a random number generator with entropy defined by environmental noise, and will block until enough entropy has been collected. `/dev/urandom` is similar but allows for repetition and thus does not block. They, as well as `/dev/zero`, can be thought of as streams of characters from which a program can read instead of files with defined beginnings and ends. We'll usually use `/dev/urandom` because it's random enough and `/dev/random` can be exhausted by bad actors and also doesn't exist on MacOS. The only use case for the latter is when cryptographically secure (as opposed to computationally secure) data is necessary.
# Copying Files

`dd` can copy data from `if` to `of`, regardless of whether the files or physical or virtual. It can also create copies of entire disks or filesystems.
# Updating Modification Time

`touch` updates a file's last modified time and creates it if it doesn't exist, which is useful when we need to recompile an unchanged file using `make`.
# Managing Filesystems

`mount` generates a list of mounted filesystems including networked, virtual, and local (SSD-based). Each line will include the type, source, and mount point. We can pipe it into `grep` and only see lines that match a regular expression.

If we download a bootable Linux disk image, we can mount the file as a filesystem by using `sudo mount -o loop "distro"` after making a new directory for it. The loop option wraps the original file as a block device (reads/writes data in fixed-size blocks).