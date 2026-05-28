# Problem

Given a reference to a `vector<int> nums`, return the *majority element*, defined as the element that appears more than $\lfloor n/2\rfloor$ times, where $n$ is `nums.size()`. Assume that the majority element always exists in the array.
# Solution

Notice that the majority element will take up more than half of the space in the array. This means that, no matter what, we can sort the array and the majority element will appear at the midpoint:

```cpp
sort(nums.begin(), nums.end());
return nums[nums.size() / 2];
```

This has time complexity $O(n\log n)$, but ideally we'd only need to look at each element once to decide which one is the majority element.

Since the majority element appears more than half the time, if we subtracted the number of times the majority element appears in `nums` by the number of times it doesn't, we'd get a positive number. Also, if we partition `nums` on index `k`, the majority element of `nums` must be at least one of the majority element of `nums[..k]` or `nums[k..]`. This means we can consider the first element our "candidate" for the majority element, increment a counter every time it occurs and decrement it every time it doesn't. If we reach $0$ at index `k`, then we're safe to choose a new candidate, as either the majority element was not our candidate or it's also the majority element of the subarray `nums[k..]` and we'll rediscover it:

```cpp
int c = 1;
int cand = nums[0];
for (int i = 1; i < nums.size(); i++) {
	if (c == 0) cand = nums[i];
	if (nums[i] == cand) c++;
	else c--;
}
return cand;
```

This has time complexity $O(n)$ as desired, and only $O(1)$ space.