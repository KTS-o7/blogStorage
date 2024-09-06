+++
title = 'Problem 2028 Find Missing Observations'
date = 2024-09-06T10:54:36+05:30
draft = false
series = 'leetcode'
tags =[]
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2028](https://leetcode.com/problems/find-missing-observations/description/)

## Question

You have observations of `n + m` 6-sided dice rolls with each face numbered from `1` to `6`. `n` of the observations went missing, and you only have the observations of `m` rolls. Fortunately, you have also calculated the average value of the `n + m` rolls.

You are given an integer array `rolls` of length `m` where `rolls[i]` is the value of the `i`<sup>th</sup> observation. You are also given the two integers `mean` and `n`.

Return an array of length `n` containing the missing observations such that the average value of the `n + m` rolls is exactly `mean`. If there are multiple valid answers, return any of them. If no such array exists, return an empty array.

The average value of a set of `k` numbers is the sum of the numbers divided by `k`.

Note that mean is an integer, so the sum of the `n + m` rolls should be divisible by `n + m`.

## Example 1

```
Input: rolls = [3,2,4,3], mean = 4, n = 2
Output: [6,6]
Explanation: The mean of all n + m rolls is
 (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.
```

## Example 2

```
Input: rolls = [1,5,6], mean = 3, n = 4
Output: [2,3,2,2]
Explanation: The mean of all n + m rolls is
 (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.
```

## Example 3

```
Input: rolls = [1,2,3,4], mean = 6, n = 4
Output: []
Explanation: It is impossible for the mean to be 6 no matter what the 4 missing rolls are.
```

## Constraints

- `m == rolls.length`
- 1 <= `n`, `m` <= 10<sup>5</sup>
- 1 <= `rolls[i]`, `mean` <= 6

## Solution

```cpp
class Solution {
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int tot = 0;
        for (int i = 0; i < rolls.size(); i++) {
            tot += rolls[i];
        }

        int sum = mean * (n + rolls.size()) - tot;
        if (sum > 6 * n or sum < n) {
            return {};
        }
        int divide = sum / n;
        int rem = sum % n;
        vector<int> ans(n, divide);
        for (int i = 0; i < rem; i++) {
            ans[i]++;
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm    | Time Complexity | Space Complexity |
| ------------ | --------------- | ---------------- |
| Accumulation | O(m)            | O(1)             |
| Division     | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

Let's try to breakdown the meaning of the problem.
We are given `m` die rolls and a number `mean` which is the supposed average of `n+m` die rolls. We are given `n` also.
Now we need to find the missing `m` entries such that the average of all `n+m` die rolls is equal to `mean`.

{{< katex >}}

$$
mean = \frac{sum\ of\ all\ rolls}{n+m} = \frac{sum\ of\ m\ rolls + sum\ of\ n\ rolls}{n+m}
$$

$$
sum\ of\ n\ rolls = mean \times (n+m) - sum\ of\ m\ rolls
$$

- Once we get the sum of `n` rolls, we can divide it by `n` to get the lower bound of the each die roll.
- We can then find the remainder and distribute it among the `n` rolls.
- But when does the solution not exist?
  - If the sum of `n` rolls is greater than `6*n` or less than `n`, then the solution does not exist.
  - This is because the die rolls are from `1` to `6` and the sum of `n` rolls should be between `n` and `6*n`.

### 2. Implementation

```markdown
- Initialize a variable `tot` to store the sum of `m` rolls.
- Initialize a variable `sum` to store the sum of `n` rolls.
- Iterate over the `rolls` array and accumulate the sum of `m` rolls.
- Calculate the sum of `n` rolls using the formula `mean * (n + rolls.size()) - tot`.
- If the sum of `n` rolls is greater than `6*n` or less than `n`, return an empty array.
- Calculate the lower bound of each die roll using the formula `sum / n`.
- Calculate the remainder using the formula `sum % n`.
- Initialize a vector `ans` of size `n` and fill it with the lower bound of each die roll.
- Distribute the remainder among the `n` rolls.
- Return the `ans` vector.
```
