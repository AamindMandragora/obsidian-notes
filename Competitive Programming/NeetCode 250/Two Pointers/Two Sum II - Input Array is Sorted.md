# Problem

Given a reference to a `vector<int> numbers` that's sorted in **non-decreasing order**, find two numbers that add up to an `int target` (a unique solution is guaranteed) and return their indices (with respect to a 1-indexed system) in a `vector<int>`. Use only constant extra space. You may not use the same index twice.
# Solution

Since the array is sorted, we can start with two pointers on the outside and see whether their sum is larger or smaller than `target`. Depending on that, we can increment one of the pointers and check again, finally returning the indices plus one:

```cpp
int i = 0; int j = numbers.size() - 1;
while (i < j) {
	if (numbers[i] + numbers[j] > target) j--;
	else if (numbers[i] + numbers[j] < target) i++;
	else break;
}
return {i + 1, j + 1};
```