---
topic: 1D DP
difficulty: Medium
leetcode: https://leetcode.com/problems/word-break/
---
# Word Break

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/word-break/)

> dp[i] = true if a word ends at i and dp[i − len] is true. Trie + memo also works.

## Problem

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

## Notes

The problem I faced here was relying on the recall and not fresh problem solving. I solved it easily using trie but given different constraints, other ways of solving dp would have been mpre effective.

I solved by creating a trie for the dictionary Once we have it we can start searching the prefix of the target word adn create the dp branch from the next character if the word is found. For the given prefix that matched we will keep on extending in the trie and getting the result of all the subproblems after it and see of any of them is true, and return true if that is the case. This is top down.

The other solution using the bottom up dp is, in that the trick is to track the end of the words.

We use first for loop for indexing onto the target string, then see if starting from this index, any word matches by using a nested for loop on the dict words. We also see, if say this i, i - word.length is true or not, means that are we looking into the next word, this way for the last endex we will have a 1 or 0.

int size = nums.size();

 int sum = 0;
 for(int n: nums) sum += n;

 if(sum % 2 == 1) return false;
 int target = sum/2;
 
 vector<bool> dp(target + 1, false);
 dp[0] = true;

 for(int i = 1; i <= size; ++i) {
 int ele = nums[i - 1];
 for(int j = target; j >= ele; --j) {
 dp[j] = dp[j] | dp[j - ele];
 }
 }

 return dp[target];

## Complexity

O(n_n + m_k)
n: length of the string to be matched
m: length of the wordDIct
k: max length of words in wordDict

## Code

```cpp
int size = static_cast<int>(s.size());

        if(mem[st] != -1) {
            return mem[st];
        }

        TrieNode* word_trie = trie;

        for(int i = st; i < size; ++i) {
            int char_idx = s[i] - 'a';
            if(word_trie->next[char_idx] == nullptr) return mem[st] = false;
            else word_trie = word_trie->next[char_idx];

            if(word_trie->is_end == true) {
                if(dp(s, i + 1, mem)) {
                    return mem[st] = true;
                }
            }
        }

        return mem[st] = false;
```
