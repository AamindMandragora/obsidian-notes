# Pointer Basics

Pointers are variables that hold numeric addresses, and the type of the pointer tells the compiler how many bytes the value being pointed to takes up, and how many bytes to move forwards / backwards during pointer arithmetic.

C's syntax considers the pointer modifier `*` to be attached to the variable and not the type, so code like `int* ptr1, ptr2;` declares `ptr2` as a regular `int`. We also use the asterisk to dereference a pointer, which gives us read and write access to the data being pointed to as long as it's a primitive type. 
# Pointer Arithmetic

Pointers can be added to or subtracted from by integers, but the integer denotes a number of bytes equal to the size of the type being pointed to. For example, an `int *ptr` points to a piece of memory four bytes wide, so `ptr + 1` points to a memory location four bytes ahead of `ptr`, while for a `char *ptr`, `ptr + 1` points to a location one byte ahead.
# Void Pointer

A `void` pointer is a pointer without a type and symbolizes a raw memory address. It's what functions like `malloc` return and will automatically get converted into any other pointer type. Arithmetic is undefined for void pointers, but compilers like `gcc` and `clang` treat it as a `char` pointer.