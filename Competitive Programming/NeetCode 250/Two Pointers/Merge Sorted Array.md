# Problem

You are given references to a `vector<int> nums1` with size $m+n$ and a `vector<int> nums2` with size $n$. Both arrays are sorted in non-decreasing order and the last $n$ elements in `nums1` are `0` to accommodate `nums2`. Merge the two arrays within `nums1`.
# Solution

To avoid corruption, we're going to perform this merge backwards. We will maintain pointers to the current elements of `nums1` and `nums2`, compare them, place the result in the index corresponding to the sum of the pointers, and decrement whichever pointer was chosen:

```cpp
if (n == 0) return;
while (m + n > 0) {
	if (n == 0 || (m != 0 && nums1[m - 1] > nums2[n - 1])) {
		nums1[m + n - 1] = nums1[m - 1];
		m--;
	} else {
		nums1[m + n - 1] = nums2[n - 1];
		n--;
	}
}
```