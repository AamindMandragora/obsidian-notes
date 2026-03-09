The first provably correct solution to the mutex problem was Dekker's Algorithm, which works as follows:

```
---lock---
raise my flag
while your flag is raised:
	if it is your turn:
		lower my flag
		wait while your turn
		raise my flag

---critical---

---unlock---
set your turn
lower my flag
```

The thread's flag is always raised during lock no matter how many times the loop is iterated, which makes it interpretable as an immediate intent to enter the critical section. A process lowers their flag and waits only if another process has also raised its flag.

We can prove that this solution solves mutual exclusion by noticing that the only way a thread can progress past a lock is if the other thread lowers their flag, so there can be no sharing the critical section. Additionally, assuming the critical section ends in finite time, a thread must eventually leave it and the turn immediately gets switched to the next thread, so no thread ends up waiting infinitely. If a thread isn't in the critical section, it just does the check and keeps executing instructions.
# Peterson's Solution

In 1981, Peterson published his surprisingly simple solution that uses a global variable turn:

```
---lock---
raise my flag
turn = other_thread_id
while your flag is up and turn equals other_thread_id
	loop

---critical---

---unlock---
lower my flag
```

To prove this satisfies mutual exclusion, we notice that a thread can't escape the loop until either the turn equals its id or the other threads aren't accessing it. Raising and lowering the flag is the first and last thing the thread does, so no two threads can access the critical section at the same time. After a thread lowers it's flag, a thread waiting in the while loop will immediately enter the critical section even if the other thread relocks as the turn will equal the thread's id. If no other thread is contesting, then no other flag is up and the thread can just go through the loop to the critical section.

Unfortunately, we can't implement a software mutex in this way anymore due to the thread optimization shuffling the order of instructions.