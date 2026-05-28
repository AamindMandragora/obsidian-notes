# Problem

Given a reference to a `vector<int> nums`, rotate it to the right by `int k` steps, where `k` is nonnegative.
# Solution

The brute force approach is to create a `vector<int> copy` of `nums`, then regenerate `nums` by adding `k` to the indexes and taking the result `mod k`:

```cpp
auto copy = vector<int>(nums.begin(), nums.end());
for (int i = 0; i < nums.size(); i++) {
	nums[(i + k) % nums.size()] = copy[i];
}
```

However, this has a space complexity of $O(n)$. Notice that when we rotate an array by `k`steps, the last `k` elements of the array get moved to the front, and the other `nums.size() - k` elements get pushed to the end. Notably, the first element of `nums` will always follow the last element and be succeeded by the second element of `nums`. If we reverse the array, then our first `k` elements and our last `nums.size() - k` elements will be correct, just in the wrong order. At this point, we can just reverse the two subarrays:

```cpp
if (k == 0) return;
k = k % nums.size();
reverse(nums.begin(), nums.end());
reverse(nums.begin(), nums.begin() + k);
reverse(nums.begin() + k, nums.end());
```

Note that we have to constrain `k` to the size of `nums` so the iterators are in bounds.