# Problem

Given a reference to a `vector<int> nums`, return a `vector<int> answer` such that `answer[i]` equals the product of all the elements in `nums` except `nums[i]`. This algorithm must run in linear time and must not use the division operator.
# Solution

We can create two vectors, `prefix` and `suffix`, then turn them into their respective products. Finally, we can construct and return our `vector<int> answer`:

```cpp
auto prefix = vector<int>(nums.size(), 1);
auto suffix = vector<int>(nums.size(), 1);
for (int i = 1; i < nums.size(); i++) {
	prefix[i] = prefix[i - 1] * nums[i - 1];
	suffix[nums.size() - i - 1] = suffix[nums.size() - i] * nums[nums.size() - i];
}
auto answer = vector<int>(nums.size());
for (int i = 0; i < nums.size(); i++) {
	answer[i] = prefix[i] * suffix[i];
}
return answer;
```

If we don't want to create two new vectors, we can hold the prefix and suffix information in integers, then construct `answer` as we go:

```cpp
auto answer = vector<int>(nums.size(), 1);
int prefix = 1;
for (int i = 1; i < nums.size(); i++) {
	prefix *= nums[i - 1];
	answer[i] *= prefix;
}
int suffix = 1;
for (int i = nums.size() - 2; i >= 0; i--) {
	suffix *= nums[i + 1];
	answer[i] *= suffix;
}
return answer;
```