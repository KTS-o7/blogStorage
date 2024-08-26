+++
title = 'Problem 3075 Maximize Happiness of Selected Children'
date = 2024-05-09T14:35:42+05:30
draft = false
series = 'leetcode'
tags =['cpp','vector','heap','priority-queue','greedy']
toc = false
+++

# Problem Statement

**Link** - [Problem 3075](https://leetcode.com/problems/maximize-happiness-of-selected-children/description)

## Question

You are given an array `happiness` of length `n`, and a positive integer `k`.

There are `n` children standing in a queue, where the `i`th child has happiness value `happiness[i]`. You want to select `k` children from these `n` children in `k` turns.

In each turn, when you select a child, the **happiness value** of all the children that have **not been selected** till now decreases by `1`. Note that the happiness value **cannot** become negative and gets decremented only if it is positive.

Return the **maximum** sum of the happiness values of the selected children you can achieve by selecting `k` children.

## Example 1

```text
Input: happiness = [1,2,3], k = 2
Output: 4
Explanation: We can pick 2 children in the following way:
- Pick the child with the happiness value == 3. The happiness value of the remaining children becomes [0,1].
- Pick the child with the happiness value == 1. The happiness value of the remaining child becomes [0].
- Note that the happiness value cannot become less than 0.
The sum of the happiness values of the selected children is 3 + 1 = 4.
```

## Example 2

```text
Input: happiness = [1,1,1,1], k = 2
Output: 1
Explanation: We can pick 2 children in the following way:
- Pick any child with the happiness value == 1.
The happiness value of the remaining children becomes [0,0,0].
- Pick the child with the happiness value == 0.
The happiness value of the remaining child becomes [0,0].
The sum of the happiness values of the selected children is 1 + 0 = 1.
```

## Example 3

```text
Input: happiness = [2,3,4,5], k = 1
Output: 5
Explanation: We can pick 1 child in the following way:
- Pick the child with the happiness value == 5.
The happiness value of the remaining children becomes [1,2,3].
The sum of the happiness values of the selected children is 5.
```

## Constraints

- `1 <= n == happiness.length <= 2 * 10^5`
- `1 <= happiness[i] <= 10^8`
- `1 <= k <= n`

## Solution

```cpp
class Solution {
public:
    long long maximumHappinessSum(vector<int>& happiness, int k) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        priority_queue<int>heap;

        for(const int it:happiness)
            heap.push(it);
        long long count = 0;
        long long answer = 0;
        while(k>0 && !heap.empty())
        {
            answer = ((heap.top()-count)>=0)?answer+heap.top()-count:answer+0;
            heap.pop();
            k--;
            count++;
        }

        return answer;

    }
};
```

## Complexity Analysis

- `Time`: O(nlogn)
- `Space`: O(n)

## Explanation

### 1. Intuition

- Since the order of selection does not matter, we can select the children with the highest happiness values first.
- this greedy approach will ensure that we maximize the sum of happiness values of the selected children.
- For this we need the array in sorted order, so we use a max heap to store the happiness values of the children.

### 2. Implementation

- We use a max heap to store the happiness values of the children.
- We iterate over the heap and select the children with the highest happiness values.
- We keep track of the `count` to ensure that the happiness value of the children that have not been selected decreases by `1`.
- We will check if the difference between the happiness value and the count is greater than or equal to `0`, if yes we add the difference to the answer, else we add `0`.
- This makes sure that the happiness value does not become negative.
- We return the answer.

---

## Alternate Approach

- We can use the sorted vector to store the happiness values of the children.

```cpp
class Solution {
public:
    long long maximumHappinessSum(vector<int>& happiness, int k) {
        ios_base::sync_with_stdio(0), cin.tie(0), cout.tie(0);
        sort(happiness.begin(), happiness.end(), greater<int>());
        long long sum = 0;
        for(int i = 0; i < k; i++) {
            happiness[i] = max(0, happiness[i]-i);
            sum += happiness[i];
        }
        return sum;
    }
};
```

This will be faster than the heap approach, as we do not need to maintain the heap.

---

> This problem demonstrates the use of a max heap to solve a greedy problem.
