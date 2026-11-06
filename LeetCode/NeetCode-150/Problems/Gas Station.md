---
topic: Greedy
difficulty: Medium
leetcode: https://leetcode.com/problems/gas-station/
---
# Gas Station

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/gas-station/)

> Total gas < total cost → -1. Else restart at i+1 whenever the running tank goes negative.

## Problem

There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique.

Follow up: if there are multiple starting positions, how would that impact the solution?

## Notes

Knowing that there is one unique solution helped, also this is not an intuitive problem as NeetCode also pointed out.

First trick is to use a diff array, which tells us how possible it is start from this index and either go down or up. 
Another thing is to find the index as the starting point, which results into the all positive sum till the end.

We used the trick that we started from the 0th index, then if the cumulative sum < 0, then we know that from 0 to this position, nothing will give us the answer, so we wil keep the start position as i + 1 and then cumulative sime goes to 0.

## Complexity

O(n),. S(1)

## Code

```cpp
int size = static_cast<int>(gas.size());
        int sum = 0;
        int cumulative_sum = 0;
        int last_idx = 0;

        for(int i = 0; i < size; ++i) {
            cumulative_sum += gas[i] - cost[i];
            sum += gas[i] - cost[i];
            if(cumulative_sum < 0) {
                cumulative_sum = 0;
                last_idx = i + 1;
            }
        }

        if(sum < 0) return -1;
        return last_idx;
```
