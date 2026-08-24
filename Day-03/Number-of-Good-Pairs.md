# Day 3 — Number of Good Pairs

> LeetCode #1512 · Easy · Array, Hash Table, Math, Counting

🔗 [LeetCode Problem](https://leetcode.com/problems/number-of-good-pairs/)

---

## Problem

Given an array of integers, return the number of good pairs (where nums[i] == nums[j] and i < j).

---

## Key Insight

The brute-force approach requires nested loops to check every pair, which takes $O(N^2)$ time. However, this is just a frequency counting problem. We can optimize it to $O(N)$ by keeping track of the frequencies of the numbers we've seen so far and counting pairs dynamically.

---

## Approach

1. **Frequency Array:** Looking at the constraints, the maximum value in `nums` is 100. Therefore, we can create an integer array `count` of size `101` (since arrays are 0-indexed) to store the frequency of each number.
2. **Dynamic Pair Counting:** We iterate through the array exactly once. When we are at a specific `num`, the number of new valid pairs it can form is exactly the number of times we have seen `num` in the past.
3. We simply add `count[num]` to our `res`, and then increment `count[num]` by 1 to include the current number for future pairings.

---

## My Solution

```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int[] count = new int[101];
        int res = 0;

        for (int num : nums) {
            res += count[num];
            count[num] += 1;
        }
        return res;
    }
}
```

---

## Complexity

* **Time:** `O(N)`
(We only iterate through the array `nums` exactly once).
* **Space:** `O(1)`
(We use a fixed-size array of 101 elements, which takes constant space regardless of how large the input array `nums` is).

---

## What I Learned

* Implemented optimal approach using Array.
* Improved understanding of time/space tradeoffs in this problem.
* Handled edge cases effectively.

---

## Things to Watch

* Avoid unnecessary iterations to keep time complexity optimal.
* Be careful with index bounds and edge cases.

---

## Progress

**Day:** 3 / 365
**Problem:** Number of Good Pairs
**Difficulty:** Easy
**Status:** ✅ Solved
