+++
title = 'Problem 121 Best Time to Buy and Sell Stock'
date = 2024-09-21T21:39:53+05:30
draft = false
series = 'leetcode'
tags =['dp','dynamic-programming','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

## Question

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

## Example 1

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6),
profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed ,
because you must buy before you sell.
```

## Example 2

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

## Constraints

- 1 <= prices.length <= 10<sup>5</sup>
- 0 <= prices[i] <= 10<sup>4</sup>

## Solution

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        int n = prices.size();
        int buyAtPrice = INT_MAX;
        int maxProfit = 0;
        for(int i=0; i<n; i++){
            buyAtPrice = min(buyAtPrice, prices[i]);
            maxProfit = max(maxProfit, prices[i] - buyAtPrice);
        }
        return maxProfit;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Dp        | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

- Objectives are to maximize the profit by buying and selling such that the difference between the selling price and buying price is maximum.
- for this to be to true, we need to find the relative differences towards the left to the right.
- So, if we arrive at a stock, we have to check if the current stock price is the minimum stock price we have seen so far.
- If it is, then we update the minimum stock price, but we can only sell towards right and not earlier.
- So every step we need to determine if we need to buy the stock now by comparing the current price to the lowest price we have seen so far.
- And profit is the difference between the current price and the lowest price we have seen so far.
- We need to maximize this profit.

### 2. Implementation

```markdown
- Initialize two variables `buyAtPrice` and `maxProfit` to `INT_MAX` and `0` respectively.
- For every `price` in `prices`:
  - Update `buyAtPrice` to the minimum of `buyAtPrice` and `price`.
  - Update `maxProfit` to the maximum of `maxProfit` and `price - buyAtPrice`.
- Return `maxProfit`.
```

> There are various follow ups, do check it out in leetcode.
