# Day 13 – Minimum Size Subarray Sum

> LeetCode #209 – Medium – Array, Sliding Window

🔗 [LeetCode Problem](https://leetcode.com/problems/minimum-size-subarray-sum/)

---

## Problem

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a contiguous subarray whose sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

---

## Key Insight

This problem perfectly captures the **Dynamic Sliding Window** pattern. Unlike a fixed-size window, here the window expands to satisfy the condition (`currentSum >= target`) and then continuously shrinks from the left to find the absolute minimum length while keeping the condition valid. The key realization is that once we hit a valid sum, we don't restart the search; we just attempt to drop elements from the back to see if we can get a smaller valid window.

---

## Approach

1. Initialize `left = 0`, `currentSum = 0`, and `minLength = Integer.MAX_VALUE`.
2. Expand the window by iterating `right` from `0` to the end of the array, adding `nums[right]` to `currentSum`.
3. Whenever `currentSum >= target`, we have a valid window. Inside a `while` loop:
   - Update `minLength` using `Math.min()`.
   - Shrink the window by subtracting `nums[left]` from `currentSum`.
   - Move the `left` pointer forward (`left++`).
4. Keep shrinking until the condition breaks, then let `right` continue expanding.
5. Finally, if `minLength` wasn't updated (meaning no valid subarray was found), return `0`. Otherwise, return `minLength`.

---

## My Solution

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int currentSum = 0;
        int minLength = Integer.MAX_VALUE;
        
        for (int right = 0; right < nums.length; right++) {
            currentSum += nums[right];
            
            while (currentSum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                currentSum -= nums[left];
                left++;
            }
        }
        
        if (minLength == Integer.MAX_VALUE) {
            return 0;
        } else {
            return minLength;
        }
    }
}
```

---

## Complexity

* **Time Complexity:** `O(N)`
  The `right` pointer iterates through the array once. The `left` pointer also traverses the array at most once. Each element is processed (added and subtracted) exactly once, yielding $O(2N)$, which simplifies to $O(N)$ time.
* **Space Complexity:** `O(1)`
  We only use a few integer variables (`left`, `currentSum`, `minLength`) to keep track of the window's state. The memory used is constant regardless of the array's size.

---

## What I Learned

* Cemented the **Dynamic Sliding Window** mechanic, specifically the inner `while` loop used to shrink the window from the left.
* Realized that I must explicitly add the new element (`nums[right]`) before checking the condition.
* Learned to always use `Math.min()` to avoid accidentally overwriting a previously found better (smaller) result.

---

## Things to Watch

* Don't forget to add `nums[right]` to the accumulator before the `while` loop!
* Don't accidentally overwrite the `minLength` inside the `while` loop with just `right - left + 1`; it must be compared against the existing `minLength`.
* Handle the edge case where no valid subarray is found by checking if `minLength` is still the initial `MAX_VALUE` at the end.

---

## Progress

**Day:** 13 / 365
**Problem:** Minimum Size Subarray Sum
**Difficulty:** Medium
**Status:** Solved
