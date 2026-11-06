# Greedy: Progressive Questions

**Asked at (2025–26 reports):** general senior loops

**Signal:** a local best choice that never needs undoing; often 'sort, then pick'.

**Core idea:** Always say why the greedy choice is safe (the exchange argument). Senior interviewers ask this.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 53, 55, 45, 134 Gas Station, 846, 1899, 763 Partition Labels, 678

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [670 Maximum Swap](https://leetcode.com/problems/maximum-swap/) | M | last index of each digit |  |  |
| [1029 Two City Scheduling](https://leetcode.com/problems/two-city-scheduling/) | M | sort by cost difference |  |  |
| [945 Minimum Increment to Make Array Unique](https://leetcode.com/problems/minimum-increment-to-make-array-unique/) | M | sort, then push values up |  |  |
| [1647 Minimum Deletions to Make Character Frequencies Unique](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/) | M | lower duplicate counts |  |  |
| [991 Broken Calculator](https://leetcode.com/problems/broken-calculator/) | M | work backwards |  |  |

## Stage 2: Twists (Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [135 Candy](https://leetcode.com/problems/candy/) | H | two passes |  |  |
| [871 Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/) | H | max-heap of fuel you passed |  |  |
| [1326 Minimum Number of Taps to Open to Water a Garden](https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/) | H | jump game on intervals |  |  |
| [330 Patching Array](https://leetcode.com/problems/patching-array/) | H | track the reachable range |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [630 Course Schedule III](https://leetcode.com/problems/course-schedule-iii/) | H | sort by deadline + max-heap to undo |  |  |
| [2589 Minimum Time to Complete All Tasks](https://leetcode.com/problems/minimum-time-to-complete-all-tasks/) | H | sort by end, fill from the right |  |  |
| [1505 Minimum Possible Integer After at Most K Adjacent Swaps On Digits](https://leetcode.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits/) | H | greedy + Fenwick tree |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
