# Problem

Given a reference to a `vector<int> nums` and an `int k`, return the `k` most frequent elements in any order. In other words, return the most frequent element, the second most frequent element, and so on until the $k\text{th}$ most frequent element. It's guaranteed that the answer is unique, and your algorithm must be better than $O(n\log n)$ time.
# Solution

We can try maintaining a frequency list using the elements as indices:

```cpp
int freq[20001] = {0};
for (int n : nums) {
	freq[n + 10000]++;
}
```

Then, since the frequencies are integers too, we can invert the array, swapping keys and values and making sure to track the largest frequency. However, since multiple numbers can have the same frequency, we'll instead use a `vector<vector<int>>`:

```cpp
auto inv = vector<vector<int>>(20001);
int max = 0;
for (int i = 0; i < 20001; i++) {
	max = (max > freq[i]) ? max : freq[i];
	inv[freq[i]].push_back(i - 10000);
}
```

Finally, we can loop backwards from `max` and add the `k` most frequent elements to our `vector<int> kmost`:

```cpp
auto kmost = vector<int>(k);
for (; max >= 0 && k > 0; max--) {
	for (int i = 0; i < inv[max].size() && k > 0; i++) {
		kmost[k - 1] = inv[max][i];
		k--;
	}
}
return kmost;
```

This solution is technically $O(n)$ time, but in practice is so bad due to the huge allocations that something with worse time complexity could easily outperform it. The first thing we'll do to combat this is trade our frequency list for a map:

```cpp
auto freq = unordered_map<int, int>;
for (int n : nums) {
	freq[n]++;
}
```

Then, we can get a vector of unique elements by iterating through the pairs:

```cpp
auto unique = vector<int>();
for (auto& f : freq) {
	unique.push_back(f.first);
}
```

Now, we can just sort `unique` by the corresponding frequencies instead of going through all the trouble to invert the list. Since we can return the top `k` in any order, we don't even have to fully sort. We just need some way to ensure that every element in `unique[n - k..]` is in the top `k` frequent elements. In other words, we'll need to partition `unique` at the $k\text{th}$ most frequent element. If we choose a random such element and partition, then its final index can be higher, lower, or equal to `k`. In the first two cases, we just recurse on the subarray that contains `k`, and in the last, we return a `vector<int>` made from the last `k` elements in the list.

First, let's write `partition`, which takes in a reference to a `vector<int> nums`, `unordered_map<int, int> freq`, `int left`, and `int right`, then partitions `nums[left..right]` in place around the median element based on frequency, then returns the index that splits the two subarrays. We guarantee that everything in `nums[left..j]` has a frequency less than or equal to the pivot, and everything in `nums[j+1..right]` has a frequency greater than or equal to the pivot. However, `j` itself may not be in its final sorted position:

```cpp
int partition(vector<int>& nums, unordered_map<int, int>& freq, int left, int right) {
	int idx = (left + right) / 2;
	int pivot = freq[nums[idx]];
	int i = left - 1; int j = right + 1;
	while (true) {
		do {i++;} while (freq[nums[i]] < pivot);
		do {j--;} while (freq[nums[j]] > pivot);
		if (i >= j) return j;
		swap(nums[i], nums[j]);
	}
}
```

Now, we'll write `topKFrequent`, which will make the frequency map and repeatedly call `partition` until our bounds cross, which will ensure that the last `k` elements in the list are the top `k`.

```cpp
vector<int> topKFrequent(vector<int>& nums, int k) {
	auto freq = unordered_map<int, int>();
	for (int n : nums) {
		freq[n]++;
	}
	auto unique = vector<int>();
	for (auto& f : freq) {
		unique.push_back(f.first);
	}
	int left = 0, right = unique.size() - 1;
	int target = unique.size() - k;
	while (left < right) {
		int p = partition(unique, freq, left, right);
		if (p < target) {
			left = p + 1;
		} else {
			right = p;
		}
	}
	return vector<int>(unique.end() - k, unique.end());
}
```

This whole algorithm has time complexity $O(n)$ in the average case (degrades to $O(n^2)$ depending on the list and pivot) and takes up $O(n)$ space.