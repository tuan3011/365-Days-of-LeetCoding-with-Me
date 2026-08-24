# Day 2 — Number of Equivalent Domino Pairs

> LeetCode #1128 · Easy · Array, Hash Table, Counting

🔗 [LeetCode Problem](https://leetcode.com/problems/number-of-equivalent-domino-pairs/)

---

## 🧩 Problem

Given a list of dominoes, return the number of pairs (i, j) for which dominoes[i] is equivalent to dominoes[j].

---

## 💡 Key Insight

To avoid the $O(N^2)$ brute-force approach of comparing every domino with each other, we need a way to group equivalent dominoes together. The best way to do this is to normalize them into a single "canonical form" so that both `[1, 2]` and `[2, 1]` look exactly the same to our program.

---

## 🚀 Approach

1. **Normalization:** For each domino `[a, b]`, we force the smaller number to be the tens digit and the larger number to be the units digit using `Math.min(a,b) * 10 + Math.max(a,b)`. This guarantees that `[1,2]` and `[2,1]` both evaluate to the integer `12`.
2. **Frequency Array:** Since the values on the dominoes are between 1 and 9, the maximum normalized value is `99`. We can use a simple integer array `count` of size 100 to act as our frequency map.
3. **Dynamic Pair Counting:** As we iterate through the dominoes, the number of new valid pairs the current domino can form is exactly equal to the number of times we have seen it in the past. We add `count[val]` to our result, and then increment `count[val]` by 1.

---

## 💻 My Solution

```java
class Solution {
    public int numEquivDominoPairs(int[][] dominoes) {
        int res=0;
        int[] count = new int[100];
        for(int[] domino : dominoes){
            int a = domino[0];
            int b = domino[1];
            int val = Math.min(a,b)*10+Math.max(a,b);
            res+=count[val];
            count[val]+=1;
        }
        return res;
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(N)`
(Where $N$ is the number of dominoes. We only iterate through the 2D array exactly once).
* **Space:** `O(1)`
(We only use an integer array of a fixed size of 100, which takes constant space regardless of the input size).

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

**Day:** 2 / 365
**Problem:** Number of Equivalent Domino Pairs
**Difficulty:** Easy
**Status:** ✅ Solved
