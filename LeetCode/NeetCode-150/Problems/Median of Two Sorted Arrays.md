---
topic: Trees
difficulty: Hard
leetcode: https://leetcode.com/problems/median-of-two-sorted-arrays/
---
# Median of Two Sorted Arrays

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/)

> Binary search the cut in the smaller array so both left halves are ≤ both right halves.

## Notes

https://www.youtube.com/watch?v=q6IEA26hvXc

Forget arrays for a moment. The median of a combined set of `N` elements is the value that **partitions the combined set into two equal halves** — left half all ≤ right half.

So the question becomes: **can you find that partition without actually merging the arrays?**

---

## Step 1: What does a valid partition look like?

Say you have:
`
A = [1, 3, 8, 9]
B = [2, 4, 6, 7]
combined sorted = [1, 2, 3, 4 | 6, 7, 8, 9]
`
The median lives at that `|` cut. Total 8 elements → 4 on each side.

Now, instead of merging, **cut both arrays independently**:
`
A: [1, 3 | 8, 9] → 2 elements from A on the left
B: [2, 4 | 6, 7] → 2 elements from B on the left
`
The partition is valid when:
- `A[left_end] ≤ B[right_start]` → `3 ≤ 6` ✓
- `B[left_end] ≤ A[right_start]` → `4 ≤ 8` ✓

If both conditions hold → you found the right cut.

---

## Step 2: Binary search on the cut position

If total length is `m + n`, the left half needs exactly `(m + n + 1) / 2` elements.

You only need to **binary search the cut in array A** (the shorter one). The cut in B is then fully determined:

`
cut_A + cut_B = half_len
cut_B = half_len - cut_A
`

So you're binary searching a single integer — the number of elements you take from A for the left half.

---

## Step 3: What does "wrong cut" look like?

`
A: [1, 3, 8 | 9] cut_A = 3
B: [2 | 4, 6, 7] cut_B = 1
`
Check:
- `A[2]=8` vs `B[1]=4` → `8 > 4` ✗ ← A's left side is too big

This means you took **too many from A**. Move cut_A **left**.

`
A: [1 | 3, 8, 9] cut_A = 1
B: [2, 4, 6 | 7] cut_B = 3
`
Check:
- `A[0]=1` vs `B[3]=7` → `1 ≤ 7` ✓
- `B[2]=6` vs `A[1]=3` → `6 > 3` ✗ ← B's left side is too big

Took too few from A → move cut_A **right**.

The binary search condition is clean:
- `A[cut_A - 1] > B[cut_B]` → cut_A too big → go left
- else → go right (or found it)

---

## Step 4: Computing the median from a valid partition

Once you have the right cut:

`
max_left = max(A[cut_A - 1], B[cut_B - 1]) ← largest on left side
min_right = min(A[cut_A], B[cut_B]) ← smallest on right side
`

- **Odd** total length → median = `max_left`
- **Even** total length → median = `(max_left + min_right) / 2.0`

Handle edge cases with `±infinity` when cut is at 0 or end of array.

---

## The full intuition in one sentence:

> Binary search for the cut position in the smaller array such that everything on the left of both cuts is ≤ everything on the right — that cut _is_ the median boundary.

---

## Complexity
- **Time:** O(log(min(m, n))) — binary search on the smaller array only
- **Space:** O(1)

---

Want me to now walk through the actual C++ implementation, or work through a tricky edge case (like one array being entirely smaller than the other)?
