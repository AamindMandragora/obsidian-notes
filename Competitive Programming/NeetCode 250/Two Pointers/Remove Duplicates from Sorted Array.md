# Problem

Given a reference to a `vector<int> nums` sorted in **non-decreasing order**, remove the duplicates in-place such that each element in `nums` appears only once after your modifications. Finally, return the number of unique elements $k$. The judge will only check that your first $k$ elements are as expected.
# Solution

The brute-force solution is just to create a `set` out of the `vector`, then repopulate the first $k$ elements of the `vector`. However, a better solution notices that, since the array is sorted, all the duplicate elements are right next to each other. Therefore, we can maintain a count of the number of unique elements we've seen as we stride through the array, occasionally moving an element to it's proper place and updating the count as we see new elements:

```cpp
int k = 1;
for (int i = 1; i < nums.size(); i++) {
	if (nums[i] != nums[i - 1]) {
		nums[k++] = nums[i];
	}
}
return k;
```