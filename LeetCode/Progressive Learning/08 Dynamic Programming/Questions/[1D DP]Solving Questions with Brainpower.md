https://leetcode.com/problems/solving-questions-with-brainpower/description/

```cpp
class Solution {
	public:
	
	long long mostPoints(vector<vector<int>>& questions) {
	
		int n = questions.size();	
		vector<long long> dp(n + 1, 0);
		for(int j = n - 1; j >= 0; --j) {
			dp[j] = max((long long)questions[j][0], dp[j + 1]);	
			int skip_questions = questions[j][1];
			if(j + skip_questions + 1 < n) {
				dp[j] = max(dp[j], (long long)questions[j][0] + dp[j + skip_questions + 1]);
			}
		}
		return dp[0];
	}
};
```