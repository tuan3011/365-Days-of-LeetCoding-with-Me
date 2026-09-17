# Day 14 – Valid Parentheses

> LeetCode #20 – Easy – String, Stack

🔗 [LeetCode Problem](https://leetcode.com/problems/valid-parentheses/)

---

## Problem

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

---

## Key Insight

This problem is the classic introduction to the **Stack (LIFO - Last In, First Out)** data structure. The core insight is that the most recently opened bracket must be the first one to be closed.

Instead of a traditional approach where we push opening brackets to the stack and compare them later, we use an elegant trick: **push the expected closing bracket** into the stack whenever we encounter an opening bracket. This way, when we actually encounter a closing bracket in the string, we just pop the stack and check if it matches perfectly.

---

## Approach

1. Initialize an empty `Stack<Character>`.
2. Iterate through each character `c` in the string `s`.
3. If `c` is an opening bracket (`(`, `{`, `[`), push its corresponding closing bracket (`)`, `}`, `]`) onto the stack.
4. If `c` is a closing bracket, check two things:
   - Is the stack empty? (This means we have a closing bracket without an opening bracket). If so, return `false`.
   - Does `stack.pop()` not equal `c`? (This means it's a mismatched bracket). If so, return `false`.
5. After the loop, return `stack.isEmpty()` to ensure there are no unclosed opening brackets left.

---

## My Solution

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        
        // Fast-fail for odd length strings
        if (s.length() % 2 != 0) return false;
        
        for (char c : s.toCharArray()) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '{') {
                stack.push('}');
            } else if (c == '[') {
                stack.push(']');
            } else {
                if (stack.isEmpty() || stack.pop() != c) {
                    return false;
                }
            }
        }
        
        return stack.isEmpty();
    }
}
```

---

## Complexity

* **Time Complexity:** `O(N)`
  We iterate through the string of length $N$ exactly once. Stack operations `push()` and `pop()` take $O(1)$ constant time. Thus, the total time is proportional to $N$.
* **Space Complexity:** `O(N)`
  In the worst-case scenario (e.g., all opening brackets like `"((((("`), we would push every character onto the stack, requiring $O(N)$ extra space.

---

## What I Learned

* Introduced to the **Stack** data structure and the LIFO (Last In, First Out) principle.
* Discovered a clever optimization trick: pushing the expected *closing* bracket instead of the opening one, which simplifies the conditional checks later.
* Realized the importance of edge cases, such as the stack being empty when a closing bracket arrives, or the stack not being empty at the end of the loop.

---

## Things to Watch

* Always check `stack.isEmpty()` before popping. If you pop an empty stack, Java throws an `EmptyStackException`.
* Don't forget to return `stack.isEmpty()` at the end instead of `true`. If the string is `"("`, the loop will finish, but the string is invalid.
* Adding a quick `if (s.length() % 2 != 0)` check at the beginning is a great habit to immediately discard impossible cases.

---

## Progress

**Day:** 14 / 365
**Problem:** Valid Parentheses
**Difficulty:** Easy
**Status:** Solved
