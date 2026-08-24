# Day 3 — Group Anagrams

> LeetCode #49 · Medium · Array, Hash Table, String, Sorting

🔗 [LeetCode Problem](https://leetcode.com/problems/group-anagrams/)

---

## Problem

Given an array of strings, group the anagrams together.

---

## Key Insight

My first thought was to convert the characters to their integer values (ASCII) and sum them up to act as a unique identifier for each anagram group. However, I quickly realized this creates severe collisions (e.g., `"ad"` and `"bc"` both have the same sum, but are not anagrams). The only 100% safe way to identify anagrams is to find a "canonical form" for them. Sorting the characters of the string guarantees that all anagrams will perfectly match a single, unique string.

---

## Approach

1. We use a `HashMap` to group the strings. The **Key** will be the sorted version of the string (the canonical form), and the **Value** will be a `List<String>` acting as a "bag" to hold all the original strings that share that sorted form.
2. For each string `s` in the array:
   - Convert it to a character array and sort it (`Arrays.sort()`).
   - Convert the sorted character array back to a string to use as the `key`. *(Note: Using `char[].toString()` will return a memory address, so we must use `new String(c)`).*
   - If this `key` doesn't exist in our map yet, we initialize a new empty `ArrayList` for it.
   - We then retrieve the list for that `key` and add the original string `s` into it.
3. Finally, we return all the grouped lists using `new ArrayList<>(map.values())`.

---

## My Solution

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        for(int i = 0; i<strs.length;i++){
            String s = strs[i];
            char[] c = s.toCharArray();
            Arrays.sort(c);
            String key = new String(c);
            if(!map.containsKey(key)){
                map.put(key,new ArrayList<>());
            }
            map.get(key).add(s);
        }
        return new ArrayList<>(map.values());

    }
}
```

---

## Complexity

* **Time:** `O(N \cdot K \log K)`
(Where $N$ is the number of strings in the array, and $K$ is the maximum length of a string. We iterate through $N$ strings, and for each string, we sort it which takes $O(K \log K)$ time).
* **Space:** `O(N \cdot K)`
(In the worst-case scenario, we are storing all $N$ strings, each of length $K$, inside our HashMap and its Lists).

---

## What I Learned

* Implemented optimal approach using Array.
* Improved understanding of time/space tradeoffs in this problem.
* Handled edge cases effectively.

---

## Things to Watch

* Avoid unnecessary iterations to keep time complexity optimal.
* Be careful with index bounds and edge cases.

---

## Progress

**Day:** 3 / 365
**Problem:** Group Anagrams
**Difficulty:** Medium
**Status:** ✅ Solved
