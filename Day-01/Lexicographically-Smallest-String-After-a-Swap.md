# Day 1 — Lexicographically Smallest String After a Swap

> LeetCode #3216 · Easy · Greedy, String

🔗 [LeetCode Problem](https://leetcode.com/problems/lexicographically-smallest-string-after-a-swap/)

---

## 🧩 Problem

Given a string representing an integer, swap any two adjacent digits that have the same parity to get the lexicographically smallest string possible.

---

## 💡 Key Insight



---

## 🚀 Approach



---

## 💻 My Solution

```java
class Solution {
    public String getSmallestString(String s) {
        char[] chars = s.toCharArray();
        for (int i = 0; i < s.length() - 1; i++) {
            
            char a = s.charAt(i);
            char b = s.charAt(i + 1);
          
            if (b<a && a%2==b%2) {
                swap(chars,i,i+1);
                return new String(chars);
            }

        }
        return s;
    }

    private void swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(?)`

* **Space:** `O(?)`


---

## 🧠 What I Learned

* Implemented optimal approach using Greedy.
* Improved understanding of time/space tradeoffs in this problem.
* Handled edge cases effectively.

---

## ⚠️ Things to Watch

* Avoid unnecessary iterations to keep time complexity optimal.
* Be careful with index bounds and edge cases.

---

## 📌 Progress

**Day:** 1 / 365
**Problem:** Lexicographically Smallest String After a Swap
**Difficulty:** Easy
**Status:** ✅ Solved
