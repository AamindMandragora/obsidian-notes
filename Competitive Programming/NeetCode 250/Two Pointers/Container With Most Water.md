# Problem

Given a reference to a `vector<int> height`, consider `n` vertical lines drawn at `x = i` from `y = 0` to  `y = height[i]` for all `i`, then find the maximum possible area of a container that can be created by using two of the lines and the $x$-axis.
# Solution

The brute force solution is to consider every pair of elements, calculate the area, and update an `int m` to hold the max such area and return:

```cpp
int m = 0;
for (int i = 0; i < height.size(); i++) {
	for (int j = i + 1; j < height.size(); j++) {
		m = max(m, (j - i) * min(height[i], height[j]));
	}
}
return m;
```

This is an $O(n^2)$ time solution, but we should be able to find one in $O(n)$. The area of the box is bounded by the difference between `j` and `i` and the minimum height of the associated elements. We can start by maintaining two pointers at the ends of the array and changing whichever one corresponds to a smaller height:

```cpp
int m = 0, i = 0, j = height.size() - 1;
while (i < j) {
	m = max(m, (j - i) * min(height[i], height[j]));
	if (height[i] < height[j]) i++;
	else j--;
}
return m;
```