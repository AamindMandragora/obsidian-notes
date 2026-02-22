We can use threads by including `pthread_h` and compiling and linking with either the `-pthread` or `-lpthread` flags. The `pthread_create` function takes four arguments:

`int pthread_create(pthread_t *thread, constr pthread_attr_t *attr, void *(*start_routine) (void *), void *arg`

The first is a pointer to what will hold the thread's ID, the second is a pointer to attributes that can edit some of the advanced features, the third is a pointer to the function we want to run, and the fourth is a `void` pointer and the single argument that function will take. When using `pthread_join`, the result will be the returned value of the passed-in function.