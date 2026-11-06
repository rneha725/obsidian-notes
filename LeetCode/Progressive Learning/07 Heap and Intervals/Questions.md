# Heap & Intervals: Progressive Questions

**Asked at (2025–26 reports):** Google, Salesforce, DocuSign

**Signal:** 'k largest', scheduling, meeting rooms, overlapping ranges.

**Core idea:** For intervals, sort by start (or end), then sweep. Use a min-heap for 'what frees up next'.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 57 Insert Interval, 56 Merge Intervals, 435 Non-overlapping Intervals, 252/253 Meeting Rooms, 1851, 703, 1046, 973, 215, 621 Task Scheduler, 295 Find Median from Data Stream

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [986 Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/) | M | two pointers over two lists |  |  |
| [452 Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | M | sort by end, greedy |  |  |
| [1094 Car Pooling](https://leetcode.com/problems/car-pooling/) | M | sweep line or difference array |  |  |
| [767 Reorganize String](https://leetcode.com/problems/reorganize-string/) | M | max-heap; take the top two each time |  |  |
| [1834 Single-Threaded CPU](https://leetcode.com/problems/single-threaded-cpu/) | M | sort tasks + heap of available tasks |  |  |

## Stage 2: Twists (Medium → Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [1353 Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/) | M | sweep days + heap of end days |  |  |
| [2402 Meeting Rooms III](https://leetcode.com/problems/meeting-rooms-iii/) | H | two heaps: free rooms and busy rooms |  |  |
| [502 IPO](https://leetcode.com/problems/ipo/) | H | unlock projects with a heap |  |  |
| [759 Employee Free Time](https://leetcode.com/problems/employee-free-time/) | H (P) | merge all intervals, find gaps |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [632 Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | H | heap holding one element from each list |  |  |
| [218 The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | H | sweep line + max-heap |  |  |
| [715 Range Module](https://leetcode.com/problems/range-module/) | H | sorted map of intervals |  |  |
| [857 Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/) | H | sort by ratio + max-heap |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
