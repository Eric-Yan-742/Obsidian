TikTok 25summer Aug10-14 OA

![[_attachments/image 2.png|image 2.png]]

![[image 1.png]]

- Convert `min > limit / length` to `min * length > limit` . For each `nums[i]`, we need to find the max length of the subarray such that `nums[i]` is the min among the subarray. They cover all the subarrays possible.
- To find those max length, we need to use monotonic stacks to find the closest element smaller than `nums[i]` on the left and on the right of `nums[i]` . With the range `(left, right)` , we will have `maxLength = left - right + 1` .

```C++
int getMaxGoodSubarrayLength(int limit, vector<int>& financialMetrics) {
	int n = financialMetrics.size();
	vector<int> leftSmaller(n, -1);
	vector<int> rightSmaller(n, 5);
	int res = 0;

	stack<int> sLeft;
	// Use a monotonic increasing stack to find the closest smaller element on the left
	// Note we traverse backward
	for (int i = n - 1; i >= 0; i--) {
		while (!sLeft.empty() && financialMetrics[i] < financialMetrics[sLeft.top()]) {
			// stack stores the index.
			leftSmaller[sLeft.top()] = i;
			sLeft.pop();
		}
		sLeft.push(i);
	}
	// Use a monotonic increasing stack to find the closest smaller element on the right
	// Note we traverse forward
	stack<int> sRight;
	for (int i = 0; i < n; i++) {
		while (!sRight.empty() && financialMetrics[i] < financialMetrics[sRight.top()]) {
			rightSmaller[sRight.top()] = i;
			sRight.pop();
		}
		sRight.push(i);
	}

	// get maxLength for each nums[i], if it meets the standard, we update the return value
	for (int i = 0; i < n; i++) {
		int maxLength = rightSmaller[i] - leftSmaller[i] - 1;
		if (financialMetrics[i] * maxLength > limit)
			res = max(res, maxLength);
	}

	return res;
}
```