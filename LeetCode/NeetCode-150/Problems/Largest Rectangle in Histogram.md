---
topic: Stack
difficulty: Hard
leetcode: https://leetcode.com/problems/largest-rectangle-in-histogram/
---
# Largest Rectangle in Histogram

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/largest-rectangle-in-histogram/)

> Increasing stack. On pop: current i is the right boundary, new top is the left.

## Problem

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

Input: heights = [2,1,5,6,2,3]
Output: 10

## Notes

My initial intuition was to get the left and the right limit for each number anf then calculate the area, then problem is the optimization with a single pass of stack is possible and that is also expected in google l5.

So area will be max till we see a smaller piallr on the left, and same for right.

But developing an intuition was a bit difficult.

The idea is if we will maintain a monotonically increasing stack, whenever we see a pillar less than our top pillar we are sure that this smaller pillar is the right boundary for the top pillar on the stack, my confusion was, what about the left boundary of the top pillar, the answer is in the monotnically increasing stack.

We know that if we will take the top pillar out, we will left with the left boundary of the taken out pillar, as we notice that it is always the nearst smaller element to the taken out pillar.

This way the area can be calculated and we can check for the max.

## Complexity

O(n), S(n)

## Code

```cpp
stack<int> stk_mono;
        int size = static_cast<int>(heights.size());
        
        stk_mono.push(-1);
        int res = 0;

        for(int i = 0 ; i <= size; ++i) {
            int current_number = i == size? 0 : heights[i];
            while(stk_mono.top() != -1 && current_number <= heights[stk_mono.top()]) {
                //pop the current
                int height = heights[stk_mono.top()];
                stk_mono.pop();
                //get left boundary from the top element of the stack
                int width = i - stk_mono.top() - 1;
                res = max(res, height*width);
            }
            stk_mono.push(i);
        }

        return res;
```
