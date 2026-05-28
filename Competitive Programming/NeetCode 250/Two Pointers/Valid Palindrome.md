# Problem

A string is a **palindrome** if, after normalizing case and removing all non-alphanumeric characters, it reads the same forwards and backwards. Given a `string s`, return whether it is a palindrome.
# Solution

If we assume the string is already normalized, then we can simply iterate through the characters and see if `s[i] == s[s.length() - i - 1]` for all `i`. However, we'll need to skip all the non-alphanumeric characters. We can do this by maintaining a second `int j` that points to the end of the string and doesn't depend on `i`. Then, if either counter points to a bad `char`, we can just increment until it doesn't:

```cpp
for (int i = 0, j = s.length() - 1; i <= j; i++, j--) {
	while (i < s.length() && !(isalnum(s[i]))) i++;
	while (j >= 0 && !(isalnum(s[j]))) j--;
	if (i <= j) {
		if (tolower(s[i]) != tolower(s[j])) {
			return false;
		}	
	}
}
return true;
```