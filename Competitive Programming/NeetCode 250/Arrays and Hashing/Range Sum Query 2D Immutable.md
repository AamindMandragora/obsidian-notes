# Problem

Create the `NumMatrix` class, which takes a reference to a `vector<vector<int>> matrix`and has one method: `int sumRegion(int row1, int col1, int row2, int col2)`, which needs to return the sum of all the squares in the `matrix` bounded by the two points `(row1, col1)` and `(row2, col2)` **in $O(1)$ time**.
# Solution

To get an $O(1)$ `sumRegion`, we'll need to do most of the work in the constructor. In `NumMatrix(vector<vector<int>>& matrix)`, we will instantiate a private field `vector<vector<int>> prefix`, where `prefix[row][col]` will contain the area from `(0, 0)` to `(row, col)`:

```cpp
NumMatrix(vector<vector<int>>& matrix) {
	if (matrix.empty() || matrix[0].empty()) {
		prefix = {{0}};
		return;
	}
	prefix = vector<vector<int>>(matrix.size() + 1, vector<int>(matrix[0].size() + 1, 0));
	for (int i = 1; i <= matrix.size(); i++) {
		for (int j = 1; j <= matrix[0].size(); j++) {
			prefix[i][j] = matrix[i - 1][j - 1] + prefix[i - 1][j] + prefix[i][j - 1] - prefix[i - 1][j - 1];
		}
	}
}
```

Now, we can have `sumRegion` simply calculate the prefix at `(row2, col2)`, then subtract the prefixes defined by `(row2, col1 - 1)` and `(row1 - 1, col2)`, then add `(row1 - 1, col1 - 1)`:

```cpp
int sumRegion(int row1, int col1, int row2, int col2) {
    return prefix[row2 + 1][col2 + 1] - prefix[row1][col2 + 1] - prefix[row2 + 1][col1] + prefix[row1][col1];
}
```