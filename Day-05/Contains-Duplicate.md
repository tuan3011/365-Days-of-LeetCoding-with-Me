# Day 5 — Contains Duplicate

> LeetCode #217 · Easy · Array, Hash Table, Sorting

🔗 [LeetCode Problem](https://leetcode.com/problems/contains-duplicate/)

---

## 🧩 Problem

Given an integer array, return true if any value appears at least twice in the array, and return false if every element is distinct.

---

## 💡 Key Insight

My first thought was using a similar method to the "Good Pairs" problem.

---

## 🚀 Approach

I initially created an array of size 100 to count the occurrences of the numbers (I didn't look carefully at the constraints). I iterated through the array to count the appearances until a number reached a count of 2 and returned true. But after testing and submitting, it threw an `IndexOutOfBoundsException` because the constraints include negative numbers and values up to $10^9$. 

Then I realized I needed a data structure that can handle large, unpredictable numbers while maintaining $O(1)$ lookup time. I switched to a `HashSet` instead, and it ran successfully!

---

## 💻 My Solution

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (!set.add(num))
                return true;
        }
        return false;
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(N)`
$O(N)$
* **Space:** `O(N)`
$O(N)$

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

**Day:** 5 / 365
**Problem:** Contains Duplicate
**Difficulty:** Easy
**Status:** ✅ Solved
