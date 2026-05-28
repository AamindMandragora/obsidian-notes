# Problem

Given a reference to a `vector<int> nums` with `n` objects (less than $300$) colored either red, white, or blue, sort them *in-place* so that objects of the same color are adjacent, with the colors in the order red, then white, then blue, represented by integers `0`, `1`, and `2`, respectively, without using the `sort` function.
# Solution

Since there are very few choices for any given element in `nums`, we can create a frequency list and then recreate the list in order based on that:

```cpp
int freq[3] = {0};
for (int n : nums) {
	freq[n]++;
}
int z = 0;
for (int i = 0; i < 3; i++) {
	while (freq[i]-- != 0) {
		nums[z++] = i;
	}
}
```

We've reached $O(n)$ time and $O(1)$ space, but maybe we can do this in one pass. Our final array will be sorted, which means there's an index `high` where each number from it to the end is `2`, `med` where each number from it to `high` is `1`, and `low` where each number from it to `med` is `0`. We can start by assuming that `low = med = 0` and `high = n - 1`, then updating our guesses based on what we actually see at that position. If `med` is one, then it's in the right spot and we can increment. If `med` is zero, then we switch it with `low` and increment `low`. If `med` is two, then we switch it with `high` and decrement `high`. Once the pointers cross, then we've finished sorting:

```cpp
int low = 0, med = 0, high = nums.size() - 1;
while (low <= med && med <= high) {
	if (nums[med] == 1) {
		med++;
	} else if (nums[med] == 0) {
		nums[med++] = nums[low];
		nums[low++] = 0;
	} else {
		nums[med] = nums[high];
		nums[high--] = 2;
	}
}
```

This is also $O(n)$ time and $O(1)$ space, but it only requires one pass.