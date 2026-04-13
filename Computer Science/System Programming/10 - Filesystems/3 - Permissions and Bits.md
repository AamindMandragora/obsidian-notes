The `st_mode` field in the stat call from last time contained more than just the file type. It also contains the permissions for the file, which are a key UNIX security measure. There are three sets of permissions: one for the *user*, one for the *group*, and one for *other* users. The three permission bits are read (r), write (w), and execute (x), and are usually represented as an octal number. For directories, permission to write means the ability to create and delete files, permission to read means the ability to list its contents, and permission to execute lets programs enter it using `cd`. This means that you can't access a directory without having execute permissions. `mknod` changes the type of the file and `chmod` changes the permission bits.
# User and Group ID

In a UNIX system, every user has a unique ID, as do the groups that they can be put in. An example of a group is `sudoers`, the collection of users that can elevate their privileges by using `sudo`. Processes also have user ids and group ids, and when processes try and access files, their uids and gids are compared with those of the file to get the permission set.
# Reading / Changing File Permissions

To read permission bits from the command line, we can use `ls -l`, which gives us one character for the file type (`-` for regular, `d` for directory, `l` for symbolic link, `p` for named pipe, `s` for socket, etc.) and nine characters for each permission. We can also call `stat` in C.

To change the permission bits, there is a system call (and eponymous command line utility) `chmod` that takes in the file and the mode, either in base-8 (where the bits are read-write-execute) or with a symbolic string.
# Understanding the `umask`

The umask subtracts permission bits from the full set `777` and is used when new files and directories are created by `open` or `mkdir`. It's set to `022` by default, removing the write permissions from group and other. We can change it in the terminal by calling `umask ###`.
## `setuid` and `setgid` Bits

Files with execute permissions may have bits set that, when the file is run, change the (effective) uid and/or gid of the executor to those of the owner of the file. A good example of this is `sudo`, which gives a user access to most parts of the system, like calling `chown` to change ownership of a file. Note that we can still get both the user's actual uid and gid and those the user was assigned.
# The Sticky Bit

A long time ago, sticky bits could be set on executable files to allow the text segment of a program to remain even after the end of execution, which made subsequent executions faster. This behavior is no longer supported. Today, when a directory's sticky bit is set, only the root user, directory owner, or the file owner can rename or delete a file, which is nice when a folder can be accessed by multiple users but they shouldn't be able to access files belonging to other users. We can set this bit using `chmod +t`.