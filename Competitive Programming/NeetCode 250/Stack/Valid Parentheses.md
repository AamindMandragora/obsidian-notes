# Problem

We're given a string `s` consisting of only parentheses, brackets, and curly braces. We must determine and return if it is valid: that every open brace is closed by the correct close brace in the correct order.
# Solution

At any point in the string, we can either open a new brace or close the last opened one. Once we close a brace, it no longer affects the rest of the string, so we can consider opening a brace pushing to a stack and closing a brace popping from it. If we ever encounter an illegal pop, we exit early:

```cpp
stack<int> st;
for (char c : s) {
	switch (c) {
	case '(':
	case '[':
	case '{':
		cout << c << endl;
		st.push(c);
		break;
	case ')':
		if (!st.empty() && st.top() == c - 1) {
			st.pop();
			break;
		} else {
			return false;
		}
	default:
		if (!st.empty() && st.top() == c - 2) {
			st.pop();
			break;
		} else {
			return false;
		}
	}
}
return st.empty();
```