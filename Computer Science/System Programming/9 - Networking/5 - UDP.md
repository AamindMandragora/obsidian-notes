UDP is a connectionless protocol built on top of IPv4 and IPv6. We just have to decide the destination host and port and send the packet. However, since there's no connection, we can't guarantee that the packets arrive in order or at all. We typically use it when sending continuous updates, as getting the state quickly is more important than making sure we traverse every single one.
# UDP Attributes

The long form of UDP is Unreliable Datagram Protocol, where a datagram is a packet and the unreliable comes from the lack of guarantees on whether datagrams arrive and the conditions in which they arrive. However, this makes it much simpler and stateless since the protocol doesn't have to keep track of packets or need extra parameters. We have full control over the flow and congestion control, but we lose the decades of TCP optimization, which means our implementation needs to be really good for our use cases to justify it. A UDP exclusive feature is sending a message to every peer connected to some router that is part of some group.

Surprisingly, many protocols that can't have data loss are built on UDP, like the Trivial File Transfer Protocol. This does require a lot of configuration.
# UDP Client

We only have to use the `getaddrinfo` and `socket` functions to set up a UDP client since it's a connectionless prototype. The following code allows us to make our port reusable (also works with TCP):

```c
int sockfd - socket(AF_INET, SOCK_DGRAM, 0);
int optval = 1;
setsockopt(sockfd, SOL_SOCKET, SO_REUSEPORT, &optval, sizeof(optval));
```

We can also time out receiving a packet using the following:

```c
struct timeval tv;
tv.tv_sec = 0;
tv.tv_usec = SOCKET_TIMEOUT;
setsockopt(sockfd, SOL_SOCKET, SO_RCVTIMEO, &tv, sizeof(tv));
```

To send a packet, we can use `int sendto(int socket, char *data, size_t len, int flags, struct sockaddr *addr, size_t addrlen)`.
# UDP Server

We set the server up the exact same way in UDP as TCP, except we don't need the `listen` or `accept` calls because it's connectionless. To get data from the server, we will need the following code:

```c
struct sockaddr_storage addr; // large enough to hold any address
int addrlen = sizeof(addr);

// reads sizeof(buf) bytes from sockfd into buf, puts sender addr into sockaddr_storage in case we want to do something with it
bytes = recvfrom(sockfd, buf, sizeof(buf), 0, &addr, &addrlen);
```

If our buffer isn't big enough, the rest of the data that couldn't be fit in it gets discarded. One call to `recvfrom` reads one packet.