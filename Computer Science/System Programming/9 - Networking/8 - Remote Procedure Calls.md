RPC is the idea that we can execute a procedure on a different machine (or on the same machine but in a different context). For example, we could send an RPC to a Docker daemon to change the container's state.
# Privilege Separation

The remote code will execute under a different user and with different privileged from the caller, which can improve the security of a system by ensuring components operate with the least necessary privilege in theory. However, we must make sure that RPC mechanisms can't be subverted to perform unwanted actions.
# Stub Code and Marshaling

The stub code hides the complexity of performing an RPC by **marshalling** the data into a format that can be sent as a stream of bytes to a remote server. One way to do this is to simply send the trace of the function call as a string, then read the response from the server and cast to the desired output type. Strings can be inefficient, and there are better implementations by Golang and Google. The server stub code unmarshals the request, performs the operation, then sends the result to the caller.

RPC requires strong documentations on data serialization conventions. For example, integers can be signed or unsigned, encoded in ASCII, Unicode, or some other encoding, a fixed or variable number or bytes, and little or big endian. To marshal a struct, we may be able to skip some fields. For a linked list, we can skip the pointers and stream the values, having the server recreate the linked list structure. For a tree, we can decide a head of time the procession order and do the same thing.
# Interface Description Language

Writing stub code by hand is painful and difficult, so we'd like to specify the objects and messages to automatically generate the code. Google's Protocol Buffer `.proto` files are a modern example of this, but RPC still is orders of magnitude slower and more complex than local calls. They also have to handle network failures and version differences, and secure RPC has to handle authentication and data validation.
# Transferring Structured Data

Protocols like JSON and XML are text-based protocols and are often used to store data. However, if an application takes too much time to parse them, we can switch to Google Protocol Buffers, which generate client and server stub code in multiple languages from the `.proto` file and emphasize high throughput with low CPU overhead.