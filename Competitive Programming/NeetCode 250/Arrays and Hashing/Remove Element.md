# Problem

We're given a reference to a `vector<int> nums` and an `int val`, and we must remove all occurrences in `val` from `nums` *in-place*, returning the *number of elements* in `nums` that aren't equal to `val`.

**IMPORTANT**: The custom judge for this problem does not care whether the true size of `nums` has changed after running your code, only considering the elements up to the number returned by the function (the number of non-`val` elements in the list).
# Solution

Since we have to modify `nums` in-place, we can't create a new vector, loop through `nums`, and only push back when `nums[i] != val`. The brute force approach would be to loop through `nums` and erase all the elements that are equal to `val`:

```cpp
for (auto it = nums.begin(); it != nums.end(); ) {
	if (*it == val) {
		it = nums.erase(it);
	} else {
		it++;
	}
}
return nums.size();
```

In `C++`, `vector::erase` takes in an iterator, not an index. While we can convert between the two, it's easier to use the iterator approach for this problem. `vector::erase` will return the iterator of the new element at the corresponding index after erasure, which is why we only increment `it` when we don't erase. This has time complexity $O(n)$.

However, the custom judge doesn't actually require us to erase any elements, just that there are no elements equaling `val` within the first $k$ elements, where $k$ is the number returned by the function. This means that, instead of performing costly erases, we can instead simply swap the element with a non-`val` element at the end of the list:

```cpp
int i = 0;
int j = nums.size() - 1;
while (i <= j) {
	if (nums[i] == val) {
		nums[i] = nums[j--];
	} else {
		i++;
	}
}
return i;
```

Note that we never have to perform the whole swap as we don't need to preserve the `val` entries. This solution also has $O(n)$ time complexity.