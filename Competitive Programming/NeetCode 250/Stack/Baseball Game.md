# Problem

We are given a reference to a `vector<string> operations` that are each one of the following:
- An integer `x`; record the score of `x`
- `'+'`; record a score that's the sum of the previous two
- `'D'`; record a score that's double the previous score
- `'C'`; remove the previous score from the record
Return the *sum of all* the scores on the record after applying all the operations.
# Solution

We'll always either be adding to the record or performing operations on scores recently added to the record, so we can use a `stack<int>` to hold our scores:

```cpp
stack<int> record;
for (string& s : operations) {
	if (isdigit(s[0]) || s[0] == '-') {
		record.push(stoi(s));
	} else if (s == "+") {
		int a = record.top();
		record.pop();
		int b = record.top();
		record.push(a);
		record.push(a + b);
	} else if (s == "D") {
		int a = record.top();
		record.push(2 * a);
	} else if (s == "C") {
		record.pop();
	}
}
int sum = 0;
while (!record.empty()) {
	sum += record.top();
	record.pop();
}
return sum;
```