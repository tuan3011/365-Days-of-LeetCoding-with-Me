# Day 15 – Min Stack

> LeetCode #155 – Medium – Stack, Design

🔗 [LeetCode Problem](https://leetcode.com/problems/min-stack/)

---

## Problem

Design a stack that supports `push`, `pop`, `top`, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
* `MinStack()` initializes the stack object.
* `void push(int val)` pushes the element `val` onto the stack.
* `void pop()` removes the element on the top of the stack.
* `int top()` gets the top element of the stack.
* `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

---

## Key Insight

The tricky part is maintaining `O(1)` for `getMin()`. If we only keep a single `min` variable, we lose the previous minimum when the current minimum is popped (because a Stack is LIFO). 
To solve this, we need a way to store the "history" of minimums. This can be achieved in two elegant ways:
1. **Two Stacks**: One stack for values, and a parallel stack just to store the minimum value at that state.
2. **Linked List (Custom Node)**: Embed the `min` value directly into the Node itself, so every element implicitly remembers what the global minimum was at the time it was inserted.

---

## My Solution (Two Stacks)

This approach uses an auxiliary stack (`minStack`) to store the minimums. To optimize memory, we only push to `minStack` if the incoming value is less than or equal to the current minimum. When popping, we only pop from `minStack` if the value being popped from the main stack matches the top of `minStack`.

```java
class MinStack {
    Stack<Integer> s;
    Stack<Integer> minStack;

    public MinStack() {
        s = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int value) {
        s.push(value);
        // Only push to minStack if it's smaller or equal to the current min
        if (minStack.isEmpty()) {
            minStack.push(value);
        } else if (value <= minStack.peek()) {
            minStack.push(value);
        }
    }

    public void pop() {
        // Use .equals() because we are comparing Integer objects
        if (s.pop().equals(minStack.peek())) {
            minStack.pop();
        }
    }

    public int top() {
        return s.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```

## Bonus Solution (Linked List - 0ms)

After solving the problem with two stacks, I explored other submissions and found this extremely fast and elegant solution from another user. Instead of maintaining two stacks, it designs the stack from scratch using a Linked List where each `Node` explicitly stores the `min` value at the time it was created. I included it here because it's a fantastic example of Object-Oriented Design.

```java
class MinStack {
    private Node head;

    public void push(int value) {
        if(head == null)
            head = new Node(value, value, null);
        else
            head = new Node(value, Math.min(value, head.min), head);
    }
    
    public void pop() {
        head = head.next;
    }
    
    public int top() {
        return head.val;
    }
    
    public int getMin() {
        return head.min;
    }

    private class Node {
        int val;
        int min;
        Node next;

        private Node(int val, int min, Node next) {
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }
}
```

---

## Complexity

* **Time Complexity:** `O(1)` for all operations in both approaches.
* **Space Complexity:** `O(N)` for both approaches, as we need to store data for all $N$ elements pushed. The Linked List approach has slightly more overhead per element (object wrapper + pointers) but avoids the overhead of resizing dynamic arrays inside `java.util.Stack`.

---

## What I Learned

* How to design a custom data structure wrapper.
* Using a parallel Stack to store "history" (state management).
* Realized the importance of using `.equals()` when comparing wrapper classes like `Integer` in Java instead of `==`.
* Looking at other people's solutions (like the Linked List approach) is a fantastic way to learn cleaner Object-Oriented implementations.

---

## Things to Watch

* When optimizing the parallel stack by only pushing `<=`, be very careful to use `<=`, not `<`. If you push duplicate minimums, you must store them multiple times so `pop()` doesn't remove the only record.
* Be careful with variable scope in Java. Don't declare variables inside the constructor if you need them at the class level!

---

## Progress

**Day:** 15 / 365
**Problem:** Min Stack
**Difficulty:** Medium
**Status:** Solved
