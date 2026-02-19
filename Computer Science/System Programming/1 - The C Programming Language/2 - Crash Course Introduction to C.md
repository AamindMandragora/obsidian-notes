The traditional first program in any language is "Hello World". In C, it looks like the following: 

```c
#include <stdio.h>
int main(void) {
	printf("Hello World\n");
	return 0;
}
```

The first line tells the compiler to `#include` the `stdio.h` (standard input and output) file in the program before generating the object file.

The second line is a declaration of a function that returns an `int`, is called `main`, and takes no parameters (denoted by `(void)`). It also uses `{}` to extend the scope containing the function definition past the line.

The third line is a function call from `stdio.h` prints formatted output to the standard output file, and we include a newline to flush the buffer (complete the write).

Recall that `main` returns an `int`. The fourth line returns `0`, which means success, while anything else would mean failure.

We can compile the program using `gcc main.c -o main`, which creates an executable called `main` using the GCC compiler, which can be run by calling `./main`.
# Preprocessor

Before compiling the program, the compiler runs preprocessing operations that replace macros with their definitions. Macros can be defined with `#define min(a,b) a < b ? a : b`. Make sure to remember that there's a limit to how nested macros can be. Also, preprocessing is a purely textual substitution and won't preserve the desired order of operations unless it's explicitly stated in the macro body.