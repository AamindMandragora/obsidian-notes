# Problem

Given a reference to a `vector<int> nums`, return the length of the longest numerically consecutive sequence of elements with time complexity $O(n)$.
# Solution

If this array were sorted, we could simply iterate through and check if the next number is part of the previous sequence and return the max length at the end. However, `sort` is $O(n\log n)$, so we can't use that. However, if we're looking at an integer `n` in `nums`, if we could check in constant time whether `n - 1` was somewhere in the `vector`, we'd easily be able to identify starts of the sequence. From there, we could check whether some `n + k` existed and count the lengths of the sequence, then proceed as normal:

```cpp
auto unique = unordered_set<int>(nums.begin(), nums.end());
int longest = 0;
for (int u : unique) {
	if (unique.find(u - 1) != unique.end()) continue;
	int i = 1;
	while (unique.find(u + i) != unique.end()) i++;
	longest = longest > i ? longest : i;
}
return longest;
```

Since every number will either be a sequence start or somewhere inside it, they're all accessed at most twice, making this solution $O(n)$ time.