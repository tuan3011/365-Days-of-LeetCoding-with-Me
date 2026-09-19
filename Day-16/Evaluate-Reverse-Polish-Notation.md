# Day 16 – Evaluate Reverse Polish Notation

> LeetCode #150 – Medium – Array, Math, Stack

🔗 [LeetCode Problem](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

---

## Problem

You are given an array of strings `tokens` that represents an arithmetic expression in a Reverse Polish Notation (RPN).
Evaluate the expression. Return an integer that represents the value of the expression.

**Note:**
* The valid operators are `+`, `-`, `*`, and `/`.
* Each operand may be an integer or another expression.
* The division between two integers always truncates toward zero.
* There will not be any division by zero.

---

## Key Insight

Reverse Polish Notation (postfix notation) is mathematically brilliant because it removes the need for parentheses and operator precedence (like "multiply before add"). The operator always appears precisely when it is time to evaluate the two operands immediately preceding it.
This LIFO (Last In, First Out) behavior makes the **Stack** the perfect data structure. Whenever we encounter an operator, the two numbers we need are sitting right at the top of the stack.

---

## Approach

1. Initialize a `Stack<Integer>` to hold the numbers.
2. Iterate through each `token` in the array.
3. If the token is a number, parse it to an integer and `push()` it onto the stack.
4. If the token is an operator (`+`, `-`, `*`, `/`):
   - `pop()` the top two numbers from the stack.
   - **Crucial Detail:** The first number popped is the second operand (`b`), and the second number popped is the first operand (`a`). This order strictly matters for subtraction (`a - b`) and division (`a / b`).
   - Perform the operation and `push()` the result back onto the stack so it can be used by subsequent operators.
5. After parsing the entire expression, the stack will contain exactly one element, which is the final evaluated result.

---

## My Solution

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        
        for (String token : tokens) {
            if (token.equals("+")) {
                stack.push(stack.pop() + stack.pop());
            } else if (token.equals("-")) {
                int b = stack.pop();
                int a = stack.pop();
                stack.push(a - b);
            } else if (token.equals("*")) {
                stack.push(stack.pop() * stack.pop());
            } else if (token.equals("/")) {
                int b = stack.pop();
                int a = stack.pop();
                stack.push(a / b);
            } else {
                stack.push(Integer.parseInt(token));
            }
        }
        
        return stack.pop();
    }
}
```

---

## Complexity

* **Time Complexity:** `O(N)`
  We iterate through the array of `N` tokens exactly once. Each token is either pushed or popped from the stack in $O(1)$ time. Overall time is linear.
* **Space Complexity:** `O(N)`
  In the worst-case scenario (an expression with all numbers and no operators initially, e.g., before reaching the operators at the end), we would push at most $O(N)$ elements onto the stack.

---

## What I Learned

* Cemented the practical use of a Stack for expression parsing and evaluation.
* Realized that RPN completely eliminates the need for an "operator stack" because calculations happen instantly as the operator is read.
* Understood the importance of pushing intermediate results back into the stack to chain calculations.

---

## Things to Watch

* **Order of Operands:** For non-commutative operations (`-` and `/`), popping order matters. `a = stack.pop()` and `b = stack.pop()` means the operation is `b - a`, not `a - b`. The first popped element is the right operand.
* **String Comparison in Java:** Always use `.equals("+")` instead of `==` or comparing with `char` `'+'` when dealing with String tokens.

---

## Progress

**Day:** 16 / 365
**Problem:** Evaluate Reverse Polish Notation
**Difficulty:** Medium
**Status:** Solved
