# Problem

Write a function that takes in a reference to a `vetcor<char> s` and reverses it **in-place** with $O(1)$ extra memory.
# Solution

If we reverse a string, then the `char` at position $0$ would get swapped with the `char` at position $n-1$, and $1$ would get swapped with $n-2$, and so on. We will use a `for` loop to swap the `char`s at index `i` and `n - i - 1`:

```cpp
for (int i = 0; i < s.size() / 2; i++) {
	swap(s[i], s[s.size() - i - 1]);
}
```