https://leetcode.com/problems/number-of-closed-islands/

### Question
Given a 2D `grid` consists of `0s` (land) and `1s` (water).  An _island_ is a maximal 4-directionally connected group of `0s` and a _closed island_ is an island **totally** (all left, top, right, bottom) surrounded by `1s.`

Return the number of _closed islands_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2026/10/27/sample_3_1610.png)

**Input:** grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]

**Output:** 2

**Explanation:**

- The grid contains four islands of 0s.
- The island spanning rows 1 to 3 and columns 1 to 4 is surrounded entirely by 1s. It also encloses the water cell at row 2, column 2. It is therefore closed.
- The single 0 at row 3, column 6 has 1s on all four sides, so it is also closed.
- The two remaining islands both lie in column 7, which is on the border of the grid, so neither of them is closed.
- Two islands are closed, shown in gray, so the answer is 2.

