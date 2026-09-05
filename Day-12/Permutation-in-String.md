# Day 12 — Permutation in String

> LeetCode #567 · Medium · Hash Table, Two Pointers, String, Sliding Window

🔗 [LeetCode Problem](https://leetcode.com/problems/permutation-in-string/)

---

## Problem

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

---

## Key Insight

& The "Aha!" Moment
This problem introduced me to a new sub-pattern: the **Fixed-Size Sliding Window**. At first, reading the theory felt a bit overwhelming. But after writing the code, debugging the boundaries, and reading through my own logic, everything suddenly clicked. 

The goal is to find a permutation of string `s1` inside string `s2`. Since permutations must have the exact same length and character frequencies, we don't need a dynamic window that expands and shrinks. Instead, we create a rigid "mold" (window) of exactly `s1.length()` and slide it across `s2`. To check for matching frequencies without heavy $O(N)$ operations, the ultimate weapon is comparing two 26-element ASCII arrays using `Arrays.equals()`.

---

## Approach

1. **The Guard (Edge Case):** If `s1` is strictly longer than `s2`, it's physically impossible for `s2` to contain a permutation of `s1`. Return `false` immediately.
2. **The Molds (Arrays):** Create two frequency arrays: `countS1` and `countWindow`.
3. **Loop 1 - Initialization:** Iterate up to `s1.length()`. This loop populates `countS1` with the target frequencies and simultaneously fills `countWindow` with the very first window of `s2`.
4. **First Check:** Before sliding, we check if this initial window is already a match.
5. **Loop 2 - Sliding the Window:** We start iterating from `s1.length()` to the end of `s2`. 
   - A new character enters the window from the right, so we increment its count.
   - An old character falls out of the window from the left, so we decrement its count.
   - We check `Arrays.equals(countS1, countWindow)`. If it's a match, we found our permutation!
6. **Move Forward:** Increment the `left` pointer to keep the window size rigidly fixed for the next iteration.

---

## My Solution

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) {
            return false;
        }
        
        int[] countS1 = new int[26];
        int[] countWindow = new int[26];
        int left = 0;
        
        // Build the frequency map for s1 and the first window of s2
        for (int i = 0; i < s1.length(); i++) {
            countS1[s1.charAt(i) - 'a']++;
            countWindow[s2.charAt(i) - 'a']++;
        }
        
        // Check the first window
        if (Arrays.equals(countS1, countWindow)) {
            return true;
        }
        
        // Slide the fixed-size window across the rest of s2
        for (int right = s1.length(); right < s2.length(); right++) {
            countWindow[s2.charAt(right) - 'a']++;
            countWindow[s2.charAt(left) - 'a']--;
            
            if (Arrays.equals(countS1, countWindow)) {
                return true;
            }
            left++;
        }
        
        return false;
    }
}

---
```

## Complexity

* **Time:** `O(N)`
** 
(Where $N$ is the length of `s2`. We iterate through the strings linearly. The `Arrays.equals()` check operates on fixed 26-element arrays, which takes $O(1)$ constant time).

- **Space complexity:** 
(We only use two primitive integer arrays of size 26. The memory footprint does not scale with the input string length).
* **Space:** `O(1)`
** 
(We only use two primitive integer arrays of size 26. The memory footprint does not scale with the input string length).

---

## What I Learned

* Cemented the use of **Fixed-Size Sliding Windows** for permutation problems.
* Realized that permutations of strings boil down to exact matches of their frequency arrays.
* Used helper methods like `Arrays.equals()` to smoothly compare frequency counts in `O(26)` which is essentially `O(1)`.

---

## Things to Watch

* Handle the edge case where `s1.length() > s2.length()` right at the start to avoid exceptions.
* In a fixed window, when `right` pointer expands the window beyond the required length, you must manually shrink it from the `left` side by decrementing the frequency map of `s2.charAt(left)` and incrementing `left`.

---

## Progress

**Day:** 12 / 365
**Problem:** Permutation in String
**Difficulty:** Medium
**Status:** Solved
