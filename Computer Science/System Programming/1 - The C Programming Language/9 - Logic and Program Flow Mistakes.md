# Equal vs. Equality

In C, the assignment operator `=` also returns the assigned value, so we can initialize multiple things on the same line. This means that if we put it inside a conditional, instead of throwing an error, it'll evaluate the truthiness of whatever is being assigned. While there are cases we'd want to do this (like saving the value of `getliine`, then checking if it equals `-1`) it's usually not the case, and if we put the constant first, the compiler will recognize the error.
# Undeclared or Incorrectly Prototyped Functions

Since functions from other header files aren't found until link time, which is after compile time. you need to make sure that you're using the right function signature. In some cases, functions that take an argument but aren't given any could work for a while as long as the arguments on the stack are zeroed out, but SEGFAULT the second they aren't.
# Extra Semicolons

Putting semicolons when unneeded can cause unintended behavior, especially after the conditions of a loop, where the single line that you're trying to loop only gets run once. This is why we always use braces.