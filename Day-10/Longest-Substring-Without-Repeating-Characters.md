# Day 10 — Longest Substring Without Repeating Characters

> LeetCode #3 · Medium · Hash Table, String, Sliding Window

🔗 [LeetCode Problem](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

---

## Problem

Given a string `s`, find the length of the longest substring without repeating characters.

---

## Key Insight

When I first read this problem, my immediate thought was to use the **Sliding Window** technique. Since I needed to ensure there were no repeating characters inside the window, the most natural data structure that came to mind was a `HashSet`. 

I spent about 15 minutes coding and debugging my own logic. The algorithm worked perfectly, but when I submitted it, the execution time was around **68ms**. While algorithmically correct, it felt a bit slow. That's when my mentor suggested a hardware-level optimization: since the problem only uses standard characters, I could completely drop the heavy `HashSet` and use a primitive **ASCII boolean array** instead.

Here is the breakdown of my journey from the initial logic to the optimized version.

---

---

## Approach

1: The Intuitive HashSet (68ms)

The core logic relies on two pointers: `left` and `right`.
- The `right` pointer aggressive expands the window by reading one character at a time.
- If the character is **NOT** in the `HashSet`, we add it and calculate the new max length.
- If the character **IS** already in the `HashSet` (a collision!), an alarm goes off. We use a `while` loop to make the `left` pointer "sweep" forward. It removes characters from the `HashSet` one by one until the exact duplicate character is kicked out of our window.

### Code (Version 1)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int left = 0;
        int max = 0;
        HashSet<Character> set = new HashSet<>();
        
        for (int right = 0; right < s.length(); right++) {
            // The Sweeper: kick elements out until the duplicate is gone
            while (set.contains(s.charAt(right))) {
                set.remove(s.charAt(left));
                left++;
            }
            // Register new character
            set.add(s.charAt(right));
            max = Math.max(max, right - left + 1);
        }
        return max;
    }
}
```

---

## My Solution

(Version 1)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int left = 0;
        int max = 0;
        HashSet<Character> set = new HashSet<>();
        
        for (int right = 0; right < s.length(); right++) {
            // The Sweeper: kick elements out until the duplicate is gone
            while (set.contains(s.charAt(right))) {
                set.remove(s.charAt(left));
                left++;
            }
            // Register new character
            set.add(s.charAt(right));
            max = Math.max(max, right - left + 1);
        }
        return max;
    }
}
```
# Approach 2: The ASCII Array Optimization (~11ms)

Why was Version 1 taking 68ms? Because in Java, `HashSet<Character>` forces primitive `char` types to be "boxed" into `Character` objects, creating overhead. Furthermore, hashing takes extra computational time.

Since standard ASCII characters only map to integer values from 0 to 127, my mentor suggested I use a simple `boolean[] set = new boolean[128]`.

- Every character maps directly to an index (e.g., `'a'` is 97).
- If `set[97] == true`, it means `'a'` is in the window.
- The execution logic remains exactly the same, but instead of calling `.contains()` and `.remove()`, we simply flip boolean values to `true` or `false` by accessing array indices directly in RAM.

This simple change bypassed all Object overhead and dropped my execution time to around **11ms**.
##

---

## Complexity

* **Time:** `O(N)`
(The fast pointer `right` traverses the array exactly once).
* **Space:** `O(1) or O(N)`
(Depends on HashSet implementation).

---

## What I Learned

* Successfully applied the Sliding Window pattern using a HashSet.
* Understood how to dynamically shrink a window from the left when a constraint (unique characters) is violated.

---

## Things to Watch

* When a duplicate is found at `right`, you cannot just remove the `right` character. You must remove characters from the `left` until the duplicate is gone.
* Always update the maximum length *after* ensuring the current window is valid.

---

## Progress

**Day:** 10 / 365
**Problem:** Longest Substring Without Repeating Characters
**Difficulty:** Medium
**Status:** Solved
