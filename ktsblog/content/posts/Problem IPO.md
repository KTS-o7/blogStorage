+++
title = 'Problem IPO'
date = 2024-06-15T10:35:35+05:30
draft = false
series = 'leetcode'
tags =['greedy','max-heap','array','min-heap']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 502](https://leetcode.com/problems/ipo/description/)

## Question

Suppose LeetCode will start its **IPO** soon. In order to sell a good price of its shares to Venture Capital, LeetCode would like to work on some projects to increase its capital before the **IPO**. Since it has limited resources, it can only finish **at most** `k` distinct projects before the **IPO**. Help LeetCode design the best way to maximize its total capital after finishing **at most** `k` distinct projects.

You are given `n` projects where the `ith` project has a pure profit `profits[i]` and a minimum capital of `capital[i]` is needed to start it.

Initially, you have w capital. When you finish a project, you will obtain its pure profit and the profit will be added to your total capital.

Pick a list of **at most** `k` distinct projects from given projects to maximize your final capital, and return the final maximized capital.

The answer is guaranteed to fit in a 32-bit signed integer.

### Note

- This question says Pure profit, which means the capital is not getting deducted for performing the project.
- if we are able to perform a project , we will get the profit directly added to current capital.
- We can perform a project only once.

## Example 1

```text
Input: k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]
Output: 4
Explanation: Since your initial capital is 0, you can only start the project indexed 0.
After finishing it you will obtain profit 1 and your capital becomes 1.
With capital 1, you can either start the project indexed 1 or the project indexed 2.
Since you can choose at most 2 projects, you need to finish the project indexed 2 to get the maximum capital.
Therefore, output the final maximized capital, which is 0 + 1 + 3 = 4.
```

## Example 2

```text
Input : k = 4, w = 0, profits = [1,2,3,4], capital = [0,1,2,3]
Output : 10
Explanation : Since your initial capital is 0, you can only start the project indexed 0.
After finishing it you will obtain profit 1 and your capital becomes 1.
With capital 1 , we can start the project indexed 1, now capital becomes 3.
With capital 3, we can start the project indexed 2, now capital becomes 6.
With capital 6, we can start the project indexed 3, now capital becomes 10.
```

## Constraints

```markdown
- `1 <= k <= 10^5`
- `0 <= w <= 10^9`
- `n == profits.length`
- `n == capital.length`
- `1 <= n <= 10^5`
- `0 <= profits[i] <= 10^4`
- `0 <= capital[i] <= 10^9`
```

## Solution

```cpp
class Solution {
public:
    int findMaximizedCapital(int k, int w, vector<int>& profits, vector<int>& capital) {
        std::ios::sync_with_stdio(false);
        vector<pair<int,int>>job;
        for(int i = 0; i<profits.size();i++)
            job.push_back({capital[i],profits[i]});
        sort(job.begin(),job.end());
        priority_queue<int> maxProfit;
        int i = 0;
        while(k>0)
        {
            while(i<profits.size() && job[i].first<=w)
            {
                maxProfit.push(job[i].second);
                i++;
            }
            if(maxProfit.empty())
                break;

            w+=maxProfit.top();
            maxProfit.pop();
            k--;
        }
        return w;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- We need to find the maximum possible profit for a given capital.
- For this if we pair the capital and profit and sort them based on capital.
- We can then iterate over the projects and check if the capital is less than or equal to the current capital.
- If yes then we can add the profit to the max heap.
- We can then pop the top element from the max heap and add it to the capital.
- We can repeat this process until we have performed k projects.
```

### 2. Implementation

```markdown
- Pair the capital and profit and sort them, store them in a vector of pairs called `job`.
- Initialize a max heap called `maxProfit`.
- Initialize a variable `i` to 0.
- Iterate over the projects until `k` becomes 0.
- Check if the capital is less than or equal to the current capital
  and `i` is less than the size of the projects.
  - If yes then add the profit to the `maxProfit` heap.
  - Increment `i`.
  - This will add all the projects which can be performed with the current capital.
- If the `maxProfit` heap is empty then break.
- Add the top element of the `maxProfit` heap to the capital.
- Pop the top element from the `maxProfit` heap.
- Decrement `k`.
- Return the final capital.
```

> This is a greedy approach where we are trying to maximize the profit by selecting the projects which can be performed with the current capital.
