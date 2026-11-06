https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero

### Approaching the problem:
Well the problem mentions a number x and an array.

We need to make x zero by subtracting the corner numbers from the array. For example,
```
Array = [1, 1, 4, 2, 3], x = 5

we can remove the last two numbers from the array to make x zero.
```

- First approach: to use two pointers and the. dp the left and right pointer. Space and Time both are O(n^2) -> did not work.
- Second Approach: [took hints], use prefix sum, then use 2 nested loops over this array to get the sum of each possible window, make sure it is sum - x. TLE. 
- Third Approach: as all the numbers in the array are positive, we can use a single loop over the array and keep on expanding the window as far as it is less than or equal the target, otherwise shrink.

### Connected to the fundamental problems
[[Given a target in positive numbers, get the window]]