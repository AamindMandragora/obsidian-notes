# Problem

We are given a reference to a `vector<int> nums` with size `n`, and we must return a `vector<int> ans` with size `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]`. In other words, `ans` should be a concatenation of `nums` with itself.
# Solution

The first thing we will do is set up our `ans` vector with a capacity of `2n` `int`s:

```cpp
int n = nums.size();
auto ans = vector<int>(2 * n);
```

Then, we'll have to loop through each element in `nums` and set the corresponding elements of `ans` equal to it:

```cpp
for (int i = 0; i < n; i++) {
	ans[i] = nums[i];
	ans[i + n] = nums[i];
}
```

Finally, we can return `ans`. This solution is $O(n)$, which is as good as we can do when copying vectors.