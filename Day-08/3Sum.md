# Day 8 — 3Sum

> LeetCode #15 · Medium · Array, Two Pointers, Sorting

🔗 [LeetCode Problem](https://leetcode.com/problems/3sum/)

---

## Problem

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

---

## Key Insight

I'll be completely honest—this problem was a massive difficulty spike and way above my initial level. My first thought was trying to juggle 3 pointers simultaneously, which quickly became overwhelming, and a brute force $O(N^3)$ approach would obviously result in Time Limit Exceeded (TLE). 

After spending a lot of time thinking and relying heavily on AI mentorship, I finally hit my "Aha!" moment. You don't manage 3 pointers at once. Instead, you **sort the array**, "anchor" one number, and then reduce the rest of the problem into a standard **Two Sum II** (Two Pointers) problem.

---

## Approach

The core algorithm is what I like to call **"Anchor and Pinch"**, combined with a ruthless **"Skip the Clones"** rule.

1. **Sort the array:** This is mandatory for the Two Pointers technique to work and for duplicates to sit next to each other.
2. **The Anchor (`i`):** We loop through the array. The current number `nums[i]` is pinned. Our goal is to find two other numbers that sum up to `-nums[i]`.
3. **The Pinch (`left` & `right`):** We place `left` right after `i`, and `right` at the end of the array. We pinch them inward based on the sum, exactly like Two Sum II.
4. **The Hardest Part (De-duplication):** To avoid logging duplicate triplets, we must aggressively skip identical numbers (clones):
   - **Rule 1 (Anchor):** If `nums[i] == nums[i-1]`, we've already calculated all combinations for this number. Skip it.
   - **Rule 2 & 3 (Pointers):** When we find a valid triplet, we log it and move both pointers inward. But we must use `while` loops to keep advancing them past any adjacent identical numbers so we don't accidentally log the exact same triplet again.

---

## My Solution

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> list = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            // De-duplication Rule 1: Skip clone anchors
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            int left = i + 1;
            int right = nums.length - 1;
            
            while (left < right) {
                if (nums[left] + nums[right] < -nums[i]) {
                    left++;
                } else if (nums[left] + nums[right] > -nums[i]) {
                    right--;
                } else {
                    // Match found!
                    list.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    
                    // Move pointers to search for the next distinct pair
                    left++;
                    right--;
                    
                    // De-duplication Rules 2 & 3: Skip clone pointers
                    while (left < right && nums[left] == nums[left - 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right + 1]) {
                        right--;
                    }
                }
            }
        }
        return list;
    }
}

```

## Complexity

* **Time:** `O(N^2)`
(Sorting takes $O(N \log N)$. The outer `for` loop runs $N$ times, and the inner `while` loop processes the remaining elements in $O(N)$ time. Overall time is dominated by $O(N) \times O(N) = O(N^2)$).
* **Space:** `O(1)`
or 
(Depending on the language's sorting algorithm. Java's `Arrays.sort()` uses dual-pivot Quicksort for primitives, which takes $O(\log N)$ auxiliary space. We don't use any extra data structures other than the required output list).

---

## What I Learned

* Used sorting to simplify the process of avoiding duplicates and managing the two pointers.
* Transformed a 3Sum problem into a series of 2Sum II problems.
* Handled duplication carefully by skipping identical adjacent elements for both the outer loop and the inner pointers.

---

## Things to Watch

* The input array must be sorted first.
* Always check `if (i > 0 && nums[i] == nums[i-1])` to prevent duplicate triplets for the first number.
* After a match is found, remember to advance both `left` and `right` pointers, and skip duplicates for `left` to avoid identical results.

---

## Progress

**Day:** 8 / 365
**Problem:** 3Sum
**Difficulty:** Medium
**Status:** Solved
