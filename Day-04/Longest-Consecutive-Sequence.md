# Day 4 — Longest Consecutive Sequence

> LeetCode #128 · Medium · Array, Hash Table, Union Find

🔗 [LeetCode Problem](https://leetcode.com/problems/longest-consecutive-sequence/)

---

## 🧩 Problem

Given an unsorted array of integers, return the length of the longest consecutive elements sequence.

---

## 💡 Key Insight

My very first thought was the most natural one: just use `Arrays.sort()`, loop through the array, and count consecutive numbers. But then I looked at the constraints—the problem strictly demands an **$O(N)$** time complexity. Since sorting takes $O(N \log N)$, it's a trap. 

I knew I needed a data structure that could answer "Does the next number exist?" in $O(1)$ time. That led me to use a `HashSet`. However, I hit a massive mental block: if I iterate through the set and use a `while` loop to check for the next consecutive numbers for *every single element*, the worst-case time complexity degrades to $O(N^2)$ (e.g., array `[1, 2, 3, 4]`). I was super frustrated until my mentor pointed out a crucial detail: **Identifying the Sequence Starter.**

---

## 🚀 Approach

1. **The $O(1)$ Bucket:** Throw all numbers into a `HashSet`.
2. **The $O(N)$ Filter (The "Aha!" Moment):** We shouldn't start counting from just any number. A number is only the **start** of a sequence if the number strictly before it (`num - 1`) DOES NOT exist in the set. For example, in `[1, 2, 3]`, we only start the `while` loop when we are at `1` (because `0` is not in the set). If we are at `2` or `3`, we completely ignore them because they are just "followers".
3. **Climbing the Tower:** 
   - I used an `if (!set.contains(num - 1))` check. If it passes, I start climbing the sequence. 
   - I keep an internal `currentLength` counter starting at 1. 
   - As long as `set.contains(num + 1)` is true, I increment both the number and the `currentLength`. *(Fun Java trick: I can safely do `num++` directly on the loop variable because primitives in a for-each loop are passed by value).*
4. **Record the Max:** After breaking out of the `while` loop, I compare `currentLength` with my global `maxLength` and update it. Because of the `if` filter, every number in the array is processed exactly once, guaranteeing a strict $O(N)$ time complexity!

---

## 💻 My Solution

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> set = new HashSet<>();

        for (int num : nums) {
            set.add(num);
        }
        int maxLength = 0;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int currentLength = 1;
                while (set.contains(num + 1)) {
                    num++;
                    currentLength++;
                }
                maxLength = Math.max(maxLength, currentLength);
            }
        }
        return maxLength;
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(N)`
(Adding to the set takes $O(N)$. Iterating through the set takes $O(N)$. The inner `while` loop looks like it could be $O(N^2)$, but because of the `if` statement, the `while` loop only runs for valid sequences. Across the entire program, each number is visited inside the `while` loop exactly once. Thus, $O(N + N) = O(N)$).
* **Space:** `O(N)`
(We store all elements inside the `HashSet`. In the worst-case scenario with no duplicates, the set's size scales linearly with the input array).

---

## 🧠 What I Learned

* Implemented optimal approach using Array.
* Improved understanding of time/space tradeoffs in this problem.
* Handled edge cases effectively.

---

## ⚠️ Things to Watch

* Avoid unnecessary iterations to keep time complexity optimal.
* Be careful with index bounds and edge cases.

---

## 📌 Progress

**Day:** 4 / 365
**Problem:** Longest Consecutive Sequence
**Difficulty:** Medium
**Status:** ✅ Solved
