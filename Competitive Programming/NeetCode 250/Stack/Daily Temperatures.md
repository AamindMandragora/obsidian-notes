# Problem

Given a reference to a `vector<int> temperatures`, return a `vector<int> result` where `result[i]` is $k$ where $k$ is either the smallest positive integer such that `temperatures[i] < temperatures[i + k]` or $0$ if none exists.
# Solution

Let `n = temperatures.size()`, then `result[n - 1]` equals $0$ for sure. Then, we move backwards. If the previous temperature is larger, then it's result will equal zero and it's our new comparison. Otherwise, it's result will equal one and we now have three cases: whether the one before the previous one will be less than it, between it and the last one, or above the last one. With every previous day, our space will keep partitioning, but if we ever find a temperature larger than any number of partitions, then we can disregard them as they are further away. This lends itself to a stack:

```cpp
auto result = vector<int>(temperatures.size());
stack<pair<int, int>> s;
for (int i = temperatures.size() - 1; i >= 0; i--) {
	while (!s.empty() && temperatures[i] >= s.top().first) {
		s.pop();
	}
	if (s.empty()) {
		result[i] = 0;
	} else {
		result[i] = s.top().second - i;
	}
	s.push({temperatures[i], i});
}
return result;
```