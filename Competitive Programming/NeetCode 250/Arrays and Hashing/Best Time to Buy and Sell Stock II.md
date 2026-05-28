# Problem

We're given a reference to a `vector<int> prices` where `prices[i]` is the cost of a given stock on the $i\text{th}$ day. We may only hold one share of the stock at any given time, but we are allowed to buy and sell multiple times on any given day. Find and return the **maximum** possible profit.
# Solution

We know that we'll have to buy a share before we can sell it, so we'll start by pretending that we buy a share on the very first day at `prices[0]`. If, on the next day, the price goes up, we'll save `prices[1]` as the price we sell at. Otherwise, we'll just pretend we bought on the second day instead of the first. Alternatively, if we decide to sell at `prices[1]` and the prices goes lower, we'll just buy again. Otherwise, we'll just pretend we sold at `prices[2]`. In this way, we can get the max profit in a single pass:

```cpp
int profit = 0;
int left = prices[0];
int right = prices[0];
for (int i = 1; i < prices.size(); i++) {
	if (prices[i] < left) {
		if (right - left > 0) profit += right - left;
		left = prices[i];
		right = prices[i];
	} else if (prices[i] > right) {
		right = prices[i];
	} else {
		if (right - left > 0) profit += right - left;
		left = prices[i];
		right = prices[i];
	}
}
if (right - left > 0) profit += right - left;
return profit;
```