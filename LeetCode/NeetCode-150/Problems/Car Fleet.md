---
topic: Stack
difficulty: Medium
leetcode: https://leetcode.com/problems/car-fleet/
---
# Car Fleet

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/car-fleet/)

> Time to target for each car, sorted by position. A car that catches up joins the fleet ahead.

## Notes

I could not understand the intuition first, but then read some part of this article, not completely https://algo.monster/liteproblems/853

The idea is the get the time it will take for each car to reach the target, and sort this data according the position and the start from backwards, and we know now that if an priori car has curr_car_time > previous_car_time: then they will meet.

You can also go forwards and use a stack additionally.

## Complexity

O(nLOgn); S(n)

## Code

```cpp
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int size = static_cast<int>(position.size());
        
        vector<pair<int, double>> position_time(size);

        for(int i = 0; i < size; ++i) {
            position_time[i] = {position[i], (double)(target - position[i])/speed[i]};
        }

        sort(position_time.begin(), position_time.end());

        stack<double> stack_fleet;
        for(int i = 0; i < size; ++i) {
            double curr_car_travel_time = position_time[i].second;
            double next_car_travel_time = position_time[i + 1].second;
            while(!stack_fleet.empty() && stack_fleet.top() <= curr_car_travel_time) {
                stack_fleet.pop();
            }
            stack_fleet.push(curr_car_travel_time);
        }

        return stack_fleet.size();
    }
};
```
