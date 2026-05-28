# Problem

Given two strings `word1` and `word2`, create a new string containing the letters of both chosen in alternating order, with any extra characters appended at the end.
# Solution

We can maintain two pointers to the starts of both strings, then add the letters each point to to the output string as long as they're both in bounds:

```cpp
if (word1.empty()) return word2;
if (word2.empty()) return word1;
string s;
for (int i = 0, j = 0; i < word1.length() || j < word2.length(); i++, j++) {
	if (i < word1.length()) s += word1[i];
	if (j < word2.length()) s += word2[i];
}
return s;
```