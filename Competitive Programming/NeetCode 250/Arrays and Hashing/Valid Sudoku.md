# Problem

Given a reference to a `vector<vector<char>> board`, return whether it's valid or not. A `board` is valid **if** the rows, columns. and $3$-by-$3$ grids all contain the digits $1$ through $9$ without repetition. The `board` may not be completely filled.
# Solution

There's no fast way to do this, so we'll just have to brute force it:

```cpp
auto seen = unordered_set<int>();
for (int i = 0; i < board.size(); i++) {
	seen.clear();
	for (int j = 0; j < board[0].size(); j++) {
		if (board[i][j] != '.') {
			if (seen.find(board[i][j]) != seen.end()) {
				return false;
			}
			seen.insert(board[i][j]);
		}
	}
}
for (int j = 0; j < board[0].size(); j++) {
	seen.clear();
	for (int i = 0; i < board.size(); i++) {
		if (board[i][j] != '.') {
			if (seen.find(board[i][j]) != seen.end()) {
				return false;
			}
			seen.insert(board[i][j]);
		}
	}
}
for (int s = 0; s < 9; s++) {
	seen.clear();
	for (int c = 0; c < 9; c++) {
		int i = (s % 3) * 3 + (c % 3);
		int j = (s / 3) * 3 + (c / 3);
		if (board[i][j] != '.') {
			if (seen.find(board[i][j]) != seen.end()) {
				return false;
			}
			seen.insert(board[i][j]);
		}
	}
}
return true;
```