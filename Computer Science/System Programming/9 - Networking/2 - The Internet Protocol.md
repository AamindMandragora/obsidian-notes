IP is the primary way to send datagrams (packets) of information from one machine to another. IPv4 and IPv6 are two versions of the Internet Protocol describing how to send packets of information across a network from one machine to another. IPv4 dominates internet traffic, but the internet is slowly switching to IPv6 due to several limitations with the former, like a 32-bit limit on source and destination addresses, since at the time having four billion devices connected to the same network was unthinkable. IPv4 addresses are typically written in a sequence of four octets delimited by periods, and each datagram includes a 20-octet header with the source and destination address. IPv6 uses 128-bit addresses and has simpler routing tables. We write IPv6 addresses in a sequence of eight groups of four hexadecimal digits delimited by colons. Machines can have IPv4 and IPv6 addresses.

A special address pointing to the current machine is `127.0.0.1` in IPv4 or `0:0:0:0:0:0:0:1` (or `::1` for short) in IPv6. It's also known as `localhost`.
# Why IPv6?

We ran out of IPv4 addresses a long time ago, but even if we discovered alien civilizations having 128-bit addresses means we probably still won't run out of IPv6 addresses. Also, the addresses are leased, given out by providers, which means they can be reassigned if necessary. IPsec, a key exchange, was also introduced, allowing encrypted communication.

IPv4 and IPv6 headers are verified in hardware, but as the IPv4 spec grew, the hardware had to become more and more complicated to support the new headers. IPv6 reorganizes its headers into a fixed-length "main header" and subsequent "extension headers" so the hardware always knows where and how much to read.
# What's My Address?

We can use `getifaddrs` to get a linked list of IPv4, IPv6, and other IP addresses of the current machine. We can call `getnameinfo` to get the host and port of an address. Any given struct in the linked list includes the family but not the size of the struct, so we must manually determine it based on the family.

To get our IP address from the command line, we can use `ifconfig` on UNIX-based systems and `ipconfig` on Windows. It will generate a lot of output, which we can filter using `grep inet`. To get the IP address of a remote website, we can use `getaddrinfo` to convert a human-readable domain name into an IPv4 and IPv6 address, returning a linked list of `addrinfo` structs.

`getaddrinfo` takes a domain name (or IP address, a service like "http" (or a port number), a `const struct addrinfo` that filters outputs based on filled out fields, and a result `struct addrinfo` that holds the linked list.

The computer can map hostnames to addresses through a service called DNS. Websites can have multiple IP addresses so that they can separate their traffic by region.