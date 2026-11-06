---
topic: Backtracking
difficulty: hard
leetcode: https://leetcode.com/problems/n-queens/
---
# N Queens

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/n-queens/)

> Go row by row. Track attacked cells: place, recurse, undo.

## Problem

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

## Notes

Easy backtrack

## Complexity

n!

## Code

```cpp
class Solution {
public:
    void AddQueenAttackCell(vector<vector<int>> &board_state, const int row, const int col, const int freq) {
        for(int i = row + 1; i < board_state.size(); ++i) {
            board_state[i][col] += freq;
        }

        for(int i = 1; row + i < board_state.size(); ++i) {
            if(col + i < board_state[0].size()) board_state[row + i][col + i] += freq;
            if(col - i >= 0) board_state[row + i][col - i] += freq;
        }

        board_state[row][col] += freq;
    }

    void GetAllPossibilities(const int row, vector<string>& board, vector<vector<string>> &res, vector<vector<int>> &board_state) {
        if(row >= board.size()) {
            res.push_back(board);
            return;
        }

        for(int i = 0; i < board[row].size(); ++i) {
            if(board_state[row][i] != 0) continue;
            char ch = board[row][i];
            board[row][i] = 'Q';
            AddQueenAttackCell(board_state, row, i, 1);
            GetAllPossibilities(row + 1, board, res, board_state);
            AddQueenAttackCell(board_state, row, i, -1);
            board[row][i] = '.';
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<string> board(n, string(n, '.'));
        vector<vector<string>> res;
        res.reserve(n);
        vector<vector<int>> board_state(n, vector<int>(n, 0));
        GetAllPossibilities(0, board, res, board_state);

        return res;
    }
};
```
