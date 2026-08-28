# Day 9 — Remove Duplicates from Sorted Array

> LeetCode #26 · Easy · Array, Two Pointers

🔗 [LeetCode Problem](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

---

## Problem

Given an integer array `nums` sorted in non-decreasing order, remove the duplicates **in-place** such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in `nums`.

---

## Key Insight

Since the problem strictly requires removing duplicates **in-place** with $O(1)$ extra memory, creating a new array or using a `HashSet` is out of the question. Because the array is already sorted, duplicates will always sit next to each other. This is a perfect scenario for the **Fast and Slow Pointers** technique.

---

## Approach

1. I use two pointers moving in the same direction: a `fast` pointer (named `second` in my code) to scout the array, and a `slow` pointer (named `first`) to build the modified array.
2. The first element (`nums[0]`) is always unique, so both pointers can start at index 1.
3. The `fast` pointer iterates through the array. At each step, it compares its current value with the previous one (`nums[second] != nums[second - 1]`).
   - If they are the same, it's a duplicate. The `fast` pointer just ignores it and moves on.
   - If they are different, we found a new unique number! We copy this new number to the position tracked by the `slow` pointer (`nums[first] = nums[second]`), and then increment the `slow` pointer to prepare for the next unique number.
4. After the loop finishes, the `slow` pointer's value represents exactly the length of our new array containing only unique elements.

---

## My Solution

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int first = 1;
        for (int second = 1; second < nums.length; second++) {
            if (nums[second] != nums[second - 1]) {
                nums[first] = nums[second];
                first++;
            }
        }
        return first;
    }
}

---

## Complexity

* **Time:** `O(N)`
(The `fast` pointer traverses the array of size $N$ exactly once).
* **Space:** `O(1)`
(We only use two integer pointers, modifying the array entirely in-place).

---

## What I Learned

* Mastered the Fast/Slow Pointer technique for in-place array modification.
* Realized that when manipulating an array in-place, the Slow pointer acts as the "writer" while the Fast pointer acts as the "reader/explorer".

---

## Things to Watch

* The array is already sorted, which guarantees all duplicates sit next to each other.
* The problem asks to return the *number of unique elements* (`left + 1`), not the 0-based index.

```

## Progress

**Day:** 9 / 365
**Problem:** Remove Duplicates from Sorted Array
**Difficulty:** Easy
**Status:** Solved
