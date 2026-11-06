---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/palindrome-partitioning/
---
# Palindrome Partitioning

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/palindrome-partitioning/)

> Precompute palindromes starting at each index, then backtrack over them.

## Problem

Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

Input
s =
"cabacghaba"
Output
[["c","a","b","a","c","g","h","a","b","a"],["c","a","b","a","c","g","h","aba"],["c","aba","c","g","h","a","b","a"],["c","aba","c","g","h","aba"],["cabac","g","h","a","b","a"],["cabac","g","h","aba"]]
Expected
[["c","a","b","a","c","g","h","a","b","a"],["c","a","b","a","c","g","h","aba"],["c","aba","c","g","h","a","b","a"],["c","aba","c","g","h","aba"],["cabac","g","h","a","b","a"],["cabac","g","h","aba"]]

## Notes

I thought I only needed to output the palindrome strings, all.

but question asks for list of possible partitions. Each can include different palindromes. As this question required to get all the solutions, this is mostly backtracking, there is pre complutation for the palindromes starting at each index, each char being the stand alone palindrome. Once this ds is build we need to go through the list of palindromes for each and backtrack:

For(palindromes_starting_at_i) {
 curr_palindromes.push(i's_palindromes)
 backtrack(next_pos_after_palindrome_end)
 curr_palindromes.pop_back()
}

I am proud of this solution of mine, I struggled but did not give up.

## Complexity

I do not understand the complexity.

## Code

```cpp
/_
in this whole solution, we will fix a center and go on left and righte
_/

class Solution {
public:
    void GetAllPartitions(int pos, vector<string> &curr_sel, const string &s, vector<vector<string>> &res, vector<vector<int>> & palindromes) {
        if (pos >= s.size()) {
            res.push_back(curr_sel);
            return;
        }

        for (int end: palindromes[pos]) {
            curr_sel.push_back(s.substr(pos, end - pos + 1));
            GetAllPartitions(end + 1, curr_sel, s, res, palindromes);
            curr_sel.pop_back();
        }
    }

    vector<vector<int>> CreatePalidromesData(const string &s) {
        vector<vector<int>> res(s.size());
        for (int i = 0; i < s.size(); ++i) {
            for (int palindrome_size = 0; i - palindrome_size >=0 && i + palindrome_size < s.size(); ++palindrome_size) {
                if (s[i - palindrome_size] == s[i + palindrome_size]) {
                    res[i - palindrome_size].push_back(i + palindrome_size);
                } else break;
            }
        }

        for (int i = 1; i < s.size(); ++i) {
            bool two_middles = s[i] == s[i - 1];
            if (two_middles) {
                for (int palindrome_size = 0; i - palindrome_size - 1 >=0 && i + palindrome_size < s.size(); ++palindrome_size) {
                    if (s[i - palindrome_size - 1] == s[i + palindrome_size]) {
                        res[i - palindrome_size - 1].push_back(i + palindrome_size);
                    } else break;
                }
            }
        }

        return res;
    }

    vector<vector<string>> partition(string s) {
        vector<string> temp_sel;
        vector<vector<string>> res;
        vector<vector<int>> palindromes = CreatePalidromesData(s);

        GetAllPartitions(0, temp_sel, s, res, palindromes);
        return res;
    }
};
```
