# Day 17 – Daily Temperatures

> LeetCode #739 – Medium – Array, Stack, Monotonic Stack

🔗 [LeetCode Problem](https://leetcode.com/problems/daily-temperatures/)

---

## Problem

Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

---

## Key Insight

This is the quintessential **Monotonic Stack** problem. A common mistake is to push the actual temperature values into the stack. However, since the problem asks for the "number of days to wait", we need to know the *indices* of the days.
The core idea is to think of the stack as a "waiting room" for indices that haven't found a warmer day yet. 
- We iterate through the array.
- While the current day's temperature is warmer than the temperature of the day at the top of the stack, we've found the answer for that past day!
- We pop the past day, calculate the distance (`currentDayIndex - pastDayIndex`), and store it in our result array.
- This creates a strictly decreasing (monotonic) order of temperatures in the stack.

---

## My Solution

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        int[] arr = new int[temperatures.length];
        
        for (int i = 0; i < temperatures.length; i++) {
            // While current day is warmer than the day at the top of the stack
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
                int dayIndex = stack.pop(); // This past day has found a warmer day
                arr[dayIndex] = i - dayIndex; // Calculate how many days it waited
            }
            // Add current day to the waiting room
            stack.push(i);
        }
        
        return arr;
    }
}
```

---

## Complexity

* **Time Complexity:** `O(N)`
  Even though there is a `while` loop inside a `for` loop, each index is pushed onto the stack exactly once and popped from the stack at most once. This results in at most $2N$ operations, simplifying to $O(N)$.
* **Space Complexity:** `O(N)`
  In the worst-case scenario (e.g., temperatures are strictly decreasing), no elements are popped until the very end (or never), so the stack will store all $N$ indices.

---

## What I Learned

* Discovered the **Monotonic Stack** pattern!
* Learned that when solving distance/index-related problems with a stack, pushing the **index** is much more useful than pushing the raw value (since we can always derive the value from the index).
* Understood that a `while` loop can be used inside a `for` loop without degrading performance to $O(N^2)$ as long as the inner loop's operations are bounded (e.g., each item is popped only once).

---

## Things to Watch

* Don't push `temperatures[i]`! Push `i`.
* Remember the loop condition: `temperatures[i] > temperatures[stack.peek()]`.
* Always check `!stack.isEmpty()` before calling `stack.peek()`.

---

## Progress

**Day:** 17 / 365
**Problem:** Daily Temperatures
**Difficulty:** Medium
**Status:** Solved
