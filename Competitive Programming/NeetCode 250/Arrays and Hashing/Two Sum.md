# Problem

Given a reference to a `vector<int> nums` and an `int target`, find two numbers in `nums` with distinct indices such that their sum is `target` (a unique solution is guaranteed to exist). Return a `vector<int>` containing the *indices* of those two numbers in any order.
# Solution

The brute force approach would be to set up two `for` loops that check whether a pair of elements sum to the target number:

```cpp
for (int i = 0; i < nums.size(); i++) {
	for (int j = i + 1; j < nums.size(); j++) {
		if (nums[i] + nums[j] == target) {
			return {i, j};
		}
	}
}
return {-1, -2};
```

However, this has time complexity $O(n^2)$, which means there must be an optimization somewhere. If we fix `nums[i]`, then we know that if it were to be part of the unique solution, there must be some index `j` such that `nums[j] = target - nums[i]`. This means at every stage of the loop we can maintain some set containing the values we'd need to see to complete a solution. However, since we need to return the indices, we'll need to link the two in a map:

```cpp
unordered_map<int, int> needed;
for (int i = 0; i < nums.size(); i++) {
	if (needed.find(nums[i]) != needed.end()) {
		return {needed[nums[i]], i};
	}
	needed[target - nums[i]] = i;
}
return {-1, -2};
```

This solution is $O(n)$, a massive improvement over the brute force solution but at the cost of an $O(n)$ space complexity.