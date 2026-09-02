# Day 11 — Longest Repeating Character Replacement

> LeetCode #424 · Medium · Hash Table, String, Sliding Window

🔗 [LeetCode Problem](https://leetcode.com/problems/longest-repeating-character-replacement/)

---

## Problem

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

---

## Key Insight

At first glance, this problem looks like it requires a complex approach to track character counts and replacements. However, it perfectly fits the **Sliding Window** pattern combined with a Mathematical condition. 

Instead of using a heavy `HashMap` to track character frequencies, I used a classic hardware-level optimization: mapping uppercase ASCII characters to a 26-element primitive array using the `'A'` offset trick (`char - 'A'`). This reduces space complexity to an absolute minimum and makes lookups lightning fast.

---

## Approach

1. **The Window and The Math:** We use two pointers (`left` and `right`) to create a sliding window. The core logic relies on this formula:
   `Window_Length - Max_Frequency_Character <= k`
   If the total characters in the window minus the most frequent character is less than or equal to `k`, it means we have enough replacement quota to make all characters in the window identical.
2. **Expanding:** As `right` moves, we update the frequency of the current character in our 26-element array and dynamically track the highest frequency seen so far (`maxFreq`).
3. **Shrinking (The Validation):** If the window breaks our mathematical rule (meaning we don't have enough `k` to fix the string), we must shrink the window. We decrement the frequency of the character at `left` and move `left` forward until the window is valid again.
4. **Recording:** Once the window is valid, we calculate its length and update our `maxLength`.

---

## My Solution

```java
class Solution {
    public int characterReplacement(String s, int k) {
        // ASCII array trick for O(1) space and lightning fast lookups
        int[] count = new int[26];
        int left = 0;
        int maxLength = 0;
        int maxFreq = 0;
        
        for (int right = 0; right < s.length(); right++) {
            // Map character to index 0-25 and increment frequency
            count[s.charAt(right) - 'A']++;
            
            // Track the most frequent character in the current window
            maxFreq = Math.max(maxFreq, count[s.charAt(right) - 'A']);
            
            // If remaining characters exceed our replacement quota 'k', shrink the window
            while ((right - left + 1) - maxFreq > k) {
                count[s.charAt(left) - 'A']--;
                left++;
            } 
            
            // Window is valid, update the maximum length record
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```
---

## Complexity

* **Time:** `O(N)`
(Both `left` and `right` pointers iterate through the string exactly once. The operations inside the loop are $O(1)$ constant time array lookups).
* **Space:** `O(1)`
(We use a primitive `int[] count = new int[26]` array. Regardless of how massive the input string is, the memory footprint remains strictly bound to 26 integers).

---

## What I Learned

* Successfully combined the Sliding Window pattern with a frequency map array (`int[26]`).
* Realized that the validity of a window is mathematically defined by `window_size - max_frequency <= k`.
* Learned that `max_frequency` doesn't strictly need to be decremented when shrinking the window, because the window length only grows when a higher `max_frequency` is found (though recalculating it via a helper method is safer and easier to understand for beginners).

---

## Things to Watch

* Calculating the `window_size` correctly using `right - left + 1`.
* Making sure to update the frequencies correctly when shrinking the window (`freq[s.charAt(left) - 'A']--`).
* When using a fast array for standard uppercase alphabet characters, index calculations use `- 'A'`.

---

## Progress

**Day:** 11 / 365
**Problem:** Longest Repeating Character Replacement
**Difficulty:** Medium
**Status:** Solved
