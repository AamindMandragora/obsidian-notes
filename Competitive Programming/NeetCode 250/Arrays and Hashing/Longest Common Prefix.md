# Problem

We are given a reference to a `vector<string> strs` (of length between $1$ and $200$, in which any `string s` has between $0$ and $200$ characters, all of them being lowercase English letters) and must return a `string` containing the longest common prefix (a substring starting at the first character). If there is no common prefix, return the empty string.
# Solution

The brute force approach would be to loop through every character in the first string, then check if all the other strings have the same character in the same place, and break otherwise:

```cpp
string prefix = "";
for (int i = 0; i < strs[0].length(); i++) {
	bool common = true;
	for (int j = 1; j < strs.size() && common; j++) {
		if (i < strs[j].length() && strs[0][i] != strs[j][i]) {
			common = false;
		}
	}
	if (common) {
		prefix += strs[0][i];
	} else {
		break;
	}
}
return prefix;
```

This is $O(nk)$, where $k$ is the length of the smallest string in `strs`. Constructing a trie would have a similar time complexity but much worse space complexity. It's not a bad solution, but maybe we can do better. If we could order the strings by prefix somehow, then we'd only have to find the common prefix of the smallest and largest string in the list with respect to that ordering. Luckily, this is exactly how `C++` orders strings (lexicographical sort). Therefore, we can simply do:

```cpp
string prefix = "";
int n = strs.size() - 1;
sort(strs.begin(), strs.end());
for (int i = 0; i < strs[0].length(); i++) {
	if (strs[0][i] == strs[n][i]) {
		prefix += strs[0][i];
	} else {
		break;
	}
}
return prefix;
```

This solution is $O(k+n\log n)$, which is better as long as $\log n < k$, which holds true at the extremes of our bounds for them.