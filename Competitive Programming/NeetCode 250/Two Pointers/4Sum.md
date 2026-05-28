# Problem

Given a reference to a `vector<int> nums`, return a `vector<vector<int>>` of all the **unique** quadruplets of elements that sum to an `int target`.
# Solution

Note that if we hold two points fixed and `sort(nums.begin(), nums.end())`, we can reduce the problem to finding two numbers that sum to a new target integer, which we can solve in $O(n)$ time. We'll have to skip all duplicates and can optimize this approach by checking if the numbers we've fixed are too large to sum to `target` no matter what other future elements we choose:

```cpp
sort(nums.begin(), nums.end());
auto quads = vector<vector<int>>();
for (int i = 0; i < nums.size(); i++) {
	if (nums[i] > target && (target >= 0 || nums[i] >= 0)) break;
	while (i > 0 && i < nums.size() && nums[i] == nums[i - 1]) i++;
	for (int j = i + 1; j < nums.size(); j++) {
		if (nums[i] + nums[j] > target && target >= 0) break;
		while (j > i + 1 && j < nums.size() && nums[j] == nums[j - 1]) j++;
		int k = j + 1; int l = nums.size() - 1;
		while (k < l && (nums[i] + nums[j] <= target - nums[k] || target < 0)) {
			if (nums[i] + nums[j] < target - nums[l] - nums[k]) {
				k++;
			} else if (nums[i] + nums[j] + nums[k] > target - nums[l]) {
				l--;
			} else {
				quads.push_back({nums[i], nums[j], nums[k], nums[l]});
				do {k++;} while (nums[k] == nums[k - 1] && k < l);
				do {l--;} while (nums[l] == nums[l + 1] && k < l);
			}
		}
	}
}
return quads;
```

This solution is $O(n^3)$ time.