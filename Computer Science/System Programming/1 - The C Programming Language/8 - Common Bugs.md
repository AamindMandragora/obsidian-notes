# NUL Bytes

Oftentimes during string manipulation, we check to see if we've reached the end by seeing if the dereferenced pointer is NUL. However, we'll often forget to copy over the NUL byte, or be changing pointers instead of changing the values being pointed to.
# Double Frees

A double free is when a program tries to deallocate the same piece of memory twice, often as a result of incorrectly writing to freed memory beforehand.
# Returning Pointers to Automatic Variables

Automatic variables are stack-allocated and disappear after the function dies, so any returned pointer to one of those variables can't be used.
# Insufficient Memory Allocation

When calling `malloc`, make sure to pass in the `sizeof` the datatype being pointed instead of the `sizeof` the pointer, otherwise you will corrupt memory after using the pointer.
# Buffer Overflow / Underflow

Since C doesn't check whether a pointer is valid, it's the programmer's responsibility to ensure their buffers are the right size. Even library functions may cause overflow or underflow if the array passed in is too short.
# Strings Require `strlen(s) + 1` Bytes

Since `strlen` doesn't count the NUL byte but every string must have one, so when allocating memory for a string, remember to add one.
# Using Uninitialized Variables

Automatic variables hold whatever garbage happened to be in that place in the stack, so don't assume it'll always be initialized to zero.
# Assuming Uninitialized Memory will be Zeroed

Automatic and heap variables may contain random bytes, so make sure to only use initialized variables.