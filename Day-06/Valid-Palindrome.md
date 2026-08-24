# Day 6 — Valid Palindrome

> LeetCode #125 · Easy · Two Pointers, String

🔗 [LeetCode Problem](https://leetcode.com/problems/valid-palindrome/)

---

## Problem

Check if a given string is a valid palindrome, considering only alphanumeric characters and ignoring cases.

---

## Key Insight

My first thought was using a loop to extract valid characters into a new array or string, and then using the two pointers method to check the whole string. However, I realized this would take $O(N)$ extra space. To optimize it, I decided to apply the two pointers directly on the original string in-place to save memory.

---

## Approach

1. Initialize two pointers: `left` at the beginning (index 0) and `right` at the end (index `s.length() - 1`).
2. Run a `while (left < right)` loop to move the pointers towards the center.
3. **Skip non-alphanumeric characters:** Use nested `while` loops to advance the `left` pointer and decrement the `right` pointer until they both point to a valid letter or digit.
4. **Compare:** Convert both characters to lowercase and compare them. If they mismatch, return `false` immediately.
5. If they match, move both pointers inward and continue checking. If the loop completes without finding any mismatches, return `true`.

---

## My Solution

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while (left < right) {
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {left++;}
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {right--;}
            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {return false;}
            left++;
            right--;
        }
        return true;
    }
}
```

---

## Complexity

* **Time:** `O(N)`
(The `left` and `right` pointers traverse the string only once. Combined, they travel a total distance of $N$ characters).
* **Space:** `O(1)`
(We modify our search space in-place using just two integer variables. No additional data structures or string copies are created).

---

## What I Learned

* Implemented optimal approach using Two Pointers.
* Improved understanding of time/space tradeoffs in this problem.
* Handled edge cases effectively.

---

## Things to Watch

* Avoid unnecessary iterations to keep time complexity optimal.
* Be careful with index bounds and edge cases.

---

## Progress

**Day:** 6 / 365
**Problem:** Valid Palindrome
**Difficulty:** Easy
**Status:** ✅ Solved
