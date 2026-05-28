# Problem

Design functions that encode a list of strings to a single string then decodes that single string back. Each string can contain any ASCII character.
# Solution

The idea that first comes to mind is simply concatenating all the strings. However, we'd have no way to correctly figure out where to split that encoded string. Also, since the strings can contain any character, we can't simply choose one to be a delimiter. We can address the first issue by prepending each string with its size, but we still wouldn't be able to determine whether the size of "12345..." is $1$, $12$, or something else. We do know that every size of a string will definitely fit inside a `size_t`, so we can `reinterpret_cast` it to a `const char*` and `append` it to the string.

```cpp
string encode(vector<string>& strs) {
	string encoded = "";
	for (string& s : strs) {
		size_t len = s.length();
		encoded.append(reinterpret_cast<const char*>(&len), sizeof(len));
		encoded.append(s);
	}
	return encoded;
}
```

To decode, we can just grab the first eight bytes, `copy` it into a `size_t` that we `reinterpret_cast` into a `char*`, then grab the substring that's that long and add it to the `decoded` list.

```cpp
vector<string> decode(string s) {
	vector<string> decoded;
	int idx = 0;
	while (idx < s.length()) {
		size_t len;
		s.copy(reinterpret_cast<char*>(&len), sizeof(len), idx);
		idx += sizeof(len);
		decoded.push_back(s.substr(idx, len));
		idx += len;
	}
	return decoded;
}
```