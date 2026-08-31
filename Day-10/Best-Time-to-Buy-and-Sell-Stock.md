# Day 10 — Best Time to Buy and Sell Stock

> LeetCode #121 · Easy · Array, Dynamic Programming

🔗 [LeetCode Problem](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

---

## Problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

---

## Key Insight

This problem is a classic introduction to the **Sliding Window** technique. At first glance, one might think of using nested loops to compare every possible buy and sell day, but that would result in a $O(N^2)$ Time Limit Exceeded error. Instead, we only need to iterate through the array once. The goal is to always buy at the lowest possible price we've seen so far and sell at the highest peak that comes *after* it.

---

## Approach

1. We use a sliding window defined by two pointers: `left` (Buy Day) and `right` (Sell Day). We start with `left = 0` and `right = 1`.
2. We continuously expand our window by moving the `right` pointer to the future (`right++`).
3. At each step, we check the profitability:
   - **If Profitable (`prices[left] < prices[right]`):** We calculate the current profit and update our `max` profit record using `Math.max()`.
   - **If Not Profitable (`prices[left] >= prices[right]`):** This means we found a day where the stock price is even cheaper (or equal) to our current buy day. We immediately shift our `left` pointer to this new `right` day (`left = right`), establishing a new minimum price to buy from.
4. Once the `right` pointer finishes scanning the array, we return the max profit recorded.

---

## My Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int left = 0;
        int right = 1;
        int max = 0;
        
        while (right < prices.length) {
            // If it's a profitable transaction
            if (prices[left] < prices[right]) {
                max = Math.max(prices[right] - prices[left], max);
            } 
            // If we found a new lower price, shift the buy day
            else {
                left = right;
            }
            // Always move forward in time
            right++;
        }
        return max;    
    }
}

```

## Complexity

* **Time:** `O(N)`
(We only traverse the `prices` array exactly once. The `right` pointer does all the scanning in a single pass).
* **Space:** `O(1)`
(We only use a few integer variables (`left`, `right`, `max`) to keep track of indices and profit, utilizing no extra memory).

---

## What I Learned

* Used the sliding window/two pointers (or state machine) concept simplified down to just tracking the minimum value seen so far.
* Optimized brute-force O(N^2) to an elegant O(N) pass by realizing we only ever care about the absolute lowest price before the current day.

---

## Things to Watch

* Make sure to update the `minPrice` *before* attempting to calculate the `maxProfit` for the current day.
* Initialize `minPrice` to a very large number (e.g., `Integer.MAX_VALUE`) to guarantee the first element will overwrite it.

---

## Progress

**Day:** 10 / 365
**Problem:** Best Time to Buy and Sell Stock
**Difficulty:** Easy
**Status:** Solved
