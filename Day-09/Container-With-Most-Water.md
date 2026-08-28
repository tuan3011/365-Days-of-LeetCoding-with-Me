# Day 9 — Container With Most Water

> LeetCode #11 · Medium · Array, Two Pointers, Greedy

🔗 [LeetCode Problem](https://leetcode.com/problems/container-with-most-water/)

---

## Problem

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`. Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store.

---

## Key Insight

I spent about an hour stuck in the mud on this one. My first instinct was to use Two Pointers, but I completely overcomplicated the math and the pointer movements. I was only tracking the heights and trying to "look ahead" by placing post-increment operators directly inside my `if` conditions (e.g., `if(height[left++] > ...)`), which caused my pointers to jump out of control. Worse, I initially forgot that I was supposed to calculate the **Area (Volume)**, not just find the tallest walls. 

After stepping back and resetting my logic, I realized this is a pure geometry and Greedy algorithm problem.

---

## Approach

1. **The Math:** The amount of water is defined by `Area = Width * Height`. 
   - `Width` is simply the distance between the two pointers: `right - left`.
   - `Height` is bottlenecked by the shorter wall: `Math.min(height[left], height[right])`.
2. **The Greedy Logic (How to move pointers):** Since we start with the maximum possible width (pointers at both ends), moving *any* pointer inward guarantees the width will decrease. To compensate for a shrinking width and potentially find a larger area, we *must* find a taller wall. Therefore, we always discard the shorter wall. 
   - If `height[left] < height[right]`, we move `left++`.
   - Otherwise, we move `right--`.
3. **The "Equal Height" Trap:** Initially, if the walls were equal, my code returned the area immediately. That was a fatal flaw (e.g., `[8, 100, 100, 8]`). If they are equal, we just drop one of them (either one works, so `else { right--; }` perfectly covers both `>` and `==` cases) and keep exploring the inner elements for a potentially massive spike in height.

---

## My Solution

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;
        
        while (left < right) {
            int currentWidth = right - left;
            int currentArea = currentWidth * Math.min(height[left], height[right]);
            maxArea = Math.max(maxArea, currentArea);
            
            // Greedy: Always drop the shorter wall
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return maxArea;
    }
}

```

## Complexity

* **Time:** `O(N)`
(The two pointers start at opposite ends and move inward until they meet. We visit each wall at most once).
* **Space:** `O(1)`
(We only use a few variables for tracking `left`, `right`, `currentWidth`, `currentArea`, and `maxArea`, requiring no extra memory scaling).

---

## What I Learned

* Solidified my understanding of the Two Pointers pattern, specifically when maximizing an area or sequence.
* Recognized that greedily abandoning the shorter boundary is mathematically the only way to find a potentially larger area.
* Learned to trust mathematical logic over brute-force exhaustive searches.

---

## Things to Watch

* The height of the water is constrained by the shorter line (`Math.min(height[left], height[right])`).
* The width of the container is the distance between the two pointers (`right - left`).
* Always move the pointer that points to the shorter line, as moving the taller line can only decrease or maintain the area, never increase it.

---

## Progress

**Day:** 9 / 365
**Problem:** Container With Most Water
**Difficulty:** Medium
**Status:** Solved
