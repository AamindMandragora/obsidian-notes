# Problem

Given a valid absolute Unix-style path that always starts with a slash, convert it into its simplified canonical path. Multiple consecutive slashes should be treated as a single one, `./` is the current directory, `../` is the parent directory, and any other number of periods are valid directory or file names.
# Solution

We'll convert the path to a `stringstream`, then use `getline(ss, name, '/')` to get each directory in the path. From there, we'll `push` if it's a valid directory of file name, do nothing if it's `""` or `"."`, and `pop` if it's `".."`. Once we're done, we'll construct the string by popping from the front and delimiting with slashes.

```cpp
stringstream ss(path);
string name;
deque<string> dq;
while (getline(ss, name, '/')) {
	if (name == "..") {
		if (!dq.empty()) dq.pop_back();
	} else if (name != "" && name != ".") {
		dq.push_back(name);
	}
}
string p = "";
while (!dq.empty()) {
	p += '/' + dq.front();
	dq.pop_front();
}
return p == "" ? "/" : p;
```