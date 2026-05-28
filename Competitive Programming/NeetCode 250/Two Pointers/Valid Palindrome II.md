# Problem

Given a `string s`, return whether `s` can be a palindrome after deleting at most one character from it.
# Solution

The brute force approach would be to check if `s` is already a palindrome, loop through all the characters and check if `s` would be a palindrome without them, and return `true` if any of those work. However, we can do better. We can maintain two pointers to the start and end of the string and a `bool deleted` to track if we've already used our deletion lifeline. We can keep iterating through the characters until we find a mismatch. At that point, we check if deleting either of the characters would result in a match, update the boolean, and carry on. If we find another mismatch, we return `false` immediately:

```cpp
bool tried_both = false;
bool deleted = false;
int x; int y;
for (int i = 0, j = s.length() - 1; i <= j; i++, j--) {
	if (s[i] != s[j]) {
		if (deleted) {
			if (tried_both) return false;
			if (s[y - 1] == s[x]) y--;
			else if (s[y] == s[x + 1]) x++;
			else return false;
			i = x; j = y; tried_both = true;
			continue;
		}
		x = i; y = j;
		if (s[i + 1] == s[j]) i++;
		else if (s[i] == s[j - 1]) j--;
		else return false;
		deleted = true;
	}
}
return true;
```

We also use a second `bool tried_both` in the above code because we make a greedy choice when choosing the `char` to delete, so if that doesn't work out we have to retry and choose the other option.

An alternative solution would implement more of the brute force logic, looping until we hit a mismatch and then running a `check` function to see if either deletion will work:

```cpp
bool check(string& s, int i, int j) {
	for (; i <= j; i++, j--) {
		if (s[i] != s[j]) {
			return false;
		}
	}
	return true;
}

bool validPalindrome(string s) {
	for (int i = 0, j = s.length() - 1; i <= j; i++, j--) {
		if (s[i] != s[j]) {
			return check(s, i + 1, j) || check(s, i, j - 1);
		}
	}
	return true;
}
```