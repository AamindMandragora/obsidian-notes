If data is unavailable, `read` will block until it is, which is fine when reading from disk, but can take a large amount of time on a slow network connection, if the data even ends up arriving. We can set a flag on any file descriptor to make system calls on it non-blocking by calling the following:

```c
int flags = fcntl(fd, F_GETFL, 0);
fcntl(fd, F_SETFL, flags | O_NONBLOCK);

int sock_fd = socket(AF_INET, SOCK_STREAM | SOCK_NONBLOCK, 0);
```

The last line creates a non-blocking socket (which means non-blocking connect, so we have to wait for the connection to complete). When we call `read` on it, we'll immediately get whatever bytes are available and a count of how many bytes we got. If there's no data, `read` will return `-1` and set `errno` to either `EAGAIN` or `EWOULDBLOCK` to tell us there's no data. `write` works in a similar way.

Since looping until our file descriptor is ready to be used again makes it no more efficient than blocking, we need to find other ways to check if our i/o has arrived. The first way is as follows:

```c
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

We create sets of `read`, `write`, and `except` file descriptors (the last one isn't well defined so we pass in `NULL`), then pass them into `select`, which waits a time defined by `timeout` and returns the total number of ready file descriptors, overwriting the sets passed in to only contain them. However, we'll still have to loop through all the file descriptors and see if they're in the set after `select` to be able to use them, which makes it extremely inefficient, especially since if any descriptor changes state, `select` will have to restart.
# Epoll

`epoll` isn't part of POSIX, but it is supported by Linux, and it tells us not only exactly which descriptors are ready but gives us a way to store some data, like an array index or pointer, with each descriptor, making it easier to access.

We can create an `epoll` file descriptor with `int epfd = epoll_create(1)` and then add any number of file descriptors to it using the following:

```c
struct epoll_event event;
event.events = EPOLLOUT; // EPOLLIN is read, EPOLLOUT is write
event.data.ptr = ptr; // points to some user-defined struct with fd field
epoll_ctl(epfd, EPOLL_CTL_ADD, ptr->fd, &event);
```

To wait for some of the file descriptors to become ready, we can use the following code: 

```c
int num_ready = epoll_wait(epfd, &event, 1, timeout_milliseconds); // max 1 event
if (num_ready > 0) {
	// gets ptr to user-defined data struct
	MyData* ptr = (MyData*)event.data.ptr;
	// do smth with ptr->fd;
}
```

If we want to switch between waiting to write and waiting to read from a file descriptor, we can use `epoll_ctl` with the `EPOLL_CTL_MOD` option. Removing a single file descriptor from `epoll` can be done using the `EPOLL_CTL_DEL` option, and we can shut down `epoll` by calling `close` on its file descriptor.
# Epoll Example

Assume we have some TCP server `listen_sock`, then we first have to create the `epoll` device, then add the listen socket with the `EPOLLIN` event (since `connect` is technically a read), then wait on `epoll` in a loop until an event happens. If we get an event on a client socket, we perform the operation. If not, then it was the server socket, so we have to `accept` the incoming connection, call `setnonblocking` on the file descriptor, then add it to the `epoll`.
# Assorted Epoll Gotchas

There are two modes to `epoll`: level triggered (default), which means that the file descriptor will be returned by `wait` as long as it has any events, and edge triggered (`EPOLLET`), which means the file descriptor will only get the file descriptor once it goes from zero events to an event. The first mode may lead to high CPU usage if all events aren't processed, since it'll keep notifying you in wait, and the second mode also requires you to process all events, otherwise the file descriptor will never again be returned by `wait` since it'll never be able to go from zero to one event.

Any duplicated file descriptors added to `epoll` will get events every time the original does, and `epoll` objects can be added to `epoll` (in this case, edge and level triggered modes are the same). Since `epoll` works on the kernel object level, it can return a file descriptor that's closed in your process but open in another one (or duplicated). Calling `DEL` on the file descriptor removes the kernel object, but if we close it first, we won't be able to perform that call and the kernel object wouldn't be removed.

The `EPOLLONESHOT` flag removes a file descriptor after it's been returned in `epoll_wait`.