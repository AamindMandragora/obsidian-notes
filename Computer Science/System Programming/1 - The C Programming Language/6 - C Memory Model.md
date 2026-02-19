The basis of the C memory model is either using an automatic variable on the stack or requesting / freeing a sequence of bytes on the heap.
# Structs

A struct is a piece of contiguous memory just like an array except a single one can hold data in several different types. The compiler looks at the struct definition to calculate how much space needs to be reserved and how offset from the struct pointer each variable is. However, the offset doesn't determine where the elements end, only where they start. Here's a hack seen in kernel code:

```c
typedef struct {
	int length;
	char c_str[0];
} p_string;

const char* to_convert = "person";
int length = strlen(to_convert);

p_string* person = malloc(sizeof(string) + length + 1);
strcpy(p_string->c_str, to_convert);
```

We define a `struct` for a P string (which holds the length of the string instead of forcing NUL termination), but make the `c_str` variable have length `0` and therefore size `0`. However, if we `malloc` enough extra space to hold the bytes in `to_convert`, we can use `c_str` as a pointer and `strcpy` into it without needing a separate `malloc` call to define how long we want the `c_str` field to be.
# Strings in C

We define a C string as a sequence of bytes ended by the NUL byte `'\0'`.
# Places for Strings

String literals are defined by `char *str = "constant"` and are stored in the data segment, which is read-only (will SEGFAULT on modification attempt) for some architecture. If we use `char[]` instead of `char*`, then the string becomes mutable and lives in either the data segment or the stack. However, alterations can't force the new string to exceed the size of the old. If we `malloc` space, the string lives on the heap and we can change it to be anything we want, including longer strings if we `realloc` more space.

Two identical string literals can share the same location in memory as they are read-only, but `char` arrays will always be in different locations.