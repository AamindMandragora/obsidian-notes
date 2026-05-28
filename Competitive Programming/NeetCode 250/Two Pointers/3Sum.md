# Problem

Given a reference to a `vector<int> nums`, return all the unique triples of distinct elements that add to zero.
# Solution

The brute force approach is to loop through every triple and check if it sums to zero, then append it to the output array:

```cpp
auto triplets = vector<vector<int>>();
for (int i = 0; i < nums.size(); i++) {
	for (int j = i + 1; j < nums.size(); j++) {
		for (int k = j + 1; k < nums.size(); k++) {
			 if (nums[i] + nums[j] + nums[k] == 0) 
				 triplets.push_back({nums[i], nums[j], nums[k]});
		}
	}
}
return triplets;
```

However, this solution fails to detect triples. An easy way to detect triples is to `sort(nums.begin(), nums.end())`. Also, if the vector is sorted, holding `i` constant turns this problem into 2Sum with `j` and `k`:

```cpp
sort(nums.begin(), nums.end());
auto triplets = vector<vector<int>>();
for (int i = 0; i < nums.size(); i++) {
	if (nums[i] > 0) break;
	while (i > 0 && i < nums.size() && nums[i] == nums[i - 1]) i++;
	int j = i + 1; int k = nums.size() - 1;
	while (j < k) {
		if (nums[i] + nums[j] + nums[k] < 0) {
			j++;
		} else if (nums[i] + nums[j] + nums[k] > 0) {
			k--;
		} else {
			triplets.push_back({nums[i], nums[j], nums[k]});
			do {j++;} while (nums[j] == nums[j - 1] && j < k);
			do {k--;} while (nums[k] == nums[k + 1] && j < k);
		}
	}
}
return triplets;
```

Note that the original faulty solution, even with triple-detection, would have been $O(n^3)$. Using our 2Sum two-pointer approach, we get an $O(n^2)$ algorithm.