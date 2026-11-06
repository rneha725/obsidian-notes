---
topic: Heap
difficulty: Medium
leetcode: https://leetcode.com/problems/task-scheduler/
---
# Task Scheduler

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/task-scheduler/)

> Max-heap per cycle, or math: max((maxFreq−1)(n+1) + countOfMax, tasks).

## Problem

You are given an array of CPU tasks, each labeled with a letter from A to Z, and a number n. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of at least n intervals between two tasks with the same label.

Return the minimum number of CPU intervals required to complete all tasks.

Example 1:

Input: tasks = ["A","A","A","B","B","B"], n = 2

Output: 8

Explanation: A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.

## Notes

First I was really confused with this question regarding how do I know if I am building the minumum nunber of cycle string by picking the one which has the most freq. 

But I did not fight it much and assumed that I need to pick the most freq one from the processes one.

I did this in phases, for me dircetly coming onto the most efficient solution did not work
One solution was using heap:

Where I am first creating the freq map and then putting those frequencies in a heap, no need to put any other data.

The popping from the heap and then pushing them to a temp storage to add them later into the heap, once we have calculated for one cycle then push the decremented frequencies again in the heap.
Loop this till the heap is empty. Also for every cycle, use while loop till either the cycle is over or the heap.

When the elements are present to complete an interval we can easily add the cycle_size in the total_number_of_cycles, but at the end we only need to schedule remaining elements. So we might not need to complete a full interval.

Once this solution was done, it was easier to see how to solve it using maths.

int max_freq_process_count = 0;
 for(int freq: freq_map) {
 if(freq == max_freq) max_freq_process_count++;
 }

 int res = (max_freq - 1)*(n + 1) + max_freq_process_count;
 
 return max(res, static_cast<int>(tasks.size()));

## Complexity

O(n) S(n)

## Code

```cpp
Suboptimal heap solution:

vector<int> freq_map(26, 0);

        for(char task: tasks) {
            freq_map[task - 'A']++;
        }

        priority_queue<int> max_heap_freq;
        for(int freq: freq_map) {
            if(freq != 0) max_heap_freq.push(freq);
        }

        int res = 0;
        while(!max_heap_freq.empty()) {
            int cycle_size = n + 1;
            vector<int> temp_freq_storage;

            int task_complete_count = 0;
            while(cycle_size-- && !max_heap_freq.empty()) {
                int new_freq = max_heap_freq.top() - 1;
                max_heap_freq.pop();
                if(new_freq != 0) temp_freq_storage.push_back(new_freq); 
                task_complete_count++;
            }

            for(int new_freq: temp_freq_storage) {
                max_heap_freq.push(new_freq);
            }

            if(!max_heap_freq.empty()) {
                res += n + 1;
            }
            else {
                res += task_complete_count;
            }
        }

        return res;

    }
```
