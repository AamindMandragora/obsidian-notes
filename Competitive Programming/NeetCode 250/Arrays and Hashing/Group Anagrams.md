# Problem

We're given a reference to a `vector<string> strs` (with elements containing $100$ or less lowercase English letters), and we need to return a `vector<vector<string>> groups` such that each element of `groups` is a `vector<string>` containing strings in `strs` that are anagrams of each other. `groups` can be in any order.
# Solution

We already know anagrams can be identified by a frequency array, so we might try creating an `unordered_map` that sends an `int[256]` to a `vector<string>` containing every string with those frequencies. However, since `int[]` degrades to an `int*`, we can't hash it, so we'll need to convert it to a representation that can be hashed, like a string. If we concatenate every number in the array with a character like `#`, we've got our unique frequency list in a hashable form, so we can do the rest normally. At the end, once our `unordered_map` is created, we can simply create `groups` by iterating through every key-value pair in `map` and appending the value.

However, the time complexity for creating a key in the map is $O(k)$ times $256$ to convert it to a string, where $k$ is the average length of a string in `strs`. Since our actual strings will always be smaller, we can instead create a key using the sorted string in $O(k\log k)$, which turns out to be much faster:

```cpp
unordered_map<string, vector<string>> map;
for (string& s : strs) {
	string key = s;
	sort(key.begin(), key.end());
	map[key].push_back(s);
}
vector<vector<string>> groups;
for (auto& p : map) {
	groups.push_back(p.second);
}
return groups;
```

The total time complexity for this solution is $O(nk\log k)$.