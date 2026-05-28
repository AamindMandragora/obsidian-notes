# Problem

Given references to two strings `s` and `t`, return `true` if `t` is an anagram of `s` (every character in `s` appears in `t` and vice versa), and `false` otherwise. `s` and `t` both consist of lowercase English letters.
# Solution

We know that if two strings are anagrams, then every character in either string appears in the other. Therefore, if we calculate the frequency maps of each character for both strings, they will be equal if and only if the two strings are anagrams. Also, since the characters in either string are a subset of those representable by `char`, we can use an integer array and index into it with the `char`:

```cpp
int sfreq[256] = {0};
int tfreq[256] = {0};

for (char c : s)
	sfreq[c]++;
for (char c : t) 
	tfreq[c]++;
	
for (char c = 0; c < 256; c++) { 
	if (sfreq[c] != tfreq[c]) return false;
}
return true;
```

A small optimization would be to maintain a single frequency list, add to the counts for every character in `s`, then subtract from the counts for every character in `t`:

```cpp
int freq[256] = {0};

for (char c : s)
	freq[c]++;
for (char c : t)
	freq[c]--;
	
for (char c = 0; c < 256; c++) {
	if (freq[c]) return false;
}
return true;
```

Both solutions have time complexity $O(k)$, where $k$ is the length of the longer string between `s` and `t`.