# Problem

We're given a reference to an unsorted `vector<int> nums`. Return the smallest positive integer that is not present in `nums` in $O(n)$ time and $O(1)$ auxiliary space.
# Solution

If `n = nums.size()`, then our answer will be in the interval $[1, n+1]$. We can't sort the array, and we can't make an extra set to easily check. Maybe we can turn `nums` into a set? We already know that any integer outside of the interval minus one won't make a difference, so we set them all to zero. Then, if `k` exists in `nums`, `nums[k - 1]` will be marked negative (or set to `-(n + 1)`.

```cpp
for (int i = 0; i < nums.size(); i++) {
	if (nums[i] < 0 || nums[i] > nums.size()) nums[i] = 0;
}
for (int i = 0; i < nums.size(); i++) {
	if (nums[i] != 0 && nums[i] != -(nums.size() + 1)) {
		int n = abs(nums[i]);
		if (nums[n - 1] == 0) nums[n - 1] = -(nums.size() + 1);
		else nums[n - 1] = -abs(nums[n - 1]);
	}
}
for (int i = 0; i < nums.size(); i++) {
	if (nums[i] >= 0) return i + 1;
}
return nums.size() + 1;
```