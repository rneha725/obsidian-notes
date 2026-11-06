https://leetcode.com/problems/partition-array-for-maximum-sum/description/

#### Question
```
Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._
```

#### Answer
```cpp
class Solution {
	public:
	int maxSumAfterPartitioning(vector<int>& arr, int k) {
		int n = arr.size();
		vector<int> dp(n + 1, 0);
		for(int i = n - 1; i >= 0; --i) {
			int max_in_subarray = arr[i];
			for(int j = 1; j <= k && i + j <= n; ++j) {
				max_in_subarray = max(max_in_subarray, arr[i + j - 1]);
				dp[i] = max(dp[i], max_in_subarray*j + dp[i + j]);
			}
		}
		return dp[0];
	}
};
```

- Here, few optimizations were, it looked like 2d dp, as n and k can be dimensions, but when you look at it closely you will find that is not the case.
- Adding sentinel node helped with line: `dp[i] = max(dp[i], max_in_subarray*j + dp[i + j]);`, No extra condition.