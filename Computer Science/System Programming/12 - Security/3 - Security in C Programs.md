# Stack Smashing

When we need to put a string in the stack, we'd typically use `strcpy`. However, if we don't check the bounds, a malicious user could pass in a string larger than intended, replacing important stored variables like the return address of the function in the stack. In some cases, we can actually get access to a shell by putting in bytecode equivalent to `execve('/bin/sh', '/bin/sh', NULL, NULL)`. This seriously messes with the integrity of the system. We can fix it by always using `strncpy` or `strlcpy` on OpenBSD systems, or turn on stack canaries.
# Buffer Overflow

Similarly, if we overfill a stack-allocated buffer, we can end up writing into another buffer that lives right next to it, which can be dangerous if either buffer is meant to hold important information.
# Out-of-Order Instructions and Spectre

Since modern processors spend a lot of time waiting for memory accesses and other i/o driven applications, they can now execute the next couple instructions to make more use of their time. This is, of course, only as long as the early execution wouldn't change the final result of the program or violate a data dependency.

However, out-of-order instructions mean that pure-software mutex implementations may fail without a memory barrier. One of the most prominent bugs regarding this is Spectre, where instructions that wouldn't be executed normally end up being speculatively executed.

For example, if we have a loop that runs some code on every iteration except for the last one, depending on the compilation conditions and flags, the processor will think that the code should be run since it was run in every past iteration and fetches the instructions as a result. If this results in a `SEGFAULT` due to a dereference of a bad address (like `0xCAFE`) but the branch shouldn't have been taken anyways, the processor discards the results. However, the cache will still hold the corresponding physical memory address, which means we can trick the processor again to read from the cache and get it, giving us access to potentially important and confidential data.
# Operating Systems Security

In POSIX, each file and directory has permissions set for certain users and groups, restricting the read-write-execute access to only intended people. Users have another set of general permissions or capabilities, like controlling networking devices and peering into IPC. 

Address Space Layout Randomization causes the address spaces of important sections of a process, like the base address of the executable and the positions of the stack, heap, and libraries, to start at randomized values on every run. This means that attackers have to guess where sensitive information could be hidden.

A stack canary is a value that resides in the stack and must remain constant for the duration of the function call. At the end, we check it, and if it's not the same, the run time will abort and report stack smashing to the user.

Data Execution Prevention (write XOR execute) protects executable code from being written to and writable files to be executed, preventing attackers from writing arbitrary code and execute using the user's permissions.

The Linux kernel provides the netfilter module to decide whether an incoming connection should be allowed or not, which can help prevent DDOS. AppArmor is a suite of OS tools in userspace to restrict applications to certain operations.

OpenBSD has arguably better security-oriented features, like pledge, which can restrict system calls (pledge), access of a current program to a few directories (unveil), and access to root capabilities (sudo).
# Virtualization Security

**Virtualization** is the act of creating a virtual version of an environment for a program to run on, providing virtual motherboard features like USB ports or monitors. Another program, the bridge, handles communication between those and the actual hardware to perform a task. A virtual machine emulate all forms of motherboard peripherals to create a full machine. A container is a virtual machine that share some peripherals with the host OS.

One reason to have virtualization is to ensure that the environment doesn't maliciously leak back into the host. We can use `chroot` to create a virtualization environment by changing where a program believes `/` is mounted on the system. A better way to do this is `namespaces` as Linux needs additional tools like the C standard library to come from different directories. Finally, there is now hardware virtualization technology that allows the OS to flip into a virtualization mode where instructions are monitored for malicious activity.