# Problem

Given a reference to a `vector<int> nums`, create a sorted version of that `vector<int>` and return it **without using any built-in functions**. It must run in $O(n\log n)$ time complexity and have minimal space complexity. All elements will be between $-5\cdot10^4$ and $5\cdot10^4$.
# Solution

The easiest sorting algorithm is to conduct $n$ passes through `nums`, selects the smallest unchosen number each time, and pushes it to the end of `sorted`:

```cpp
auto sorted = vector<int>(nums.size());
for (int s = 0; s < sorted.size(); s++)
	int small = nums[0];
	int idx = 0;
	for (int i = 1; i < nums.size(); i++) {
		if (small > nums[i]) {
			small = nums[i];
			idx = i;
		}
	}
	sorted[s++] = small;
	nums.erase(nums.begin() + idx);
}
return sorted;
```

However, this runs in $O(n^2)$ time, which is too slow. Let's try considering two subarrays in `nums`, the left half and the right half. Generally, this doesn't help, but if the two subarrays are sorted, we can simply maintain two pointers and iterate through the lists, appending whichever element is smaller at each step. We can try a recursive approach based on this:

```cpp
vector<int> merge(vector<int>& v1, vector<int>& v2) {
	int i = 0, j = 0;
	auto merged = vector<int>(v1.size() + v2.size());
	while (i < v1.size() && j < v2.size()) {
		if (v1[i] <= v2[j]) {
			merged[i + j] = v1[i];
			i++;
		} else {
			merged[i + j] = v2[j];
			j++;
		}
	}
	while (i < v1.size()) {
		merged[i + j] = v1[i];
		i++;
	}
	while (j < v2.size()) {
		merged[i + j] = v2[j];
		j++;
	}
	return merged;
}

vector<int> sortArray(vector<int>& nums) {
	if (nums.size() <= 1) {
		return nums;
	} else if (nums.size() == 2) {
		if (nums[0] > nums[1]) {
			int temp = nums[0];
			nums[0] = nums[1];
			nums[1] = temp;
		}
		return nums;
	}
	auto v1 = vector<int>(nums.begin(), nums.begin() + nums.size() / 2);
	auto v2 = vector<int>(nums.begin() + nums.size() / 2, nums.end());
	v1 = sortArray(v1);
	v2 = sortArray(v2);
	return merge(v1, v2);
}
```

This runs in $O(n\log n)$ time and requires $O(n)$ extra space. Despite meeting the requirements, we may be able to do better. Since we have a small bounded range on the elements in `nums`, we can maintain a frequency list of each element and rebuild the array using it:

```cpp
int freq[100001] = {0};
for (int n : nums) {
	freq[n + 50000]++;
}
auto sorted = vector<int>(nums.size());
int s = 0;
for (int i = 0; i < 100001; i++) {
	while (freq[i] != 0) {
		sorted[s] = i - 50000;
		freq[i]--; s++;
	}
}
return sorted;
```

Since the bound remains constant, this is technically $O(n)$ time and $O(1)$ space, which is a massive improvement.