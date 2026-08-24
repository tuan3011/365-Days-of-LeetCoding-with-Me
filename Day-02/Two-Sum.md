# Day 2 — Two Sum

> LeetCode #1 · Easy · Array, Hash Table

🔗 [LeetCode Problem](https://leetcode.com/problems/two-sum/)

---

## Problem

Given an array of integers and an integer target, return indices of the two numbers such that they add up to target.

---

## Key Insight

My initial thought was to use a frequency array to keep track of the numbers I've seen, similar to other counting problems. However, looking at the constraints, the numbers can be negative and as large as $10^9$. Creating an array of that size is impossible and would result in an `IndexOutOfBoundsException` or `OutOfMemoryError`. That's when I realized I needed a data structure that provides $O(1)$ lookup time without being constrained by the values themselves: A `HashMap`.

---

## Approach

1. **The Core Logic:** For any given `currentNum` at index `i`, we are looking for a specific `neededNum` such that `currentNum + neededNum = target` (which means `neededNum = target - currentNum`).
2. **The Map:** We use a `HashMap` where the **Key** is the number we have visited, and the **Value** is its index. 
3. **One-Pass Search:** As we iterate through the array, we check if our `neededNum` already exists in the map. 
   - If it does, we have found our pair! We simply retrieve the `oldIndex` from the map and return `[oldIndex, i]`.
   - If it doesn't, we add `currentNum` and its index `i` into the map to act as a potential pair for future numbers.

---

## My Solution

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            int currentNum = nums[i];
            int neededNum = target - currentNum;
            if(map.containsKey(neededNum)){
                int oldIndex = map.get(neededNum);
                 return new int[] {oldIndex, i};
            }
            map.put(currentNum,i);

        }
        return new int[] {};
    }
}
```

---

## Complexity

* **Time:** `O(N)`
(We traverse the array containing $N$ elements exactly once. Each lookup and insertion in the HashMap takes $O(1)$ time).
* **Space:** `O(N)`
(In the worst-case scenario where no pair is found until the very end, we will store all $N$ elements in the HashMap).

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

**Day:** 2 / 365
**Problem:** Two Sum
**Difficulty:** Easy
**Status:** ✅ Solved
