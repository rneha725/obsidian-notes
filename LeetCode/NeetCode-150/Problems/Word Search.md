---
topic: Backtracking
difficulty: Medium
leetcode: https://leetcode.com/problems/word-search/
---
# Word Search

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/word-search/)

> DFS from each cell. Mark visited in place and restore after.

## Problem

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

## Notes

I did not prune the solution well, if having a seq of ifs helped.

Also, in place modification was also important.

## Complexity

m_n_3^k; S(k)

## Code

```cpp
//Run for each cell

bool backtrack(vector<vector<char>>& board, const string &word, int word_i, int row, int col) {
        if(word_i == word.size()) return true;
        
        if(row >= board.size() | row < 0 | col >= board[0].size() | col < 0 | board[row][col] == '0' | board[row][col] != word[word_i]) return false;

        bool answer = false;
        char temp = board[row][col];
        board[row][col] = '0';
        if(backtrack(board, word, word_i + 1, row - 1, col)) return true;
        if(backtrack(board, word, word_i + 1, row + 1, col)) return true;
        if(backtrack(board, word, word_i + 1, row, col + 1)) return true;
        if(backtrack(board, word, word_i + 1, row, col - 1)) return true;
        board[row][col] = temp;
    
        return answer;
    }
```
