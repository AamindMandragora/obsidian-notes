# Problem

We are given a reference to a `vector<int> nums`, and we must return `true` if `nums` contains a duplicate entry and `false` otherwise. The size of `nums` will be between $1$ and $10^5$.
# Solution

There are a couple different ways we can do this. The brute force approach is to set up two `for` loops iterating through every pair of elements and checking whether they are equal:

```cpp
for (int i = 0; i < nums.size(); i++) {
	for (int j = i + 1; j < nums.size(); j++) {
		if (nums[i] == nums[j]) return true;
	}
}
return false;
```

However, an $O(n^2)$ solution can definitely be improved upon, since if we've already seen $k$ elements, there has to be a quick way to check if the $k+1\text{th}$ element is in that set. My first instinct for an optimization was to create an `unordered_set<int> distinct` from `nums` and return whether their lengths were different (if they were, a duplicate must exist):

```cpp
auto distinct = unordered_set<int>(nums.begin(), nums.end());
return distinct.size() != nums.size();
```

This solution has $O(n)$ time complexity through creating the set, which makes sense as we would need to check every element to ensure that no two are the same. To speed this up in the average case, we could start with an empty set and reserve enough space to hold `nums`, then loop through each element, attempt to insert it, and check if the actual size of the set is what we'd expect:

```c
int n = nums.size();
unordered_set<int> distinct;
distinct.reserve(n);
for (int i = 0; i < n; i++) {
	distinct.insert(nums[i]);
	if (distinct.size() != n) return true;
}
return false;
```

However, in both cases, the space complexity is $O(n)$ as well, prompting us to look for a more efficient solution. Notice that there's no indication that the state of `nums` should be preserved; therefore, we can sort the vector in place in $O(n\log n)$ time and scan for duplicate entries in $O(n)$:

```c
sort(nums.begin(), nums.end());
for (int i = 1; i < nums.size(); i++) {
	if (nums[i - 1] == nums[i]) return true;
}
return false;
```

While the time complexity is higher, our relatively low maximum array size means that actually creating and populating the set will take more time.