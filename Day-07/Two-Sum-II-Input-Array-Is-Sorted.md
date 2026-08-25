# Day 7 — Two Sum II - Input Array Is Sorted

> LeetCode #167 · Medium · Array, Two Pointers, Binary Search

🔗 [LeetCode Problem](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

---

## Problem

Given a **1-indexed** array of integers `numbers` that is already **sorted in non-decreasing order**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

---

## Key Insight

Since I previously solved the standard "Two Sum" using a `HashMap`, my first instinct was to do that again. However, this problem explicitly states the array is **already sorted** and restricts us to **$O(1)$ Space**. This constraint completely kills the HashMap idea. Because the array is sorted, this is a textbook scenario for the **Two Pointers** pattern.

---

## Approach

1. Place one pointer at the start (`left = 0`) and one at the end (`right = numbers.length - 1`).
2. Calculate the sum of the numbers at these two pointers.
3. Because the array is sorted in ascending order:
   - If the sum is **greater** than the target, it means our sum is too large. We need to decrease it by moving the `right` pointer to the left (`right--`).
   - If the sum is **less** than the target, it means our sum is too small. We need to increase it by moving the `left` pointer to the right (`left++`).
4. If it perfectly matches the target, return the indices! *(Note: The problem asks for a 1-indexed array, so we must return `left + 1` and `right + 1`).*

---

## My Solution

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        while(left < right){
            while(numbers[left] + numbers[right] < target){
                left++;
            }
            while(numbers[left] + numbers[right] > target){
                right--;
            }
            if(numbers[left] + numbers[right] == target){
                return new int[]{left + 1, right + 1};
            }
        }
        return null;
    }
}
```
# Pro-Tip: The "Safe Code" Approach (Version 2)
Although Version 1 gets **2ms / 99%** on LeetCode, it has a hidden trap. It only works because LeetCode guarantees *"exactly one solution"*. If we were in a real-world scenario where a solution might **NOT** exist, the inner `while` loops would push the pointers out of bounds, crashing the app with an `ArrayIndexOutOfBoundsException`. 

To write robust, production-level code, we should replace the nested `while` loops with a clean `if - else if - else` structure. This prevents infinite loops, avoids out-of-bounds errors, and calculates the `sum` only once per iteration.

# Robust Version (Version 2): #
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right]; 
            
            if (sum < target) {
                left++;
            } else if (sum > target) {
                right--;
            } else {
                return new int[]{left + 1, right + 1};
            }
        }
        return null;
    }
}

---

## Complexity

* **Time:** `O(N)`
(The `left` and `right` pointers traverse the array exactly once, meeting in the middle).
* **Space:** `O(1)`
(We only use two integer pointers, strictly adhering to the memory constraint).

---

## What I Learned

* Avoided using a HashMap to strictly adhere to the $O(1)$ space requirement.
* Applied the Two Pointers pattern, perfectly suited for sorted arrays.
* Realized the hidden risks of nested `while` loops that skip boundary checks in edge cases.
* Wrote robust, production-level code by using clear `if - else` conditions to avoid potential `ArrayIndexOutOfBoundsException`.

---

## Things to Watch

* Remember that the problem uses a 1-indexed array, so adjust return values accordingly (`left + 1`, `right + 1`).
* Beware of infinite loops or out-of-bounds errors when using inner `while` loops.

---

## Progress

**Day:** 7 / 365
**Problem:** Two Sum II - Input Array Is Sorted
**Difficulty:** Medium
**Status:** Solved
