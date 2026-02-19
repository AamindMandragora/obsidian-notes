# Everything is a File

POSIX was built on the belief that "everything is a file", which is now outdated, but the convention is still used today. What that means is that everything (files, sockets, kernel objects, etc.) is a file descriptor, which is an integer (that acts like a pointer) stored in the kernel's file descriptor table. Operations on file descriptors are done through system calls. These objects can be allocated, deallocated, opened, closed, and more through the API specified by system calls and library functions.
# System Calls

A system call is an operation carried out by the kernel. It's first prepared by the OS, then an attempt is made to execute it in kernel space and is therefore a privileged operation. An example of a system call is `write(file_fd, "Hello!", 6);` which writes "Hello!" (which is six characters long) to the file pointed to by the file descriptor. "An attempt is made" means that something may cause the operation to fail, like a no-longer-valid file, a failed hard drive, an interrupted system, etc. Also, system calls are very expensive in terms of time and clock cycles.
# C System Calls

Many C functions are abstractions that call the required system calls based on the current OS, but we'll look at them in the context of Linux.