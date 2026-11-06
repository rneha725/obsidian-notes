---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/maximum-earnings-from-taxi/
---
# Maximum Earnings From Taxi

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/maximum-earnings-from-taxi/)

> DP on the number line: dp[i] = max(dp[i−1], dp[start] + earning) for rides ending at i.

## Problem

There 3 problems have fucked up my mind.

## Notes

This is same as the maximum profit in Job Scheduling, all same, but this is getting solved in the O(n + m), you know why? because here the length of the line where intervals reside, it is within the integer boundaries.

Here, the solution is consider we are riding on a road, we can take a ride or not. We create an array storing the end time as the index and a vector of {startTime, earning} as the value.

We will be on the number line and dp[i] = dp[i-1] -> to propagate the value previously calculated.
And we do this, for each ride ending at i, we will go though each ride and do this dp[i] = max(dp[i], dp[start] + earning_for_that_ride)

## Complexity

O(n + m)

## Code

```cpp
class Solution {
public:
    using pii=pair<int, int>;
    long long maxTaxiEarnings(int n, vector<vector<int>>& rides) {
        vector<vector<pii>> rides_by_end_time(n + 1);

        for(vector<int> ride: rides) {
            int start = ride[0];
            int end = ride[1];
            int earning = end - start + ride[2];

            rides_by_end_time[end].push_back({start, earning});
        }

        vector<long long> dp(n + 1, 0);

        for(int i = 1; i <= n; ++i) {
            dp[i] = dp[i - 1];
            for(pii r: rides_by_end_time[i]) {
                dp[i] = max(dp[i], dp[r.first] + r.second);
            }
        }

        return dp[n];
    }
};
```
