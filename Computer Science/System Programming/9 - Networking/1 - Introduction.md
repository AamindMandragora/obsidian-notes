Arguably the most important use of computers in the past couple decades is networking, as most of us expect WiFi or other forms of connectivity wherever we are. It's now crucial to understand networking and how to write programs that communicate across networks. POSIX has defined nice standards that make this easy, and lets us optimize any and all subtasks within connections.
# The Open Source Interconnection Model

The OSI 7-layer model is a sequence of segments that define standards for both infrastructure and protocols for forms of radio communication like the internet. Each segment gets progressively more abstracted.

1. Layer 1 is the physical layer, the actual waves that carry the bauds (signals) across a wire. We can alter the amplitude and the frequency of the wave to get more bits per cycle, which is where the term baud comes from.
2. Layer 2 is the link layer, and determines how each of the nodes react to events like errors or noise in the channel, and is where Ethernet and WiFi live.
3. Layer 3 is the network layer, the heart of the Internet. While the bottom two protocols deal with communication between two directly connected different computers, this layer routes packets between two endpoints.
4. Layer 4 is the transport layer, specifying the ordering of the packets that are being sent. The bottom three layers don't guarantee that the packets will be received in order, but this layer can depending on the protocol used.
5. Layer 5 is the session layer, ensuring that a dropped connection in the below layers can be reestablished without disturbing the user.
6. Layer 6 is the presentation layer, dealing with encryption, compression, and data translation between different operating systems.
7. Layer 7 is the application layer, where HTTP, FTP, and other internet protocols are defined. We really only need to go lower than this layer when we think we can create better algorithms for our specific use case.